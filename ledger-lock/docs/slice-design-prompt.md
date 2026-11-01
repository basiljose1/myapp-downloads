# Slice-inspired design and animation: prompt for another Android app

Copy everything inside the fenced block below into Claude Code (or any coding agent) opened in the target Android project.
Fill in the brand inputs first. Written for Jetpack Compose + Material 3, minSdk 26+.

````text
Redesign this whole Android app (Jetpack Compose, Material 3) in a clean, soft, rounded style inspired by the Slice
banking app (slice.bank.in), with one brand colour, one type family and one motion language used on EVERY screen.
Make the change through shared theme tokens and shared components first, so screens pick it up automatically, then
sweep each screen for hard-coded leftovers. Work in small steps and compile after each one.

BRAND INPUTS (set these, change nothing else per app)
- Brand primary (light): <<#0F6B6E>>   Brand primary on dark: <<#6FC7C2>> (about 25% lighter)
- Brand tint (container): <<#E3F1F0>>  Text on tint: <<#0A4A4C>>   Dark tint: <<#12494B>>
- Accent / "needs attention": amber #F2A93B (text on light: #8A4B00). Never use it as a second brand colour.

0. BEFORE EDITING
- Read the existing theme (Color/Theme/Type/Shape), the shared components, the nav host, and 3 representative screens.
- Grep for hard-coded colours (Color(0x...), Color.Red/Black/White), RectangleShape, .border(, uppercase(), letterSpacing.
  These are the leftovers you will remove at the end.
- Keep every existing name that call sites use (e.g. Teal, Amber) if renaming would touch dozens of files; change values.

1. COLOUR RULES (one palette, nothing else)
- Light page = white #FFFFFF. Cards = white with a 1dp hairline (ink 8%) and a faint shadow. Pressed/chip fill #EFF4F4.
- Ink #191919. Muted text = ink at 60%. Hairline = ink at 8%. Strong outline = ink at 30%.
- Brand primary is the ONLY brand colour: filled buttons, active tab, switches, progress fills, links, focus.
- Money in = green (#15803D light, #34C77B dark). Money out / neutral amounts = plain ink (white on dark). Not red.
- Amber only for warnings and "needs review". Error = #C0262D (light) / #FF8A80 (dark).
- Dark mode: page #0E1414, card #141C1C, raised #1C2828, white-alpha text ladder (80/70/55/30%), brand-on-dark primary.
- Set the M3 colorScheme fully (primary, primaryContainer, secondaryContainer, surface*, outline, outlineVariant,
  error, tertiary) so default M3 purple/lavender never leaks. Chart palettes use the brand colour first, then blue,
  violet, amber, grey; keep a lighter variant for dark mode.
- No second accent colour, no gradients, no per-screen colours. If a screen needs a new colour, it is wrong.

2. SHAPES
- Shapes(extraSmall = 28dp [text fields and menus: a full pill when one line], small = 16dp, medium = 20dp,
  large = 28dp [cards], extraLarge = 32dp [dialogs, sheets]). Export CardShape = RoundedCornerShape(28.dp).
- Buttons, chips, badges, avatars, nav discs, icon tiles = full pills/circles (CircleShape). Remove every
  `shape = RectangleShape` and every override of a text field's shape, so all inputs follow the theme.
- Progress bars, chart swatches, calendar selection = rounded/circular, never square.

3. TYPOGRAPHY
- Use Bricolage Grotesque (OFL, Google Fonts) as the single family. Bundle the variable font as
  res/font/bricolage_grotesque.ttf (lowercase name) and build one FontFamily from the variable axis:
    @OptIn(ExperimentalTextApi::class)
    private val Bricolage = FontFamily(
        listOf(Normal, Medium, SemiBold, Bold, ExtraBold).map {
            Font(R.font.bricolage_grotesque, it, variationSettings = FontVariation.Settings(FontVariation.weight(it.weight)))
        })
- Scale: display 48/36 and headline 28 = ExtraBold, tight tracking (-0.02em). titleLarge 22 ExtraBold (screen titles,
  left aligned, big, like "Banking" / "Activity"). titleMedium 16 SemiBold (card titles, row titles, buttons).
  titleSmall 14 Bold. body 16/14 Normal. labelLarge 13 SemiBold. labelMedium 12.
- Sentence case everywhere. Delete .uppercase(), wide letterSpacing and all-caps section labels. Section headers are
  bold sentence-case titles. Keep monospace only for codes/keys.
- Big amounts use displayMedium/displayLarge ExtraBold; labels above amounts are muted and small.
- Keep the OFL licence text with the font (e.g. in an About/licences screen or assets).

4. SHARED COMPONENTS (build once, then replace usages)
a) Buttons, in one file, wrapping Material buttons so existing call sites keep compiling (same names, import-shadowing:
   remove the androidx.compose.material3.Button/OutlinedButton/TextButton imports in call sites):
   - Button: filled brand pill, min height 48dp. OutlinedButton: brand-outlined (1.5dp) white pill. TextButton: brand text.
   - ALL three squish on press: graphicsLayer scale 1 -> 0.94 with spring(DampingRatioMediumBouncy, StiffnessMedium),
     plus a light haptic (HapticFeedbackType.TextHandleMove) on touch-down. Pass an interactionSource and read
     collectIsPressedAsState().
   - A big CTA variant: pill, 52dp, label + trailing arrow icon that nudges 4dp right on press.
   - Never colour one button differently (no orange/red special cases); destructive = TextButton in error colour.
b) PremiumCard (and make SectionCard just call it, so every panel is identical):
   fillMaxWidth, softShadow (shadow(6.dp, CardShape, ambient black 6%, spot black 8%)), clip(CardShape),
   background(surface), border(1.dp, hairline, CardShape), padding 20-24dp, spacedBy(12dp), animateContentSize.
   Clickable cards squish to 0.98 and darken to the pressed fill; no ripple.
