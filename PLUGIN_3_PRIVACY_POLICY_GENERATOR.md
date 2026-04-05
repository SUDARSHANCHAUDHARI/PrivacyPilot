# Plugin 3 — Play Store PrivacyPilot
**Claude Code Plugin | SudarshanTechLabs**
**Package:** `com.sudarshantechlabs` | **Author:** SUDARSHANCHAUDHARI
**Target Marketplace:** claude.com/plugins (official Anthropic directory)
**Estimated Build Time:** 2–4 days

---

## Overview

Reads an Android app's `AndroidManifest.xml` and Gradle dependencies, automatically detects all permissions and third-party SDKs (AdMob, Firebase, Crashlytics, etc.), and generates a fully compliant privacy policy HTML page. Pushes directly to the developer's GitHub Pages following the SudarshanTechLabs URL pattern.

No MCP server required. File reading + template generation + shell execution for git push.

---

## Folder Structure

```
privacypilot/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── commands/
│   ├── generate.md
│   ├── audit.md
│   ├── github-page.md
│   └── update.md
├── agents/
│   ├── manifest-reader.md
│   ├── sdk-detector.md
│   └── policy-writer.md
├── skills/
│   ├── permission-data-map.md
│   ├── sdk-disclosure-rules.md
│   ├── github-pages-deploy.md
│   ├── gdpr-compliance.md
│   └── play-data-safety.md
├── templates/
│   └── privacy-policy-template.html
└── README.md
```

---

## plugin.json

```json
{
  "name": "privacypilot",
  "version": "1.0.0",
  "description": "Automatically generate Play Store-compliant privacy policies from AndroidManifest.xml and Gradle dependencies. Deploys to GitHub Pages.",
  "author": "SUDARSHANCHAUDHARI",
  "homepage": "https://github.com/SUDARSHANCHAUDHARI/privacypilot-claude-plugin",
  "license": "MIT",
  "commands": [
    { "name": "generate",     "file": "commands/generate.md" },
    { "name": "audit",        "file": "commands/audit.md" },
    { "name": "github-page",  "file": "commands/github-page.md" },
    { "name": "update",       "file": "commands/update.md" }
  ],
  "agents": [
    { "name": "manifest-reader", "file": "agents/manifest-reader.md" },
    { "name": "sdk-detector",    "file": "agents/sdk-detector.md" },
    { "name": "policy-writer",   "file": "agents/policy-writer.md" }
  ],
  "skills": [
    "skills/permission-data-map.md",
    "skills/sdk-disclosure-rules.md",
    "skills/github-pages-deploy.md",
    "skills/gdpr-compliance.md",
    "skills/play-data-safety.md"
  ],
  "keywords": ["android", "privacy-policy", "gdpr", "google-play", "github-pages", "compliance"]
}
```

---

## Commands

### commands/generate.md

```markdown
Generate a complete privacy policy for an Android app by scanning its manifest and dependencies.

## Steps

1. Ask for:
   - Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
   - Path to `app/build.gradle.kts` or `build.gradle`
   - App name (display name)
   - Package name (e.g. com.sudarshantechlabs.myapp)
   - Developer/company name (default: SudarshanTechLabs)
   - Contact email (default: sudarshantechlabs@gmail.com)
   - Developer country (default: Thailand)

2. Spawn manifest-reader agent to parse AndroidManifest.xml:
   - Extract all `<uses-permission>` tags
   - Identify sensitive permissions (location, camera, contacts, storage, phone, SMS)
   - Note any `android:exported` components

3. Spawn sdk-detector agent to parse build.gradle/libs.versions.toml:
   - Detect AdMob (com.google.android.gms:play-services-ads)
   - Detect Firebase (com.google.firebase:*)
   - Detect Crashlytics (com.google.firebase:firebase-crashlytics)
   - Detect Analytics (com.google.firebase:firebase-analytics)
   - Detect Facebook SDK
   - Detect Adjust, AppsFlyer, or other attribution SDKs
   - Detect Room, Retrofit, OkHttp, Coil, Glide
   - Detect any other third-party libraries with data collection

4. Spawn policy-writer agent to generate the HTML privacy policy

5. Save output to `privacy-policy/index.html` in the project root

6. Output summary:
   ```
   PRIVACY POLICY GENERATED
   ─────────────────────────
   Permissions found: [list]
   SDKs detected: [list]
   Data collected: [list]
   Third-party sharing: [list]
   
   File saved: privacy-policy/index.html
   Privacy policy URL: https://sudarshanchaudhari.github.io/[appname]-privacy-policy/
   
   Next: Run /privacypilot:github-page to deploy
   ```
```

