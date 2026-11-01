# LedgerLock UX/UI Audit

Status: **draft for approval.** No code has been changed. Implementation waits for sign-off on the direction, navigation structure, and per-screen changes below.

Scope: `app/src/main/kotlin/com/basiljose/ledgerlock/ui/` (Compose screens, theme, navigation) and `app/src/main/res/`. Out of scope: business logic (`logic/`), data layer, API contracts, DB schema, signing configs, `.env`/secrets, `gradle.properties`.

## 1. Inventory

Navigation is one `NavHost` in `AppRoot.kt`. Five bottom-nav tabs, the rest are pushed routes.

| Layer | Destinations |
|---|---|
| Entry | Onboarding, Lock (BiometricPrompt), Lost-key recovery |
| Tabs (5) | Home `home`, Transactions `txns`, Budgets `budgets`, Insights `analytics` (tab label "Insights"), More `more` |
| Pushed from tabs | Accounts, Account detail, Subscriptions, Lending, Lending detail, Calendar, Financial insights `insights`, Reports, Report, Review (incl. held window), Settings, Paywall, About, Motion, Manage accounts, Categories, Transaction detail, Statement import, Category drill-down |
| Dialogs and sheets | Add transaction (`CashEntryDialog`), `FullScreenForm`, amount keypad |

Theme: `ui/theme/` has `Color.kt`, `Theme.kt`, `Type.kt` (Bricolage Grotesque), and `Shape.kt`. Dark, light, and Material You (Android 12+) are all implemented in `LedgerlockTheme`.

## 2. Findings

Each finding is backed by a grep or file read. Counts are from the current tree.

### Design system

| # | Finding | Evidence | Severity |
|---|---|---|---|
| D1 | Hardcoded colour literals outside the theme. Analytics pie colours are inline `Color(0x…)` values. | `AnalyticsScreen.kt:301-302` (23 `Color(0x` hits outside `theme/`, 2 files) | High |
| D2 | Legacy names contradict their values. `Teal`/`InkTeal` hold indigo, `NightTeal` is indigo, and `Amber` means marigold. Future work will keep misreading the palette. | `Color.kt:12-20`, `Theme.kt` | Medium |
| D3 | Inline strings are everywhere. About 350 `Text("…")` literals exist in `ui/`. `strings.xml` has only 3 lines. Examples: lock screen copy in `AppRoot.kt`, "Add transaction" in `AddTransaction.kt`. | `grep Text("` | High |
| D4 | No `@Preview` anywhere in the repo. Light, dark, and large-font previews do not exist. | `grep @Preview` → 0 | High |
| D5 | No WindowSizeClass support. Layouts assume compact phones. The `material3-window-size-class` artifact is not in `app/build.gradle.kts`, so adding it is a dependency decision (see §5). | `grep WindowSizeClass` → 0; build file | High |
| D6 | Typography and shape tokens are good. `CredTypography` and `CredShapes` exist, but screens also set raw `fontSize = 12.sp` and similar. | `AnalyticsScreen.kt:355` and others | Low |

### Accessibility

| # | Finding | Evidence | Severity |
|---|---|---|---|
| A1 | 16 `Icon(... contentDescription = null)` calls. Some are decorative next to text, but some are actionable (the Backspace key in `AmountKeypad`, the search and filter icons in `TransactionsScreen`). Each needs a review pass. | `grep contentDescription = null` | Medium |
| A2 | Touch targets are mostly fine (`heightIn(min = 48.dp)` is common). Only 2 explicit sub-48dp sizes were found, and both are icons inside larger tappable rows. Still needs a per-control check. | grep `size(3x.dp)` | Low |
| A3 | Contrast needs measurement. Many text roles use `InkMuted` (`onSurfaceVariant`, alpha 0.60) and `Text55`/`Text30` alphas. These may fail WCAG AA on `PaperBand` and `BandLow`. | `Color.kt` alpha values | Medium (unverified) |
| A4 | Live regions are used on the lock error. Other async results (saves, errors) are not announced. | `AppRoot.kt` `liveRegion` | Low |

### Information architecture

| # | Finding | Evidence | Severity |
|---|---|---|---|
| I1 | Two overlapping analytics screens. The "Insights" tab opens `AnalyticsScreen`, which navigates to `insights` (`InsightsScreen`, labelled "Financial insights" under More). The same concept lives in two places with two names. | `AppRoot.kt:472,478`; `MoreScreen.kt:67`; `AnalyticsScreen.kt:167` | High |
| I2 | Tab label and route disagree. Tab "Insights" uses route `analytics`. | `AppRoot.kt:335` | Low |
| I3 | `AppRoot.kt` is 544 lines and mixes the lock, onboarding, recovery, nav graph, and back-stack logic. The nav graph should be its own file. | line count | Medium |
| I4 | Primary actions are fast. Home has Debit/Credit quick actions (1 tap to the form). Transactions and Accounts add flows are reachable in 2 taps. | `HomeScreen.kt:250-257` | OK |
| I5 | Feature sprawl under More. Accounts, Subscriptions, Lending, Calendar, Reports, Review, Categories, Motion, and About all sit under More with no grouping. | `MoreScreen.kt` | Medium |

### States (loading, empty, error)

| # | Finding | Evidence | Severity |
|---|---|---|---|
| S1 | No loading state on data screens. Zero `CircularProgressIndicator` in `ui/`. Lists pop in with no placeholder. | grep → 0 | High |
| S2 | Empty states are inconsistent. Some screens say "No spending in this period." as bare text. Others have no empty state. | `AnalyticsScreen.kt:206-243` | Medium |
| S3 | Errors are inconsistent. Some use inline text, some use `Toast`, some use an `AlertDialog`. There is no shared error pattern. | `AppRoot.kt` (Toast, AlertDialog) | Medium |