c) riseIn(): every card fades from alpha 0 and slides up 24dp over 450ms (ease-out), STAGGERED by its y position:
   delay = (positionInWindow().y / 8f).toLong().coerceIn(0, 280) ms measured in onPlaced, so a screen fills top to
   bottom. Runs once per composition. Use Animatable + graphicsLayer, not recomposition.
d) Bottom navigation: no divider, page-coloured bar, SpaceEvenly row of circles. Idle tab = 48dp grey disc with a
   24dp muted icon. SELECTED tab = 60dp brand-filled disc with a 30dp white icon, a soft brand-tinted shadow, and the
   tab name (labelLarge, brand colour) beneath it. Size/icon animate with spring(MediumBouncy, MediumLow). Items are
   at least 88dp tall so the bar never jumps. contentDescription = the label. No ripple.
e) Top app bar: big bold left title, background = page colour, actions are plain icons that animate (see 6).
f) Lists: flat rows on the page (no white strips); round tinted avatar/category tile 44dp; title bold-ish, subtitle
   muted, amount right-aligned; hairlines inset to the text column. Group sections inside ONE card where Slice does.
g) "More"/settings index: one white card per group; rows = round tinted icon (tint container colour) + titleMedium +
   muted detail + chevron; hairline dividers inset 76dp. Search = pill text field above.
h) Text fields: default OutlinedTextField only (the theme shape makes it a pill). Same border/focus colours everywhere.
i) Detail screens: receipt-style header = huge amount (displayMedium), payee (titleMedium), date (muted) on the left,
   a 40dp status disc on the right (green check = fine, amber "!" = needs review).
j) Amount entry (add transaction): a full brand-coloured panel with rounded bottom corners (32dp) holding the title,
   a Spent/Received pill switch, a huge centred amount, and a 3x4 keypad (1-9, ".", 0, backspace) of big headline
   digits on the brand colour; each key squishes to 0.8 on press with a haptic tick. Other fields sit on a white sheet
   below. Keep the keypad rules in one pure function and unit-test it (one dot, max 2 decimals, max 10 chars, no
   leading zeros, backspace on empty is safe).
k) Empty/loading: pulsing rounded skeleton cards (alpha 0.04-0.12) for first load; empty states are an invitation with
   one pill CTA.

5. MOTION LANGUAGE (apply the same everywhere)
- One easing: CubicBezierEasing(0.2, 0.7, 0.2, 1). One duration set: SHORT 150, MEDIUM 300, LONG 450 ms. Put both in
  one file and forbid literals elsewhere.