---

### commands/audit.md

```markdown
Audit an Android app's manifest and existing privacy policy for compliance gaps.

## Steps

1. Ask for:
   - AndroidManifest.xml path
   - Existing privacy policy URL or file (optional)
   - Build gradle file path

2. Scan and report:

   **Permissions declared but not disclosed**
   - List permissions in manifest that are not explained in privacy policy

   **SDKs detected but not disclosed**
   - Third-party libraries found in gradle that collect data but aren't in the policy

   **Missing required sections**
   - Contact information
   - Data deletion/retention policy
   - Third-party links
   - Children's privacy section (if targeting minors)
   - GDPR section (if distributing in EU)

   **Play Data Safety mismatches**
   - Data collected doesn't match Data Safety form declarations

3. Output audit report:
   ```
   PRIVACY POLICY AUDIT
   ─────────────────────
   App: [name]
   
   GAPS FOUND:
   ✗ [issue] — [how to fix]
   
   WARNINGS:
   ⚠ [warning] — [recommendation]
   
   COMPLIANT:
   ✓ [items that are correctly disclosed]
   
   VERDICT: [Compliant / Needs Update / Significant Gaps]
   ```

4. Offer to auto-fix by running `/privacypilot:generate`
```

---

### commands/github-page.md

```markdown
Deploy the generated privacy policy to GitHub Pages.

## Steps

1. Ask for:
   - GitHub username (default: SUDARSHANCHAUDHARI)
   - App name (e.g. MyFamilyTracker)
   - Confirm the target repo name: `[appname-lowercase]-privacy-policy`

2. Show the deployment plan:
   ```
   DEPLOYMENT PLAN
   ────────────────
   Repo to create/update: SUDARSHANCHAUDHARI/[appname-lowercase]-privacy-policy
   GitHub Pages URL: https://sudarshanchaudhari.github.io/[appname-lowercase]-privacy-policy/
   Branch: main
   File: index.html
   ```

3. Ask for explicit confirmation before any git operations

4. After confirmation, run these commands:
   ```bash
   # Navigate to privacy-policy folder
   cd privacy-policy
   
   # Initialize git if not already done
   git init
   git remote add origin https://github.com/SUDARSHANCHAUDHARI/[appname-lowercase]-privacy-policy.git
   
   # Add, commit, push
   git add index.html
   git commit -m "Update privacy policy — $(date +%Y-%m-%d)"
   git push -u origin main
   ```

5. Remind user to enable GitHub Pages in repo settings:
   - Settings → Pages → Source: Deploy from branch → main → / (root)
   - It takes 1–2 minutes for the URL to go live

6. Verify the URL is accessible after deployment

7. Remind user to add this URL to:
   - Play Console → Store Presence → Store settings → Privacy policy URL
   - The app itself (Settings screen or About screen)
   - AndroidManifest meta-data if declared
```

---

### commands/update.md

```markdown
Update an existing privacy policy when app permissions or SDKs change.

## Steps

1. Ask for:
   - Current privacy policy file location or URL
   - What changed: new permissions / new SDKs / removed features / other

2. Fetch current policy (from file or URL)

3. Re-scan the current manifest and build.gradle

4. Compare:
   - New permissions added since last policy update
   - New SDKs added
   - Permissions/SDKs removed (may need removal from policy)
   - Any policy section that's now outdated

5. Generate a diff of what needs to change:
   ```
   POLICY UPDATE REQUIRED
   ───────────────────────
   ADD these sections:
   - Camera permission disclosure (new in this version)
   - AdMob integration (newly added SDK)
   
   UPDATE these sections:
   - Data retention: update to reflect new 90-day limit
   
   REMOVE these sections:
   - Location permission (removed from manifest)
   ```

6. Generate the updated policy and ask for confirmation before overwriting

7. After update, run github-page command to redeploy
```

---

## Agents

### agents/manifest-reader.md

