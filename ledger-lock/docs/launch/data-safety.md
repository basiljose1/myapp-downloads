# Play Data safety form

Answers for Play Console → App content → Data safety. Checked against Google's definitions on 2026-10-17.

## The definitions that decide the answers

From [Provide information for Google Play's Data safety section](https://support.google.com/googleplay/android-developer/answer/10787469):

- **Collect** = "transmitting data from your app off a user's device". SDKs and libraries count.
- Data "only processed locally on the user's device and not sent off device does **not** need to be
  disclosed."
- **Share** = transferring collected user data to a third party. Excluded: transfers to service providers,
  for legal reasons, and transfers "based on a specific user-initiated action".
- **Payment services:** with Google Play Billing you "don't need to declare collection of the data that the
  payment service collects" if your app never accesses it.
- A privacy policy link is required even when no data is collected.

User-facing explanation of the same rule: [Google Play Help: Data safety](https://support.google.com/googleplay/answer/11416267).

## Why "collected" needs care for Ledgerlock

Ledgerlock reads Messages (SMS) and Financial info, which are the most sensitive types on the form. They are
not "collected" only because nothing is transmitted off the device: there is no INTERNET permission, no SDK
that sends data, auto-backup is disabled (`allowBackup=false`, data extraction rules), and Play Asset
Delivery/Billing traffic is done by the Play Store app, not Ledgerlock. That claim must stay true for every
release: `scripts/check-no-internet.sh` must pass, and any new library must be reviewed for its own
manifest entries and data flows.

Three flows send data off the device, all started by the user through the Android share sheet:

| Flow | What leaves | Assessment |
| --- | --- | --- |
| Encrypted backup / export | Password-encrypted file to a location the user picks | User-initiated, encrypted, developer never receives it → not collection by the developer, not sharing. |
| Share diagnostic report | Parser ids, error class names, counts; no amounts, names, accounts or text (`core/diagnostics`) | Usually sent to the developer by email. Arguably not "your app transmitting"; if declared, it is **Crash logs / Diagnostics, optional**. |
| Report unparsed message | One user-redacted SMS sample, sent by the user to the developer | Closest to **Messages, optional**. The most debatable item. |

**Decision for you:** Option A (recommended unless your lawyer says otherwise): declare **no data
collected**, because the app transmits nothing and the share-sheet flows are user-initiated, with the
reasoning above recorded here. Option B (most conservative): declare *Crash logs*, *Diagnostics* and
*Messages* as collected, **optional**, not shared, purpose "App functionality", processed by the developer
only. Option B loses the "No data collected" label but cannot be called misleading. Whichever you pick, the
privacy policy describes all three flows (it already does). Verify before submission.

## Form answers (Option A)

| Question | Answer |
| --- | --- |
| Does your app collect or share any of the required user data types? | **No** |
| Is all of the user data collected by your app encrypted in transit? | Not asked when "No" (verify before submission); if asked: no data is transmitted. |
| Do you provide a way for users to request that their data is deleted? | Not asked when "No" (verify before submission); if asked: in-app "Erase all data" or uninstall. |
| Privacy policy URL | [HOSTED URL of privacy-policy.md] |

Data types considered and why each is "not collected":

| Data type | Read by the app? | Leaves the device? |
| --- | --- | --- |
| Messages → SMS or MMS | Yes, business senders only | No |
| Financial info → User payment info / Other financial info | Yes (amounts, last-4 digits, balances parsed from SMS) | No |
| Financial info → Purchase history | Pro ownership read from Play Billing | No (Google collects the payment; exempt) |
| App activity, App info and performance | Local crash records only | Only if the user shares a redacted report (see above) |
| Personal info (email) | Only for a non-Play licence key, if offered | Entered on the developer's website, not in the app; covered in the privacy policy |
| Photos (receipts) | User-attached, stored encrypted | No |
| Location, contacts, device IDs | No | No |

The licence key may include a hash of `ANDROID_ID` (sideload channel only, spec 5.2), verified on device.
It is not transmitted, and that code must stay out of the Play build flavor.

## Keep consistent with

- The privacy policy (`privacy-policy.md`) and store listing claims (`store-listing.md`).
- The SMS declaration (`play-sms-declaration.md`): it says SMS never leaves the device.
- Re-check this form whenever a dependency, permission or sharing flow changes.
