Generate a complete privacy policy for an Android app by scanning its manifest and dependencies.

## Steps

1. Ask for:
   - Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
   - Path to `app/build.gradle.kts` or `build.gradle`
   - App name (display name)
   - Package name (e.g. com.example.yourapp)
   - Developer/company name (default: ${user_config.company})
   - Contact email (default: ${user_config.email})
   - Developer country (default: ${user_config.country})

2. Spawn manifest-reader agent to parse AndroidManifest.xml:
   - Extract all `<uses-permission>` tags
   - Identify sensitive permissions (location, camera, contacts, storage, phone, SMS)
   - Note any `android:exported` components

3. Spawn sdk-detector agent to parse build.gradle/libs.versions.toml:
   - Detect AdMob, Firebase, Crashlytics, Analytics
   - Detect Facebook SDK, Adjust, AppsFlyer, and other attribution SDKs
   - Detect Room, Retrofit, OkHttp, Coil, Glide
   - Detect any other third-party libraries with data collection

4. Spawn policy-writer agent, passing:
   - App name, package name
   - Developer/company name, contact email, country collected in step 1
   - Full permissions output from manifest-reader
   - Full SDK output from sdk-detector
   - Save path: `privacy-policy/index.html` in the current project directory

5. Output summary:
   ```
   PRIVACY POLICY GENERATED
   ─────────────────────────
   Permissions found: [list]
   SDKs detected: [list]
   Data collected: [list]
   Third-party sharing: [list]

   File saved: privacy-policy/index.html
   Privacy policy URL: https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/

   Next: Run /privacypilot:github-page to deploy
   ```