```markdown
---
name: manifest-reader
description: Parses AndroidManifest.xml to extract permissions and app components
---

You are an Android manifest parser. Your job is to read AndroidManifest.xml and extract all data-relevant information.

## What you extract

### Permissions
Read all `<uses-permission>` tags and categorize:

**Location**
- ACCESS_FINE_LOCATION — precise GPS location
- ACCESS_COARSE_LOCATION — approximate network location
- ACCESS_BACKGROUND_LOCATION — background location access

**Camera & Media**
- CAMERA — camera access
- READ_EXTERNAL_STORAGE — read files/photos
- WRITE_EXTERNAL_STORAGE — write files/photos
- READ_MEDIA_IMAGES / READ_MEDIA_VIDEO / READ_MEDIA_AUDIO — media access (Android 13+)

**Contacts & Identity**
- READ_CONTACTS — read contacts
- WRITE_CONTACTS — write contacts
- GET_ACCOUNTS — list accounts on device
- READ_PHONE_STATE — device ID, phone number
- READ_PHONE_NUMBERS — phone numbers

**Communication**
- SEND_SMS — send SMS
- READ_SMS — read SMS
- RECEIVE_SMS — receive SMS
- CALL_PHONE — make calls
- READ_CALL_LOG — read call history

**Network & Device**
- INTERNET — internet access (required by most apps)
- ACCESS_WIFI_STATE — WiFi network info
- BLUETOOTH — Bluetooth access
- NFC — NFC access
- VIBRATE — vibration (not data-sensitive)
- RECEIVE_BOOT_COMPLETED — start on boot
- USE_BIOMETRIC / USE_FINGERPRINT — biometric data

## Output format
```
PERMISSIONS DETECTED:
Category: Location
- ACCESS_FINE_LOCATION → Data collected: Precise location
- ACCESS_COARSE_LOCATION → Data collected: Approximate location

Category: Camera
- CAMERA → Data collected: Photos/videos (if saved)

[... etc]

SENSITIVE PERMISSIONS (require explicit explanation in policy):
[list of sensitive ones only]

NON-SENSITIVE (no disclosure needed):
INTERNET, VIBRATE, etc.
```
```

---

### agents/sdk-detector.md

```markdown
---
name: sdk-detector
description: Detects third-party SDKs in Gradle files and identifies their data collection practices
---

You are an Android SDK data-collection analyst. You read Gradle files to identify third-party libraries and their privacy implications.

## SDKs you know and their data practices

### Advertising
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Google AdMob | com.google.android.gms:play-services-ads | Device ID, IP address, advertising ID, app usage |
| Google AdMob Lite | com.google.android.gms:play-services-ads-lite | Same as AdMob |
| Facebook Audience Network | com.facebook.android:audience-network-sdk | Device info, location, app activity |
| Unity Ads | com.unity3d.ads:unity-ads | Device ID, gameplay data |
| AppLovin | com.applovin:applovin-sdk | Device info, location, behavioral data |
| ironSource | com.ironsource.sdk:mediationsdk | Device info, ad interactions |

### Analytics & Crash Reporting
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Firebase Analytics | com.google.firebase:firebase-analytics | App usage, device info, events |
| Firebase Crashlytics | com.google.firebase:firebase-crashlytics | Crash logs, device info, app state |
| Firebase Performance | com.google.firebase:firebase-perf | Performance metrics, network requests |
| Google Analytics | com.google.android.gms:play-services-analytics | Usage data |

### Core Firebase
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Firebase Auth | com.google.firebase:firebase-auth | Email, phone, social auth tokens |
| Firebase Firestore | com.google.firebase:firebase-firestore | User-provided data |
| Firebase Storage | com.google.firebase:firebase-storage | Files uploaded by user |
| Firebase Messaging | com.google.firebase:firebase-messaging | Device token for push notifications |
| Firebase Remote Config | com.google.firebase:firebase-config | App configuration (no user data) |

### Attribution & Marketing
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Adjust | com.adjust.sdk:adjust-android | Install referrer, device ID, events |
| AppsFlyer | com.appsflyer:af-android-sdk | Install source, events, device info |
| Branch | io.branch.sdk.android:library | Device ID, app events |

### Networking
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Retrofit | com.squareup.retrofit2:retrofit | Sends data you specify to your backend |
| OkHttp | com.squareup.okhttp3:okhttp | Network requests (your data) |

### Image Loading
| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| Coil | io.coil-kt:coil | URLs you specify (no user data sent) |
| Glide | com.github.bumptech.glide:glide | URLs you specify (no user data sent) |
| Picasso | com.squareup.picasso:picasso | URLs you specify (no user data sent) |

## Output format
```
SDKs DETECTED:
✗ AdMob (com.google.android.gms:play-services-ads)
  → Collects: Advertising ID, device info, IP address
  → Shares with: Google advertising partners
  → Must disclose: Yes — significant data collection

