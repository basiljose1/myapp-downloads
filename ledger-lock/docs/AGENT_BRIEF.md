# Ledgerlock — engineering brief (read first)

Ledgerlock is an Android-only, zero-network expense tracker for India. It reads bank SMS (and later
payment-app notifications), parses them on-device, reconciles transfers/card bills so they never
inflate spend or income, and shows dashboards. Solo developer, Kotlin + Jetpack Compose.

The product spec is the PDF "Offline Indian Expense Tracker — Product Specification" in the claude.ai
Project "Ledgerlock" (read it with the Projects tool, `project_read`, if you have it). Feature IDs
like "2.3" below refer to it.

## Core design rule

Every SMS/notification becomes an immutable **raw event**; only reconciled events become **ledger
entries** (`txn`). Dashboards read only the ledger and its aggregates. Every `txn` has one
`FlowType`; metrics are defined only by flow type:

- Net Spend = Σ EXPENSE + Σ LOAN_EMI − Σ REFUND
- Core Income = Σ INCOME
- True Savings = Core Income − Net Spend
- SELF_TRANSFER, CC_BILL_PAYMENT, INVESTMENT: excluded from all three ("money in motion");
  INVESTMENT tracked separately as "invested". PENDING_REVIEW: held out of everything.
- Card swipes count as EXPENSE on swipe date; the bank debit paying the card bill is
  CC_BILL_PAYMENT (both legs excluded), so each rupee counts once.

## Repo layout (Gradle, Kotlin DSL, version catalog `gradle/libs.versions.toml`)

| Module | Type | Owns |
| --- | --- | --- |
| `:core:model` | Kotlin/JVM | `Paise`, `FlowType`, `Direction`, `Channel`, `Instrument` |
| `:core:parser` | Kotlin/JVM | `SmsParser.parse(address, body)` → `ParseResult(verdict, sender, institution, record, statement)`; `SmsParser.VERSION` stamps records; `Gate`, `Extractor`, `Dates`, `Statements`, `SenderRegistry` |
| `:core:reconcile` | Kotlin/JVM | Reconciliation engine (spec §2): pure functions returning decisions |
| `:data` | Android lib | SQLCipher DB, schema, repositories |
| `:ingest` | Android lib | SMS receiver, inbox backfill, WorkManager pipeline |
| `:app` | Android app | Compose UI |

More pure modules may be added under `core/` (e.g. `core/insights`, `core/rules`, `core/crypto`).
When you add one: create `core/<name>/build.gradle.kts` copying `core/reconcile/build.gradle.kts`,
add it to `settings.gradle.kts`, and append it to `scripts/jvm-modules.txt` (dependency order).

## Hard rules

- Money is always `Paise` (Long paise). Never Double for money.
- No network, ever. Never add `android.permission.INTERNET` or any network/analytics/crash SDK.
- Privacy: never commit real SMS. Golden test cases use anonymised text (fake names, accounts,
  VPAs, reference numbers). The real corpus lives only on the user's machine.
- Pure logic goes in `core/*` Kotlin/JVM modules with JUnit 5 tests. Android modules stay thin.
- Smallest complete change: no speculative options, wrappers or "for later" code. A known limit
  gets a comment `// shortcut: <limit>, <when to upgrade>`.
- Keep the existing structure and naming; read the code you touch first.
- Every non-trivial piece of logic gets a test.

## Verify (this sandbox has NO internet; Gradle cannot run here)

Pure-Kotlin modules compile and test offline with:

```bash
/opt/ktest/verify.sh <repo-root>        # compiles modules in scripts/jvm-modules.txt, runs tests
```

It uses kotlinc 2.4 and a minimal JUnit 5 stand-in (`org.junit.jupiter.api`: `@Test`,
`@TestFactory`, `@BeforeEach`, `@AfterEach`, `@Disabled`, `DynamicTest.dynamicTest`, `Assertions.*`
incl. `assertThrows`, `Assumptions.assumeTrue`). Stick to those JUnit APIs. Only kotlin-stdlib is on
the classpath: no kotlinx-coroutines, no kotlinx-serialization, no third-party libs in `core/*`.
`VERIFY STATUS: 0` is required before you finish.