- Springs for anything touched (press squish, nav disc, icon pops); tweens for entrances and colour changes.
- Page transitions: pushed screens slide in from the right by the FULL width over 450ms with a fade; the page beneath
  drifts left by a quarter width and fades; back is the mirror. Switching between bottom tabs is a quick crossfade
  (90ms out, then in), never a slide.
- Numbers count up on first show; progress bars and chart arcs grow in from empty and glide to new values.
- Respect the system animation scale: use only Compose animation APIs (they follow it). Check the phone's Battery
  saver is off when testing; it disables animations and looks like a bug.
- Do NOT animate things used hundreds of times a day beyond the small press squish.

6. ANIMATED ICONS (our own, drawn in Canvas; no asset downloads)
- One composable AnimatedIcon(name, fallback, tint, active, replay): picks a drawn icon by key, else a Lottie file in
  res/raw if present (optional dependency com.airbnb.android:lottie-compose, add ONLY if the team supplies Lottie files),
  else the Material icon popping in (scale 0.55 -> 1 spring, tilt -14deg -> 0).
- Drawn icons live on a 24-unit grid with a 2-unit round-cap stroke, drawn with Canvas + Path/PathMeasure. A progress
  Animatable 0 -> 1 runs spring(dampingRatio = 0.5f, stiffness = 250f) (about 0.5s, two overshoots; the stock medium
  spring ends in 180ms and is invisible) whenever `active` turns true or `replay` changes (tap counter for buttons).
  The icon also scales 0.75 -> 1 with progress. The resting pose is progress = 1, so it always ends exact.
- Per-icon motion ideas: home = roof lifts 5 units and door draws; receipt = lines write in staggered; wallet = strap
  draws, clasp pops; bars = rise left to right; more = three dots hop in a wave; arrows = fly in from a corner;
  refresh = full turn; bell = swings from the hook; lock = shackle drops shut; back = arrow slides in and nudges.
- Draw tab icons, top-bar actions and the 6-8 most visible row icons; leave the rest on the Material pop fallback.
- Unit-test the key->icon lookup (unique keys, unknown key falls through).

7. SWEEP CHECKLIST (do this last, per screen)
- No RectangleShape, no hairline-only boxes, no sharp corners, no all-caps labels, no hard-coded colours left.
- Every card is the shared card; every button is the shared button; every field uses the theme shape.
- Dialogs, bottom sheets and menus use the large corner shapes and the same button styles.
- Contrast: body text >= 4.5:1 in light and dark; brand button text white on light, dark on the lighter dark primary.
- Touch targets >= 48dp. Screen-reader labels on every icon-only control. No colour-only status (add text).
- Large font sizes and long labels still fit (nav bar goes icon-only above 1.3x font scale if labels would clip).
- Run unit tests, install a debug build, and walk Home, list, detail, settings/more, forms, dialogs in light and dark.

8. WHAT NOT TO COPY FROM SLICE
- Its logo, name, illustrations, mascots, copy and gradient art. Use only the structure, shapes, spacing and motion.
- Third-party fonts other than the OFL one named above.

DELIVERABLE
Short summary of the tokens and components added, a list of screens touched, anything left on fallback, and how to
verify it on a device. Do not commit.
````

## Notes for whoever runs it

- Screenshots: if the app sets `FLAG_SECURE` (finance and privacy apps often do), the agent cannot screenshot it. Verify on the device by eye.
- Why the spring numbers: a stock Material spring on a 24dp icon finishes in about 180 ms and reads as "no animation". Damping `0.5` and stiffness `250` run about half a second with two visible overshoots.
- Battery saver switches animations off on many phones (Samsung especially). Turn it off before judging motion.
- This prompt was distilled from the LedgerLock redesign. Reference files there: `ui/theme/{Color,Theme,Shape,Type}.kt`, `ui/CredComponents.kt`, `ui/AppButtons.kt`, `ui/AnimatedIcon.kt`, `ui/OwnIcons.kt`, `ui/AmountKeypad.kt`, `ui/MoreScreen.kt`.