✓ Retrofit (com.squareup.retrofit2:retrofit)
  → Collects: Only what your app sends to your server
  → Must disclose: Depends on what data you send

THIRD-PARTY DATA SHARING REQUIRED IN POLICY:
- Google (AdMob, Firebase)
- [others]
```
```

---

### agents/policy-writer.md

```markdown
---
name: policy-writer
description: Generates a complete, Play Store-compliant HTML privacy policy
---

You are a privacy policy writer for Android apps. You generate clear, legally-appropriate, Play Store-compliant privacy policies.

## What you receive
- App name, package name, developer info
- List of permissions from manifest-reader
- List of SDKs from sdk-detector

## What you generate

A complete HTML privacy policy covering:

1. **Introduction** — what app does, who operates it
2. **Information we collect** — broken into categories:
   - Information you provide directly
   - Information collected automatically (permissions-based)
   - Information collected by third parties (SDK-based)
3. **How we use your information** — purpose for each data type
4. **Third-party services** — list each SDK with link to their privacy policy
5. **Data retention** — how long data is kept
6. **Data security** — how data is protected
7. **Children's privacy** — COPPA compliance statement
8. **Your rights** — data access, deletion, opt-out
9. **Changes to this policy** — update notification method
10. **Contact us** — developer contact info

## HTML template structure
Generate clean, mobile-friendly HTML that:
- Works well on mobile browsers (users often access from Play Store link)
- Has clear headings and bullet points
- Includes last updated date
- Has a contact email
- Does NOT use any CDN or external resources (must work offline/self-contained)
- Uses inline CSS only

## Tone
- Plain language — not legal jargon
- Specific — name the actual data collected, not vague language
- Accurate — only disclose what actually happens

## Key sentences to include for AdMob
"We use Google AdMob to display advertisements. AdMob may collect and use data including your device's advertising ID to show personalized ads. You can opt out of personalized ads in your device settings under Google → Ads. For more information, see Google's Privacy Policy at https://policies.google.com/privacy"

## Key sentences to include for Firebase Analytics
"We use Firebase Analytics to understand how users interact with our app. Firebase collects usage data such as which screens are viewed and app events. This data is anonymous and aggregated. For more information, see Google's Privacy Policy."

## Key sentences for Firebase Crashlytics
"We use Firebase Crashlytics to detect and fix app crashes. Crashlytics collects crash logs including device information and app state at the time of the crash. No personally identifiable information is included in crash reports."
```

---

## Skills

### skills/permission-data-map.md

```markdown
---
name: permission-data-map
description: Complete mapping of Android permissions to data types collected
trigger: when reading AndroidManifest.xml or generating a privacy policy
---

## Android Permission → Data Collection Mapping

| Permission | Data Type | Sensitivity | Must Disclose |
|---|---|---|---|
| ACCESS_FINE_LOCATION | Precise GPS location | High | Yes |
| ACCESS_COARSE_LOCATION | Approximate location | High | Yes |
| ACCESS_BACKGROUND_LOCATION | Continuous location | Very High | Yes — prominent disclosure required |
| CAMERA | Photos/videos | High | Yes |
| READ_CONTACTS | Contact names, numbers, emails | High | Yes |
| READ_PHONE_STATE | Device ID, IMEI | High | Yes |
| READ_EXTERNAL_STORAGE | Files, photos, documents | Medium-High | Yes |
| READ_MEDIA_IMAGES | Photos | Medium | Yes |
| SEND_SMS | SMS messages | High | Yes |
| READ_SMS | SMS content | Very High | Yes |
| RECORD_AUDIO | Audio/voice | High | Yes |
| USE_BIOMETRIC | Biometric patterns | Very High | Yes |
| GET_ACCOUNTS | Account list | Medium | Yes |
| INTERNET | Network access | Low | Only if sending data to your servers |
| VIBRATE | Haptic feedback | None | No |
| RECEIVE_BOOT_COMPLETED | Boot awareness | Low | No |
| ACCESS_WIFI_STATE | WiFi network name | Low | Mention if used for features |
| BLUETOOTH | Nearby devices | Medium | Yes if collecting device info |
| NFC | NFC tag data | Medium | Yes if reading NFC data |

## Play Data Safety categories
These map to the Data Safety form in Play Console:
- Location → Location (precise or approximate)
- Camera → Photos and videos
- Contacts → Contacts
- Storage → Files and docs / Photos and videos
- Phone state → Device or other IDs
- Microphone → Audio files
- Biometric → Health and fitness / Personal info
```

