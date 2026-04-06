# PrivacyPilot — Claude Code Plugin

> Scan your `AndroidManifest.xml` and Gradle files, generate a fully compliant privacy policy, and deploy it to GitHub Pages — all from Claude Code.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Code-blueviolet)
![Target](https://img.shields.io/badge/target-Android%20%2F%20Google%20Play-brightgreen)

---

## The Problem

Every Android app on Google Play requires a privacy policy. Writing one manually is:

- **Tedious** — dozens of sections to cover
- **Error-prone** — easy to miss a permission or SDK
- **Legally risky** — SDKs like AdMob and Firebase collect data you may not even realise you're sharing
- **Time-consuming to maintain** — every new permission or SDK requires a policy update

PrivacyPilot solves this by reading your actual project files and generating a policy that reflects exactly what your app does.

---

## What It Does

**PrivacyPilot automates the entire privacy policy workflow:**

1. Reads your `AndroidManifest.xml` — detects every `<uses-permission>` tag
2. Reads your `build.gradle` / `libs.versions.toml` — detects every third-party SDK
3. Maps permissions and SDKs to the exact data they collect
4. Generates a complete, mobile-friendly HTML privacy policy
5. Deploys it to GitHub Pages at a standard URL
6. Gives you the URL to paste directly into Play Console

---

## Install

```bash
/plugin marketplace add SUDARSHANCHAUDHARI/PrivacyPilot
/plugin install privacypilot@privacypilot
```

**First-time configuration:** When you enable the plugin, Claude Code automatically prompts you for your developer details:

| Field | Example |
|---|---|
| Developer name | Your Name |
| Company | YourCompany |
| Contact email | you@example.com |
| GitHub username | your-username |
| Country | Your Country |

These are stored globally in your Claude Code settings. You enter them once — every project uses them automatically.

---

## Commands

| Command | Description |
|---|---|
| `/privacypilot:generate` | Scan manifest + Gradle, generate complete privacy policy HTML |
| `/privacypilot:audit` | Audit an existing policy for compliance gaps |
| `/privacypilot:github-page` | Deploy the generated policy to GitHub Pages |
| `/privacypilot:update` | Update an existing policy when permissions or SDKs change |

---

## Quick Start

### Step 1 — Generate your policy

```
/privacypilot:generate
```

You will be asked for:
- Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
- Path to `build.gradle.kts` or `build.gradle`
- App name (display name shown in Play Store)
- Package name (e.g. `com.example.yourapp`)

Output:
```
PRIVACY POLICY GENERATED
─────────────────────────
Permissions found: CAMERA, USE_BIOMETRIC, POST_NOTIFICATIONS
SDKs detected: AdMob, Firebase Analytics, Firebase Crashlytics
Data collected: Advertising ID, device info, crash logs, app usage
Third-party sharing: Google

File saved: privacy-policy/index.html
Privacy policy URL: https://your-username.github.io/yourapp-privacy-policy/

Next: Run /privacypilot:github-page to deploy
```

### Step 2 — Deploy to GitHub Pages

```
/privacypilot:github-page
```

PrivacyPilot will:
1. Show a deployment plan for your review
2. Ask for explicit confirmation before any git operations
3. Push `privacy-policy/index.html` to `[appname-lowercase]-privacy-policy` on GitHub
4. Give you the live URL

**URL format:**
```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

### Step 3 — Add URL to Play Console

In Play Console:
`Store Presence → Store Settings → Privacy policy URL`

Paste the GitHub Pages URL. Done.

---

## Auditing an Existing Policy

```
/privacypilot:audit
```

Scans your current manifest and Gradle against an existing policy and reports:

```
PRIVACY POLICY AUDIT
─────────────────────
App: YourApp

GAPS FOUND:
✗ AdMob detected in build.gradle but not disclosed in policy
✗ ACCESS_FINE_LOCATION declared in manifest — no location section found

WARNINGS:
⚠ Data retention period not specified

COMPLIANT:
✓ Contact information present
✓ Children's privacy section present
✓ Third-party links included

VERDICT: Needs Update
```

---

## Updating After a New Release

```
/privacypilot:update
```

When you add new permissions or SDKs between releases, run this command. It diffs your current manifest and Gradle against the existing policy and shows exactly what needs to change:

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

---

## What Gets Detected

### Android Permissions

| Permission | Data Collected | Sensitivity |
|---|---|---|
| `ACCESS_FINE_LOCATION` | Precise GPS location | Very High |
| `ACCESS_COARSE_LOCATION` | Approximate location | High |
| `ACCESS_BACKGROUND_LOCATION` | Continuous background location | Very High |
| `CAMERA` | Photos / videos | High |
| `READ_CONTACTS` | Contact names, numbers, emails | High |
| `WRITE_CONTACTS` | Contact modification | High |
| `READ_PHONE_STATE` | Device ID, IMEI | High |
| `READ_PHONE_NUMBERS` | Phone numbers | High |
| `READ_EXTERNAL_STORAGE` | Files, photos, documents | Medium-High |
| `READ_MEDIA_IMAGES` | Photos | Medium |
| `READ_MEDIA_VIDEO` | Videos | Medium |
| `READ_MEDIA_AUDIO` | Audio files | Medium |
| `RECORD_AUDIO` | Audio / voice recordings | High |
| `USE_BIOMETRIC` | Biometric authentication patterns | Very High |
| `USE_FINGERPRINT` | Fingerprint data | Very High |
| `GET_ACCOUNTS` | Account list on device | Medium |
| `SEND_SMS` | SMS messages | High |
| `READ_SMS` | SMS content | Very High |
| `RECEIVE_SMS` | Incoming SMS | High |
| `CALL_PHONE` | Phone calls | High |
| `READ_CALL_LOG` | Call history | High |
| `BLUETOOTH` | Nearby Bluetooth devices | Medium |
| `NFC` | NFC tag data | Medium |
| `ACCESS_WIFI_STATE` | WiFi network name/info | Low |
| `INTERNET` | Network access | Low |
| `VIBRATE` | Haptic feedback | None |
| `RECEIVE_BOOT_COMPLETED` | Device boot awareness | Low |

### Third-Party SDKs

| SDK | Gradle Identifier | Data Collected |
|---|---|---|
| **Google AdMob** | `com.google.android.gms:play-services-ads` | Advertising ID, device info, IP address |
| **Firebase Analytics** | `com.google.firebase:firebase-analytics` | App usage, events, device info |
| **Firebase Crashlytics** | `com.google.firebase:firebase-crashlytics` | Crash logs, device info, app state |
| **Firebase Auth** | `com.google.firebase:firebase-auth` | Email, phone, social auth tokens |
| **Firebase Firestore** | `com.google.firebase:firebase-firestore` | User-provided data |
| **Firebase Messaging** | `com.google.firebase:firebase-messaging` | Push notification token |
| **Firebase Performance** | `com.google.firebase:firebase-perf` | Performance metrics, network requests |
| **Facebook Audience Network** | `com.facebook.android:audience-network-sdk` | Device info, location, app activity |
| **Adjust** | `com.adjust.sdk:adjust-android` | Install referrer, device ID, events |
| **AppsFlyer** | `com.appsflyer:af-android-sdk` | Install source, events, device info |
| **Branch** | `io.branch.sdk.android:library` | Device ID, app events |
| **Unity Ads** | `com.unity3d.ads:unity-ads` | Device ID, gameplay data |
| **AppLovin** | `com.applovin:applovin-sdk` | Device info, behavioral data |
| **Retrofit** | `com.squareup.retrofit2:retrofit` | Data you send to your backend |
| **Coil / Glide / Picasso** | Image loading libraries | No user data collected |

---

## Compliance Coverage

### Google Play Data Safety
Aligns your policy with the Data Safety form in Play Console. Covers:
- Data types collected and why
- Whether data is shared with third parties
- Whether data is encrypted in transit
- User data deletion options

### GDPR (European Union)
Covers all required elements for EU users:
- Data controller identity
- Legal basis for processing
- Data subject rights (access, correction, deletion, portability)
- Data transfers outside the EU (Standard Contractual Clauses)
- Retention periods

### CCPA (California)
Includes required California Consumer Privacy Act disclosures:
- Right to know what data is collected
- Right to delete personal information
- Statement on not selling personal information

### COPPA (Children under 13)
Includes a children's privacy section. If your app is targeted at children under 13, additional legal counsel is recommended.

---

## How It Works — Agent Pipeline

When you run `/privacypilot:generate`, three specialised agents work in sequence:

```
AndroidManifest.xml ──► manifest-reader ──┐
                                           ├──► policy-writer ──► privacy-policy/index.html
build.gradle ────────► sdk-detector ──────┘
```

**manifest-reader** — parses `AndroidManifest.xml`, extracts all `<uses-permission>` tags, categorises them by sensitivity, and identifies exported components.

**sdk-detector** — reads `build.gradle` / `build.gradle.kts` / `libs.versions.toml`, matches dependency identifiers against a known database of SDKs and their data collection practices.

**policy-writer** — receives the combined output from both agents and generates a complete, Play Store-compliant HTML privacy policy using the detected permissions and SDKs.

---

## GitHub Pages URL Convention

PrivacyPilot uses a consistent URL pattern across all your apps:

```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

Examples:
| App Name | GitHub Pages URL |
|---|---|
| `SpendWise` | `https://your-username.github.io/spendwise-privacy-policy/` |
| `BudgetTracker` | `https://your-username.github.io/budgettracker-privacy-policy/` |
| `FitnessLog` | `https://your-username.github.io/fitnesslog-privacy-policy/` |

Each app gets its own dedicated GitHub repository for its privacy policy. This URL goes directly into Play Console → Store Presence → Store Settings → Privacy Policy.

### One-time GitHub Pages setup per app
1. Create repo: `[github-username]/[appname-lowercase]-privacy-policy`
2. Push `index.html` to `main` branch
3. Enable GitHub Pages: Settings → Pages → Source: main / root
4. URL goes live in 1–2 minutes

---

## Requirements

- [Claude Code](https://claude.ai/code) installed and authenticated
- Git installed and configured on your machine
- GitHub account
- Android project with `AndroidManifest.xml` and `build.gradle`

---

## Plugin Structure

```
PrivacyPilot/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest + userConfig fields
│   └── marketplace.json         # Marketplace manifest
├── commands/
│   ├── generate.md              # /privacypilot:generate
│   ├── audit.md                 # /privacypilot:audit
│   ├── github-page.md           # /privacypilot:github-page
│   └── update.md                # /privacypilot:update
├── agents/
│   ├── manifest-reader.md       # Parses AndroidManifest.xml
│   ├── sdk-detector.md          # Detects third-party SDKs in Gradle
│   └── policy-writer.md         # Generates HTML privacy policy
├── skills/
│   ├── permission-data-map/
│   │   └── SKILL.md             # Permission → data type mapping
│   ├── sdk-disclosure-rules/
│   │   └── SKILL.md             # SDK disclosure requirements
│   ├── gdpr-compliance/
│   │   └── SKILL.md             # GDPR requirements reference
│   ├── play-data-safety/
│   │   └── SKILL.md             # Play Console Data Safety guide
│   └── github-pages-deploy/
│       └── SKILL.md             # GitHub Pages deployment guide
├── templates/
│   └── privacy-policy-template.html
├── README.md
├── CHANGELOG.md
└── LICENSE
```

---

## Troubleshooting

**"SDK not detected"**
If your Gradle uses a version catalog (`libs.versions.toml`), provide that file path in addition to `build.gradle.kts`. The sdk-detector agent reads both.

**"GitHub Pages returning 404"**
Go to: Repo Settings → Pages → Source → Deploy from branch → `main` → `/ (root)` → Save. Takes 1–2 minutes to go live.

**"Permission not found in policy"**
Run `/privacypilot:audit` to identify missing permissions. Then run `/privacypilot:update` to regenerate only the missing sections.

**"Policy URL not accepted by Play Console"**
The URL must be publicly accessible. Verify GitHub Pages is enabled and the URL returns HTTP 200 before submitting to Play Console.

**"libs.versions.toml not scanned"**
When prompted for the Gradle file path, provide the path to `libs.versions.toml` alongside `build.gradle.kts`. Both files will be scanned.

---

## FAQ

**Does PrivacyPilot generate legally binding privacy policies?**
PrivacyPilot generates accurate, comprehensive privacy policies based on your app's actual permissions and SDKs. For apps targeting sensitive demographics or handling regulated data, review the output with a legal professional.

**Does it work with Kotlin DSL (build.gradle.kts)?**
Yes. Both `build.gradle` (Groovy) and `build.gradle.kts` (Kotlin DSL) are supported.

**What if I use a version catalog (libs.versions.toml)?**
Provide the path to `libs.versions.toml` when prompted. The sdk-detector agent will read it alongside your Gradle file.

**Do I need to regenerate the policy every release?**
Only if you add or remove permissions or SDKs. Use `/privacypilot:update` to diff what changed and update only the relevant sections.

**Can I customise the generated policy?**
Yes. The output is a plain HTML file saved to `privacy-policy/index.html` in your project. Edit it directly before deploying.

---

## Author

**SUDARSHANCHAUDHARI** — [github.com/SUDARSHANCHAUDHARI](https://github.com/SUDARSHANCHAUDHARI)
SudarshanTechLabs | sudarshantechlabs@gmail.com

---

## License

MIT — see [LICENSE](LICENSE)
