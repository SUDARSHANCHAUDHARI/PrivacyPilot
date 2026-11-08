---
description: Draft the Play Console Data Safety form answers for an Android app from its manifest and dependencies.
argument-hint: "[manifest path] [gradle path] [accounts: yes|no] [deletion method]"
---

Draft the answers for the Play Console Data Safety form (Play Console → App → Policy → Data
Safety) from the app's manifest and Gradle files. This drafts answers for the developer to review
and enter; it does not submit anything.

## Steps

1. Use what `$ARGUMENTS` gives; ask only for what is still missing:
   - Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
   - Path to `app/build.gradle.kts` or `build.gradle`, and `gradle/libs.versions.toml` if present
   - Whether the app has user accounts or sign-in (the files cannot show this reliably)
   - How users can request data deletion (default: email ${user_config.email})

2. Spawn manifest-reader agent on the manifest, and sdk-detector agent on the Gradle files, as
   `/privacypilot:generate` does.

3. Using the play-data-safety skill, decide each answer from the findings only:
   - Does the app collect or share user data?
   - Is all user data encrypted in transit? (Yes only if every network library found uses HTTPS by
     default and the manifest does not set `android:usesCleartextTraffic="true"`.)
   - Can users request deletion? (from step 1)
   - For every data type: collected, shared, which SDK or permission causes it, purpose, and
     whether it is optional.

4. Mark every answer that the files cannot prove as **CONFIRM** instead of guessing (for example:
   whether collection is optional, or data the app's own backend stores).

## Output

```
DATA SAFETY — <app>
───────────────────
Collects or shares user data: Yes | No
Encrypted in transit: Yes | No | CONFIRM — <reason>
Users can request deletion: Yes — <method>

| Data type | Collected | Shared | Because of | Purpose | Optional |
|---|---|---|---|---|---|
| Device or other IDs → Advertising ID | Yes | Yes | AdMob | Advertising | No |
| ... | | | | | |

CONFIRM before submitting:
- <each answer the files could not prove>

Common mistakes checked: Advertising ID with AdMob, "Shared" with AdMob, Crash logs with Crashlytics
```

List only data types with at least one Yes. End with: "Check these against your privacy policy:
run /privacypilot:audit, or the policy-verifier agent on privacy-policy/index.html."