---

### skills/sdk-disclosure-rules.md

```markdown
---
name: sdk-disclosure-rules
description: Rules for disclosing third-party SDK data collection in privacy policies
trigger: when generating a privacy policy or auditing data disclosure
---

## Third-Party SDK Disclosure Rules

### Rule 1 — Always disclose advertising SDKs
Any advertising SDK (AdMob, Facebook Audience Network, etc.) MUST be disclosed because:
- They collect advertising IDs
- They may share data with their ad networks
- Google Play requires this in the Data Safety form
- GDPR/CCPA may apply

### Rule 2 — Disclose analytics SDKs
Firebase Analytics, Google Analytics, etc. must be disclosed.
- Describe as "anonymous usage data" if it truly is
- Note that data is processed by Google

### Rule 3 — Disclose crash reporters
Crashlytics, Sentry, etc. must be disclosed.
- Clarify that no personal data is in crash logs (if true)

### Rule 4 — Link to third-party policies
For each SDK disclosed, include a link to the third-party's privacy policy:
- Google: https://policies.google.com/privacy
- Facebook: https://www.facebook.com/privacy/explanation
- Unity: https://unity.com/legal/privacy-policy

### Rule 5 — Data Safety form must match policy
Whatever you declare in Play Console's Data Safety form must be consistent with your privacy policy. Mismatch = policy warning or removal.

### Common Data Safety form mistakes
- Declaring "no data collected" when AdMob is included → violation
- Not declaring Firebase Analytics → violation
- Claiming data is not shared when AdMob shares with Google → violation

### Opt-out information
For advertising data, always include:
- How users can opt out of personalized ads (device settings → Google → Ads)
- Link to Google's ad settings: https://adssettings.google.com
```

---

### skills/github-pages-deploy.md

```markdown
---
name: github-pages-deploy
description: Deploying privacy policy HTML to GitHub Pages for SudarshanTechLabs apps
trigger: when deploying or updating a privacy policy to GitHub Pages
---

## GitHub Pages Deployment Pattern

### URL convention (SudarshanTechLabs)
`https://sudarshanchaudhari.github.io/[appname-lowercase]-privacy-policy/`

Examples:
- MyFamilyTracker → `https://sudarshanchaudhari.github.io/myfamilytracker-privacy-policy/`
- BatteryGuard → `https://sudarshanchaudhari.github.io/batteryguard-privacy-policy/`

### Repository naming
`[appname-lowercase]-privacy-policy`

Example: `myfamilytracker-privacy-policy`

### One-time setup per app
1. Create repo on GitHub: `SUDARSHANCHAUDHARI/[appname-lowercase]-privacy-policy`
2. Push `index.html` to main branch
3. Enable GitHub Pages: repo Settings → Pages → Source: main branch / root
4. URL goes live in 1–2 minutes

### Update workflow
```bash
# In the policy repo directory
git add index.html
git commit -m "Update privacy policy — $(date +%Y-%m-%d)"
git push origin main
```

### Verify deployment
After push, check:
`https://sudarshanchaudhari.github.io/[appname-lowercase]-privacy-policy/`

If 404, check GitHub Pages is enabled in repo Settings.

### Single file setup
The entire privacy policy should be one self-contained `index.html`:
- No external CSS files
- No external JS files
- No CDN links
- All styling inline or in `<style>` tag
- Loads fast, works offline, no dependencies

### Play Console URL field
In Play Console → Store Presence → Store Settings → Privacy policy:
Enter: `https://sudarshanchaudhari.github.io/[appname-lowercase]-privacy-policy/`
```

---

### skills/gdpr-compliance.md

```markdown
---
name: gdpr-compliance
description: GDPR and international privacy law requirements for Android apps
trigger: when generating a privacy policy or assessing compliance
---

