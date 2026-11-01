# Play Permissions Declaration: SMS

Answers for Play Console → App content → Sensitive app permissions → Permissions Declaration Form.
This is the main launch risk (spec section 6): Bluecoins was refused in 2019 because SMS was judged not core.
Everything below keeps SMS parsing visibly the core feature.

## What the policy and form require (checked 2026-10-17)

- `READ_SMS`/`RECEIVE_SMS` are restricted to default SMS/Phone/Assistant handlers plus listed exceptions.
  **"SMS-based money management"** ("apps that track and manage budget") is an exception allowing
  `READ_SMS`, `RECEIVE_MMS`, `RECEIVE_SMS`, `RECEIVE_WAP_PUSH`. Exceptions are **temporary**, need Play
  review and approval, and apply only where no alternative gives the same core functionality.
  [SMS & Call Log policy](https://support.google.com/googleplay/android-developer/answer/9047303)
- Spyware policy rider: budgeting apps "may not exfiltrate or share non-financial or personal SMS history".
  Ledgerlock has no network access and discards personal senders before reading the body.
- Form steps: choose core use case → review instructions → **video demonstration (required; YouTube link
  preferred, or a cloud link to an MP4)** → restricted-access/test credentials → APK exceptions → confirm.
  Review can take **several weeks**; the release sits in "pending publication".
  [Declaration form help](https://support.google.com/googleplay/android-developer/answer/9214102)
- Upcoming policy changes (July 2026 announcement) touch only `READ_CALL_LOG`; no change for the money
  management exception. [Policy update](https://support.google.com/googleplay/android-developer/answer/17225965)
- Prominent disclosure must be in-app, shown in normal use, immediately before the runtime prompt, describe
  data and use, and need an affirmative tap.
  [User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311)

## Permissions declared

| Permission | Why |
| --- | --- |
| `READ_SMS` | One-time import of the last 12 months of bank/card/wallet SMS (default; user can change) so the dashboard is useful on day one, plus on-demand re-scan to fill gaps. |
| `RECEIVE_SMS` | Record each new transaction alert as it arrives. |

Not requested: `SEND_SMS`, `WRITE_SMS`, `RECEIVE_MMS`, `RECEIVE_WAP_PUSH`, any Call Log permission,
`INTERNET`. Before submitting, confirm the merged release manifest lists only these two SMS permissions.

## Form answers

**Core functionality (use case):** SMS-based money management.

**Describe your app's core functionality** (paste):

> Ledgerlock is an expense tracker for India that builds the user's spending, income and account balances
> automatically from the transaction alert SMS their banks, card issuers and wallets send. Parsing these SMS
> on the device is the app's core function: every transaction, balance, budget and chart in the app comes
> from them. Without SMS access the app has no transactions to show.
>
> READ_SMS imports past transaction alerts once (user-chosen period, default 12 months) so the history and
> balances are complete; RECEIVE_SMS records each new alert. Only messages from business sender headers (for
> example VM-HDFCBK-S) are parsed. Messages from phone numbers are discarded before their content is read,
> and OTP and promotional messages are dropped. The app has no INTERNET permission: SMS content is stored
> only in an encrypted on-device database and is never transmitted.
>
> There is no alternative: Indian banks do not offer a free, offline, on-device transaction feed, and the
> app is designed to work without accounts or servers.

**Instructions for reviewers** (paste):

> 1. Install and open. No sign-in is needed.
> 2. Tap "Get started". Read the SMS disclosure screen, tap "Allow SMS access", accept the system prompt.
> 3. Choose the import period and tap "Import". Transactions from bank SMS appear on Home and in
>    Transactions; open one to see the SMS it came from.
> 4. To test live capture, send a test SMS shaped like a bank alert from an emulator (e.g. sender
>    `VM-HDFCBK-S`: "Rs.250.00 debited from A/c XX1234 on 12-10-26 to VPA test@upi. Ref 123456789012").
>    A new transaction appears within seconds. An SMS from a phone number does not.
> 5. Settings → "Erase all data" removes everything.

**Restricted access:** "All functionality is available without special access." (No login.) If the Pro
trial matters to the review, note: "A 14-day Pro trial starts automatically; no purchase needed."

## In-app disclosure (shown before the system prompt)

Full screen in onboarding, step 2, after the promise screen. Buttons: **Allow SMS access** (then the system
prompt) and **Not now** (continue to a screen explaining the app needs SMS or a statement import).

> **Ledgerlock reads your bank SMS, on this phone only**
>
> To track your spending automatically, Ledgerlock needs permission to read SMS. It uses it to:
>
> - import past transaction alerts from banks, cards and wallets, and
> - record new alerts as they arrive.
>
> Only messages from business senders (like VM-HDFCBK-S) are read. Messages from people are skipped
> before their text is read. OTPs and offers are ignored.
>
> Ledgerlock has no internet access. Your messages and transactions are stored encrypted on this phone and
> are never sent to us or anyone else.
>
> You can turn this off any time in Android settings.
>
> [Allow SMS access]   [Not now]

Same pattern (separate screen, affirmative tap) for notification access when that feature ships.

## Demo video (30–60 s) shot list

Record a screen capture on a real phone or emulator with **fake, anonymised SMS** (no real names, account
numbers or VPAs). Upload unlisted to YouTube. Add captions burned in so reviewers can follow with sound off.

| # | Time | Shot | Caption |
| --- | --- | --- | --- |
| 1 | 0–4 s | Launcher → open Ledgerlock, promise screen | "Ledgerlock: expense tracking from bank SMS" |
| 2 | 4–12 s | SMS disclosure screen, hold 3 s, tap "Allow SMS access" | "In-app disclosure before permission" |
| 3 | 12–16 s | System permission dialog → Allow | "User grants READ_SMS / RECEIVE_SMS" |
| 4 | 16–24 s | Import progress, then Home fills with net spend, income, savings | "Past bank SMS parsed on device" |
| 5 | 24–32 s | Split view or quick cut: Messages app with a bank SMS → same transaction in Transactions → open detail showing source SMS | "Each transaction comes from an SMS" |
| 6 | 32–42 s | Send a new fake bank SMS (emulator) → notification → it appears at the top of Transactions; balance updates | "New SMS recorded instantly (RECEIVE_SMS)" |
| 7 | 42–48 s | Send an SMS from a phone number → nothing appears | "Personal messages are ignored" |
| 8 | 48–55 s | Analytics/budgets built from these transactions | "Budgets and analytics depend on SMS data" |
| 9 | 55–60 s | Android Settings → App info → Permissions (no Internet listed) | "No internet permission: data stays on device" |

Keep it under 60 s; do not show other features first. The first 15 seconds must already show SMS.
