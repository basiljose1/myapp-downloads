# Release checklist: signed build to production

Ordered. Items marked **(verify)** could not be fully confirmed online on 2026-10-17. Long-lead items: SMS
declaration review (up to several weeks) and the 14-day closed test. Start both with the first closed-test
build, in parallel.

## 1. Build

1. [ ] **Upload key.** Create an upload keystore (`keytool -genkeypair -v -keystore upload.jks -keyalg RSA
   -keysize 4096 -validity 10000 -alias upload`). Store it and its passwords outside the repo, in two
   offline backups. Add a `release` `signingConfig` that reads them from `~/.gradle/gradle.properties` or
   environment variables (none exists in `app/build.gradle.kts` yet).
2. [ ] **Play App Signing.** On first upload, let Google generate and hold the app signing key (required for
   AAB uploads, verify). Keep the upload key; a lost upload key can be reset through Play support.
3. [ ] **Sideload signing.** Decide whether the sideload APK uses the same key as Play (lets users switch
   channels without uninstalling) or its own key. Download the signed universal APK or sign yourself
   accordingly.
4. [ ] **Version.** Bump `versionCode` (integer, always increasing, currently 1) and `versionName`
   (currently `0.0.1`) in `app/build.gradle.kts`. Tag the commit `v<versionName>`.
5. [ ] **Target API.** `targetSdk = 36` (already set). Required for new apps and updates from
   2026-08-31 ([target API](https://developer.android.com/google/play/requirements/target-sdk),
   [Play Help](https://support.google.com/googleplay/android-developer/answer/11926878)).
6. [ ] **R8.** `isMinifyEnabled = true` is set for release; add `isShrinkResources = true` if wanted. Test the
   release build on a device: SQLCipher, BiometricPrompt, WorkManager and the SMS receiver must survive
   shrinking (add keep rules if anything is stripped). Upload `mapping.txt` with the bundle.
7. [ ] **Build:** `./gradlew test :app:bundleRelease :app:assembleRelease`.
8. [ ] **No network:** `scripts/check-no-internet.sh` passes. Also inspect the merged manifest
   permissions: only `READ_SMS`, `RECEIVE_SMS` (+ `com.android.vending.BILLING` once billing ships, plus
   library-added ones you have reviewed). Check that the Play build contains no sideload licence code
   (spec 5.2: Gradle flavor).
9. [ ] **Smoke test the release APK** on a low-end phone and one Xiaomi/Realme/Vivo/Oppo device: onboarding,
   SMS import, live SMS, app lock, backup/restore, erase all data, Pro purchase/restore offline.

## 2. Play Console setup (do once, before the first closed test)

10. [ ] Host `privacy-policy.md` on a public, non-geofenced URL (not a PDF); fill placeholders; lawyer review.
    Link it from inside the app too ([User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311)).
11. [ ] **App content:** privacy policy URL, ads (No), app access (no login), target audience (18+),
    content rating questionnaire (IARC), news app (No), government app (No).
12. [ ] **Financial features declaration** (required for every app on any track): choose the closest option
    for a personal budgeting tracker that offers no banking, loans or payments; likely "Other" (verify)
    ([help](https://support.google.com/googleplay/android-developer/answer/13849271)).
13. [ ] **Data safety** form per `data-safety.md` (decide Option A or B first).
14. [ ] **Sensitive permissions: SMS declaration** per `play-sms-declaration.md`, with the demo video
    uploaded unlisted to YouTube. Required on any track that ships `READ_SMS`/`RECEIVE_SMS`
    ([form help](https://support.google.com/googleplay/android-developer/answer/9214102)).
15. [ ] **Store listing** per `store-listing.md`: icon 512×512, feature graphic 1024×500, 6 screenshots with
    anonymised data.

## 3. Closed test (required for new personal developer accounts)

16. [ ] Create a closed testing track and upload the AAB. For personal accounts created after 2023-11-13,
    **at least 12 testers must be opted in, continuously for the preceding 14 days**, before you can apply
    for production; a tester who opts out and back in restarts the count
    ([testing requirements](https://support.google.com/googleplay/android-developer/answer/14151465)).
    Recruit 15–20 to keep a margin.
17. [ ] Ask testers to use it for real (import SMS, review inbox, budgets) and collect feedback through a
    form or email; you will need to summarise it.
18. [ ] **Pre-launch report:** read it for every closed-test upload. Crashes on SMS-less test devices
    (empty inbox, permission denied) must show a sensible empty state. Note that robo tests cannot grant
    SMS realistically.
19. [ ] Fix, upload new builds to the same track (testers stay opted in).

## 4. Onboarding must-haves (check in the closed-test build)

20. [ ] SMS prominent-disclosure screen before the system prompt, with an affirmative button.
21. [ ] Denied-permission path: explain why SMS is needed, offer settings deep link (and statement import
    once it ships), never a dead end.
22. [ ] **OEM battery/autostart guidance:** detect manufacturer and show steps or deep links to autostart and
    battery-optimisation settings (Xiaomi/Redmi/POCO, Realme/Oppo/OnePlus, Vivo/iQOO, Samsung), plus
    `ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` only if its policy allows our use case (verify; otherwise
    link to the settings list). The SMS backfill re-scan catches anything missed.
23. [ ] Privacy policy link inside the app (Settings → Privacy).

## 5. Production

24. [ ] Dashboard → **Apply for production**: answer the three sections (closed test, app, production
    readiness).
25. [ ] After approval, create a production release with a **staged rollout** (e.g. 10% → 50% → 100%) and
    watch Android vitals and reviews.
26. [ ] The SMS declaration must be **approved** before an SMS build goes live; if an urgent fix is needed
    while review is pending, the form help says you can ship without the restricted permissions, which
    would remove SMS (not useful here; plan reviews ahead).

## 6. Fallback channel

27. [ ] **Sideload APK:** publish the signed APK with its SHA-256 checksum and signing-certificate
    fingerprint on the website, so the app survives a Play denial or later revocation (exceptions are
    temporary). Non-Play builds use the licence-key flavor for Pro (spec 5.2), and need the in-app guide for
    Android 13+ "restricted settings" before notification access can be enabled.
28. [ ] Re-submit the SMS declaration whenever SMS usage changes, and re-check the data safety form when a
    dependency or permission changes.