## GDPR Compliance for Android Apps

### Who GDPR applies to
GDPR applies if your app is distributed to users in the European Union, regardless of where you (the developer) are based. Even as a developer in Thailand with a Play Store listing available globally, GDPR applies to your EU users.

### Required elements in GDPR-compliant privacy policy

**Data controller identity**
- Name / company name: SudarshanTechLabs
- Contact: sudarshantechlabs@gmail.com
- Country: Thailand

**Legal basis for processing**
Choose the correct basis for each data type:
- Legitimate interest — analytics, crash reporting
- Consent — advertising (especially personalized ads)
- Contract — data needed to provide the service
- Legal obligation — if required by law

**Data subject rights**
Must inform users they can:
- Access their data
- Correct inaccurate data
- Delete their data ("right to be forgotten")
- Object to processing
- Data portability

**Data transfers outside EU**
If using Firebase/AdMob: mention that data may be transferred to the US (Google's servers) under Standard Contractual Clauses.

**Retention periods**
Specify how long each data type is retained.

### GDPR-required language
"Residents of the European Economic Area (EEA) have additional rights under the General Data Protection Regulation (GDPR). You may contact us at sudarshantechlabs@gmail.com to exercise your rights of access, correction, deletion, or portability."

### CCPA (California)
For California users:
"California residents may have additional rights under the California Consumer Privacy Act (CCPA). We do not sell your personal information."

### COPPA (Children under 13)
If app is NOT targeted at children under 13:
"Our app is not directed to children under 13 years of age. We do not knowingly collect personal information from children under 13."

If app IS targeted at children under 13 — additional COPPA compliance required. Contact legal counsel.
```

---

### skills/play-data-safety.md

```markdown
---
name: play-data-safety
description: How to complete the Play Console Data Safety form accurately
trigger: when filling out the Data Safety section in Play Console
---

## Play Console Data Safety Form

### Navigation
Play Console → App → Policy → Data Safety

### Key decisions

**Does your app collect or share user data?**
- Yes — if you use AdMob, Firebase Analytics, Crashlytics, or collect anything
- No — only if app is 100% offline and sends nothing anywhere

**Is all user data encrypted in transit?**
- Yes — if using HTTPS for all network calls (Retrofit/OkHttp default)
- Firebase uses HTTPS by default

**Can users request data deletion?**
- Must provide a way — either in-app or via email
- Link in Play Console to your deletion request method
- Acceptable: "Email sudarshantechlabs@gmail.com to request data deletion"

### Data type checklist

**Location**
- Check "Location" → Approximate or Precise
- If using location: check "Shared with third parties" if AdMob enabled

**Personal info**
- Name, email, phone — check if collected via Firebase Auth

**Financial info**
- Payment info — check if using Play Billing (Google handles this, mark accordingly)

**Health and fitness**
- Only if app tracks health data

**Messages**
- Only if app sends/reads SMS or shows messages

**Photos and videos**
- Check if app accesses camera or photo library

**Audio files**
- Check if app records audio

**Files and docs**
- Check if app reads/writes external storage

**Apps on device**
- Only if app queries installed apps list

**Web browsing**
- Only if app has WebView logging activity

**App activity**
- App interactions — check if using Firebase Analytics
- Installed apps — leave unchecked unless querying package manager
- Other user-generated content

**App info and performance**
- Crash logs — check if using Crashlytics
- Diagnostics — check if using Firebase Performance

**Device or other IDs**
- Advertising ID — ALWAYS check this if using AdMob
- Device ID — check if reading device identifiers

### Common Data Safety mistakes
1. Not checking "Advertising ID" when AdMob is in the app
2. Not declaring data as "Shared" when using AdMob (it shares with Google)
3. Declaring data as optional when it's required to use the app
4. Missing "Crash logs" when Crashlytics is used
```

---

## Template

### templates/privacy-policy-template.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Privacy Policy — {{APP_NAME}}</title>
<style>
  body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; color: #333; line-height: 1.6; }
  h1 { color: #1a1a1a; border-bottom: 2px solid #eee; padding-bottom: 10px; }
  h2 { color: #2a2a2a; margin-top: 30px; }
  a { color: #0066cc; }
  .last-updated { color: #666; font-size: 0.9em; }
  ul { padding-left: 20px; }
  li { margin: 6px 0; }
</style>
</head>
<body>

<h1>Privacy Policy</h1>
<p class="last-updated">Last updated: {{DATE}}</p>

<p>{{APP_NAME}} ("we", "us", or "our") operates the {{APP_NAME}} mobile application (the "App"). This page informs you of our policies regarding the collection, use, and disclosure of personal data when you use our App and the choices you have associated with that data.</p>

<h2>Information We Collect</h2>

{{PERMISSIONS_SECTION}}

{{SDK_SECTION}}

<h2>How We Use Your Information</h2>
<p>We use the collected data for the following purposes:</p>
<ul>
{{USAGE_LIST}}
</ul>

<h2>Third-Party Services</h2>
<p>Our App uses the following third-party services that may collect information used to identify you:</p>
{{THIRD_PARTY_LIST}}

<h2>Data Retention</h2>
<p>We retain collected information for as long as necessary to provide our services. Crash reports and analytics data are retained for up to 90 days. If you wish to request deletion of your data, please contact us at {{CONTACT_EMAIL}}.</p>

<h2>Data Security</h2>
<p>We value your trust and use commercially acceptable means to protect your information. All data transmitted between the App and our servers is encrypted using HTTPS. However, no method of transmission over the internet or electronic storage is 100% secure.</p>

<h2>Children's Privacy</h2>
<p>Our App is not directed to children under the age of 13. We do not knowingly collect personally identifiable information from children under 13. If you are a parent or guardian and believe your child has provided us with personal information, please contact us at {{CONTACT_EMAIL}}.</p>

<h2>Your Rights</h2>
<p>You have the right to:</p>
<ul>
  <li>Access the personal data we hold about you</li>
  <li>Request correction of inaccurate data</li>
  <li>Request deletion of your data</li>
  <li>Object to our processing of your data</li>
  <li>Opt out of personalized advertising (via device settings → Google → Ads)</li>
</ul>
<p>To exercise these rights, contact us at {{CONTACT_EMAIL}}.</p>

{{GDPR_SECTION}}

<h2>Changes to This Privacy Policy</h2>
<p>We may update our Privacy Policy from time to time. We will notify you of any changes by posting the new Privacy Policy on this page and updating the "last updated" date. You are advised to review this Privacy Policy periodically for any changes.</p>

<h2>Contact Us</h2>
<p>If you have any questions about this Privacy Policy, please contact us:</p>
<ul>
  <li>Email: {{CONTACT_EMAIL}}</li>
  <li>Developer: {{DEVELOPER_NAME}}</li>
  <li>Country: {{DEVELOPER_COUNTRY}}</li>
</ul>

</body>
</html>
```

---

## README.md

```markdown
# PrivacyPilot — Claude Code Plugin

Scan your AndroidManifest.xml and Gradle files, generate a compliant privacy policy, deploy to GitHub Pages — all in one command.

## Install

```bash
/plugin marketplace add SUDARSHANCHAUDHARI/privacypilot-claude-plugin
/plugin install privacypilot@SUDARSHANCHAUDHARI-PrivacyPilot
```

## Commands

| Command | What it does |
|---|---|
| `/privacypilot:generate` | Full policy from manifest + Gradle scan |
| `/privacypilot:audit` | Audit existing policy for gaps |
| `/privacypilot:github-page` | Deploy to GitHub Pages |
| `/privacypilot:update` | Update policy after changes |

## What it detects automatically
- All Android permissions (location, camera, contacts, storage, etc.)
- AdMob, Firebase Analytics, Crashlytics, Adjust, AppsFlyer, and more
- Generates proper disclosures for each

## Requirements
- Claude Code installed
- Git installed (for deployment)
- GitHub account: SUDARSHANCHAUDHARI

## License
MIT
```

---

## Development Checklist

- [ ] Create GitHub repo: `SUDARSHANCHAUDHARI/privacypilot-claude-plugin`
- [ ] Build all files from structure above
- [ ] Test `generate` command on MyFamilyTracker (com.sudarshantechlabs.myfamilytracker)
- [ ] Test `audit` command against an existing policy
- [ ] Test `github-page` deploy command end-to-end
- [ ] Test `update` command with a changed manifest
- [ ] Verify HTML output is mobile-friendly
- [ ] Load locally: `/plugin load /path/to/privacypilot`
- [ ] Submit at: `platform.claude.com/plugins/submit`
