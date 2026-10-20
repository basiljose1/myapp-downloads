# Ledgerlock Privacy Policy

**Effective date:** [EFFECTIVE DATE]
**Developer:** [DEVELOPER NAME]
**Contact:** [CONTACT EMAIL]

> Draft for hosting on a public static page (Google Play requires an active, public, non-geofenced URL, not a
> PDF). This draft is not legal advice: have a lawyer familiar with India's Digital Personal Data Protection
> Act, 2023 review it before you publish. Delete this note before hosting.

## The short version

Ledgerlock is an expense tracker that works only on your phone. It has **no internet permission**, so the
app itself cannot send anything anywhere. There are no accounts, no analytics, no ads, no crash-reporting
services and no servers. Your financial data stays encrypted on your device unless **you** choose to export
or share it.

## What Ledgerlock reads, and why

To build your expense history automatically, Ledgerlock reads:

1. **SMS from business senders.** With your permission (`READ_SMS` to import past messages, `RECEIVE_SMS`
   to see new ones), Ledgerlock checks each message's sender. Messages from registered business senders
   (for example bank, card, wallet and payment headers such as `VM-HDFCBK-S`) are parsed to find the amount,
   account (last 4 digits), merchant or payee, date and balance.
2. **Notifications from payment apps you allow** (when this feature is available and you turn it on). Only
   notifications from an allowlist of payment and banking apps are read; all other notifications are
   ignored.

## What Ledgerlock never reads

- **Personal messages.** If a message comes from a phone number or any sender that is not a business
  header, Ledgerlock discards it **before reading its text**. Its content is never stored or parsed.
- **One-time passwords and promotions.** OTP and promotional messages are recognised and dropped; they are
  not stored.
- Contacts, call logs, location, photos (except receipt photos you attach yourself), or any other app's
  data.

## Where your data lives

- Everything Ledgerlock stores (parsed transactions, the business-sender messages they came from,
  categories, rules, budgets) is kept in an **encrypted database on your phone** (SQLCipher, with the key
  protected by the Android Keystore and unlocked by your screen lock or biometrics).
- Android's automatic cloud backup and device-to-device transfer are **switched off** for Ledgerlock, so
  your data is not copied to Google Drive or a new phone without your action.
- We (the developer) never receive your messages, transactions or balances.

## No network access

Ledgerlock does not request Android's `INTERNET` permission. You can check this yourself on the Google Play
listing ("About this app" → "App permissions") or in your phone's app settings. Our release process fails
the build if that permission ever appears.

Some things happen through Google Play on your behalf, not through Ledgerlock:

- **Purchases** of Ledgerlock Pro are handled by Google Play Billing. Google processes the payment under
  Google's privacy policy; we receive the standard order record Google gives developers. Ledgerlock checks
  whether you own Pro by asking the Play Store app on your phone.
- **App and language-model updates** are downloaded by the Play Store.

## Data that leaves your phone only when you choose

- **Backups and exports.** You can create a backup or export file. It is **encrypted with a password you
  choose** and saved wherever you pick (for example your phone storage or a cloud drive you use). We cannot
  open it, and we cannot recover a lost password.
- **Diagnostic reports.** If something goes wrong you can tap "Share diagnostic report". The report contains
  only internal parser names, error types and counts; amounts, account numbers, names and message text are
  removed before the file is created. You see the system share sheet and decide whether and where to send
  it.
- **"Report unparsed message".** You can choose to send us a single message that Ledgerlock could not read,
  so we can support that bank format. You redact it before sharing, and you send it yourself.

If you email us (support, diagnostics, a sample), we receive what you send and your email address. We use it
only to answer you and fix the problem, and delete it within [RETENTION PERIOD, e.g. 12 months] or sooner on
request.

[IF A NON-PLAY LICENCE KEY IS SOLD: To issue a licence key outside Google Play we use your email address and
store only a one-way hash of it inside the key. Describe the payment provider and retention here.]

## Retention and deletion

- Data stays on your phone until you delete it.
- **Delete everything:** use Settings → "Erase all data" in the app, or uninstall Ledgerlock. Either removes
  the database and its encryption key; there is no copy anywhere else for us to delete.
- Backup or export files you created are yours; delete them wherever you saved them.
- To withdraw permission to read SMS or notifications, turn it off in Android settings at any time. Data
  already imported stays until you erase it.

## Your rights

Because your data is processed on your device and not by us, you control it directly: you can view,
correct, export and erase it in the app. For anything we do hold (emails you sent us), contact
[CONTACT EMAIL] to access, correct or delete it, or to raise a grievance. We will respond within
[RESPONSE TIME]. If you are in India you may also complain to the Data Protection Board of India.

## Security

Encrypted database, key protected by the Android Keystore, optional app lock, no network access,
password-encrypted backups and exports, and redacted diagnostics.

## Children

Ledgerlock is not directed at children under 18.

## Changes

We will update this page and its effective date, and mention significant changes in the app's release notes.

## Sources used for this draft

- Google Play User Data policy (privacy policy and prominent disclosure requirements):
  https://support.google.com/googleplay/android-developer/answer/10144311
- Google Play SMS and Call Log permissions policy:
  https://support.google.com/googleplay/android-developer/answer/9047303
- DPDP Rules, 2025 notified 13/14 November 2025; notice requirements (Rule 3) apply after an 18-month
  phase-in (verify before submission):
  https://www.mondaq.com/india/data-protection/1708164/digital-personal-data-protection-rules-2025-notified
  , https://en.wikipedia.org/wiki/Digital_Personal_Data_Protection_Rules,_2025