**Android modules (`:data`, `:ingest`, `:app`) cannot be compiled here** (no Android SDK, no
AndroidX). Write them conservatively: standard, stable APIs only, explicit imports, no clever
generics. Any new Android dependency goes in `gradle/libs.versions.toml` with a widely-released
stable version and a comment `# verify version on first sync`. SQL schema/queries: validate them with
Python's `sqlite3` (`python3 -c ...`) by executing the DDL and representative queries against an
in-memory DB.

## Current Android dependency choices

- Database: SQLCipher for Android (`net.zetetic:sqlcipher-android`) used directly via
  `net.zetetic.database.sqlcipher.SQLiteDatabase` / `SQLiteOpenHelper` with hand-written SQL
  (no Room/KSP: keeps the build simple and lets SQL be validated offline). DB key: random 32 bytes,
  wrapped by an Android Keystore AES key, unlocked via `androidx.biometric` BiometricPrompt.
- Background work: `androidx.work:work-runtime-ktx`.
- UI: Compose Material 3 (BOM in catalog), Navigation Compose. Charts drawn with Compose `Canvas`
  (no chart library).
- Billing: `com.android.billingclient:billing-ktx` (app/Billing.kt). The library declares INTERNET, but the
  app manifest strips it (`tools:node="remove"`); billing still works because the library talks to the Play
  Store app over IPC (AIDL), and Play makes the network calls. Purchases are signature-checked locally with
  `core/entitlement` PlaySignature (key from gradle property `ledgerlock.playLicenseKey` → BuildConfig).
- Phase 5 tables: `data/Phase5Schema.kt` (`PHASE5_DDL`, DB migration version 3), queries in
  `BudgetRepository.kt`, validated by `scripts/validate_phase5_sql.py`. Pure app logic lives in
  `app/.../logic/` (no Android imports) and is tested offline by `scripts/verify-app-logic.sh`.

## Data model (MVP tables; amounts Long paise, times epoch millis)

raw_event(id, channel, sender, body, device_ts, source_key, content_hash UNIQUE, status)
parsed_record(id, raw_event_id, institution, amount_paise, currency, direction, account_last4,
  instrument, counterparty, rrn, balance_paise, txn_ts, source, confidence, parser_version)
account(id, institution, instrument, last4, label, is_own, kind; UNIQUE(institution, instrument, last4))
balance_snapshot(account_id, amount_paise, as_of)
own_identity(id, type, value, account_id, scope)
txn(id, account_id, amount_paise, direction, txn_ts, flow_type, category_id, merchant, status,
  transfer_group_id, user_override, primary_record_id)
txn_source(txn_id, parsed_record_id, role)
classification_log(txn_id, from_flow, to_flow, rule_id, tier, counterpart_txn_id, ts, undone)
review_item(id, kind, ref_id, created_at, resolved_at, resolution)
rule(id, priority, condition_json, action_json, stop, enabled)
agg_daily / agg_monthly (account_id, category, flow_type, day|month, total_paise, count) via triggers

## Design (UI)

Phone-first, light theme first. Palette: paper #F7F6F2 background, ink #15171C, muted #5B616B,
line #E4E2DC, teal accent #0F6E5F (tint #E3F0EC), amber review #6B4100 on #FBEFD9. Indian number
formatting (₹1,23,456) via `Paise.format()`. Bottom nav: Home · Transactions · Analytics · Accounts
(Budgets tab appears in Phase 5). Screens: Onboarding (promise → SMS permission → import history →
confirm own accounts → app lock), Home (net spend hero + core income + true savings + "N unverified"
chip, money in motion card, recent transactions, accounts strip), Transactions (search, filter chips,
excluded items greyed with badge), Transaction detail (both legs + "why it's not counted" + Undo),
Review inbox (one card at a time: confirm / other / ignore), Accounts (balances with as-of age,
card outstanding/limit, balance-auditor warning). Accessible: 44dp touch targets, content
descriptions on icon buttons, colour never the only signal.

## Real corpus (never commit, never copy into the repo)

The user's real SMS (business senders only, 23.6k lines, TSV `date \t sender \t body` with `\n`/`\t`
escaped) is at `/home/claude/corpus/sms.tsv`. Run the corpus coverage test with:

```bash
LEDGERLOCK_CORPUS=/home/claude/corpus/sms.tsv /opt/ktest/verify.sh <repo-root>
```

Use it to tune rules and to find formats; any example you put in a test must be anonymised.