### Forms

| # | Finding | Evidence | Severity |
|---|---|---|---|
| F1 | Keyboard types are set in 21 places, so the basics are present. Inline validation is not consistent: `FullScreenForm` disables Save (`saveEnabled`) instead of explaining what is wrong. | `LendingScreen.kt:422`, `SubscriptionsScreen.kt:200` | Medium |
| F2 | Error copy is generic in places, e.g. "Couldn't open your data. Try again with your screen lock PIN or pattern." That one is fine. Field-level errors mostly do not exist. | `AppRoot.kt` unlock | Medium |

## 3. Proposed changes

Each change keeps current behaviour. Nothing changes business logic, data, or schema.

### 3.1 Design system (no navigation impact)
1. Create `ui/theme/Tokens.kt` with spacing (4/8dp scale), and a `TextRole` set on top of `CredTypography`. Replace raw `dp`/`sp` in screens.
2. Rename legacy palette aliases (`Teal`→`Brand`, `Amber`→`Accent`, etc.). Mechanical rename, no value change.
3. Move the 23 literal colours into `Color.kt` as named tokens (e.g. `ChartPalette`). Use `Cred`/`MaterialTheme` everywhere else.
4. Move user-visible strings into `res/values/strings.xml`. Start with the lock, onboarding, and Home screens, then the rest.
5. Add `@Preview` for every screen: light, dark, `fontScale = 1.5f`. Preview data comes from fakes, never the real DB.

### 3.2 Accessibility
6. Decide per `contentDescription = null` icon: decorative stays null, actionable gets a label string.
7. Measure contrast of `InkMuted`, `Text55`, `Text30` against their backgrounds. Raise alpha where AA fails (4.5:1 body, 3:1 large).
8. Add `Modifier.semantics` roles to custom-drawn controls (`AmountKeypad`, `NeuButton` variants).

### 3.3 Information architecture (**needs your approval, changes navigation**)
9. **Merge Insights and Analytics** into one tab, "Insights", with Financial insights as a section. Removes the duplicate entry (I1). Route `analytics` becomes the single entry, `insights` becomes an inner section or is removed.
10. **Group More** into sections: "Money" (Accounts, Subscriptions, Lending, Calendar), "Reports" (Reports, Review), "Setup" (Categories, Manage accounts, Settings), "About". Same destinations, clearer grouping.
11. Split `AppRoot.kt` into `NavGraph.kt`, `LockFlow.kt`, `Onboarding` host. Behaviour unchanged.

### 3.4 States
12. Shared `LoadingState`, `EmptyState(icon, title, body, action)`, and `ErrorState(message, retry)` composables in `Common.kt` or a new `ui/states/` file. Apply to all data screens.

### 3.5 Forms
13. Inline field error text under each field, shown after first blur or submit. `saveEnabled` stays, but the reason shows next to Save.

### 3.6 Adaptive layout (**needs your approval, adds a dependency**)
14. Add `androidx.compose.material3:material3-window-size-class` to `app/build.gradle.kts` and use `WindowSizeClass` to switch `NavigationBar` to `NavigationRail` on medium and expanded widths. The version must come from the existing BOM.

## 4. Per-screen plan

| Screen | Changes |
|---|---|
| Lock / Onboarding | Strings to resources; previews; keep flow as is. |
| Home | Strings; empty and loading states; quick-action labels reviewed. |
| Transactions | Icon labels (A1); filter sheet semantics; empty and error states. |
| Budgets | Strings; icon labels; empty state. |
| Insights (merged) | Remove duplicate entry (I1); replace pie `Color(0x)` with tokens (D1); empty states. |
| More | Group into sections (item 10). |
| Accounts / Account detail | Strings; add-cash flow inline errors; empty state. |
| Subscriptions / Lending / Calendar | Strings; form inline errors; empty states. |
| Reports / Report / Review | Strings; loading state for report generation. |
| Settings / About / Motion | Strings; section grouping. |
| Paywall | Strings; check contrast of price and CTA. |
| Add transaction (`CashEntryDialog`) | Keyboard types already present; inline validation; semantics on keypad. |

## 5. Decisions for you

1. **Insights merge (item 9):** merge the two analytics screens into one tab? This removes a screen. Recommended.
2. **More grouping (item 10):** approve the section layout?
3. **WindowSizeClass dependency (item 14):** add `material3-window-size-class`? This is an added dependency, so it needs your explicit yes.
4. **Scope of string extraction:** all screens in one PR, or per screen? Recommended: per screen, so each diff stays reviewable.

## 6. Verification plan (after approval)

Not run yet. This is the audit only.

- `./gradlew assembleDebug`
- `./gradlew lint` (baseline first, compare after)
- `./gradlew testDebugUnitTest` (baseline first)
- Previews render in light, dark, and `fontScale = 1.5f`
- Manual check on a compact and an expanded emulator

## 6a. Corrections found during implementation

- **S1 was wrong.** Data screens already had a designed loading state: `SkeletonScreen` (CredComponents.kt), which the original grep for spinners missed. It stays the loading pattern everywhere, and no spinner was added.
- **I5 was mostly wrong.** More was already grouped into three sections with search. The only change is removing the duplicate Financial insights row (item 9).

## 7. Not verified

- Actual contrast ratios (A3 is an estimate from alpha values).
- Whether dynamic colour is exposed in Settings (the code path exists).
- Whether every screen was read in full; this audit used greps and structural reads.
- Build and lint baselines.
