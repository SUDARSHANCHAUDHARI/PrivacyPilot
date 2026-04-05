# PrivacyPilot — Claude Code Plugin

> Scan your `AndroidManifest.xml` and Gradle files, generate a fully compliant privacy policy, and deploy it to GitHub Pages — all from one command.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Code-blueviolet)
![Target](https://img.shields.io/badge/target-Android%20%2F%20Google%20Play-brightgreen)

---

## What It Does

Android developers are required to provide a privacy policy on Google Play. Writing one manually is tedious, error-prone, and easy to get wrong — especially when SDKs like AdMob or Firebase silently collect data that must be disclosed.

**PrivacyPilot automates the entire process:**

1. Reads your `AndroidManifest.xml` to detect every permission
2. Reads your `build.gradle` / `libs.versions.toml` to detect every third-party SDK
3. Maps permissions and SDKs to their exact data collection implications
4. Generates a clean, mobile-friendly HTML privacy policy
5. Deploys it to GitHub Pages at your standard URL

---

## Install

```bash
/plugin marketplace add SUDARSHANCHAUDHARI/PrivacyPilot
/plugin install privacypilot
```

---

## Commands

| Command | Description |
|---|---|
| `/privacypilot:generate` | Scan manifest + Gradle and generate a complete privacy policy HTML |
| `/privacypilot:audit` | Audit an existing policy for compliance gaps |
| `/privacypilot:github-page` | Deploy the generated policy to GitHub Pages |
| `/privacypilot:update` | Update an existing policy when permissions or SDKs change |

---

## Quick Start

### Generate a policy from scratch

```
/privacypilot:generate
```

You will be asked for:
- Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
- Path to `build.gradle.kts` or `build.gradle`
- App name, package name, contact email

Output:
```
PRIVACY POLICY GENERATED
─────────────────────────
Permissions found: CAMERA, USE_BIOMETRIC, POST_NOTIFICATIONS
SDKs detected: AdMob, Firebase Analytics, Firebase Crashlytics
Data collected: Advertising ID, device info, crash logs, app usage
Third-party sharing: Google

File saved: privacy-policy/index.html
Privacy policy URL: https://[your-github-username].github.io/myapp-privacy-policy/

Next: Run /privacypilot:github-page to deploy
```

### Deploy to GitHub Pages

```
/privacypilot:github-page
```

Pushes `privacy-policy/index.html` to your GitHub Pages repo following the naming convention:
`https://[username].github.io/[appname-lowercase]-privacy-policy/`

### Audit an existing policy

```
/privacypilot:audit
```

Output example:
```
PRIVACY POLICY AUDIT
─────────────────────
App: MyApp

GAPS FOUND:
✗ AdMob detected in build.gradle but not disclosed in policy
✗ ACCESS_FINE_LOCATION declared in manifest — no location section found

COMPLIANT:
✓ Contact information present
✓ Children's privacy section present
✓ Data retention policy present

VERDICT: Needs Update
```

### Update after a release

```
/privacypilot:update
```

Diffs your current manifest and Gradle against the existing policy and shows exactly what sections need to be added, updated, or removed.

---

## What Gets Detected

### Permissions
| Permission | Data Type |
|---|---|
| `ACCESS_FINE_LOCATION` | Precise GPS location |
| `ACCESS_COARSE_LOCATION` | Approximate location |
| `CAMERA` | Photos / videos |
| `READ_CONTACTS` | Contact names, numbers, emails |
| `READ_PHONE_STATE` | Device ID, IMEI |
| `USE_BIOMETRIC` | Biometric authentication |
| `RECORD_AUDIO` | Audio / voice |
| `SEND_SMS` / `READ_SMS` | SMS messages |
| `READ_EXTERNAL_STORAGE` | Files, photos, documents |
| + more | See `skills/permission-data-map.md` |

### Third-Party SDKs
| SDK | Data Collected |
|---|---|
| Google AdMob | Advertising ID, device info, IP address |
| Firebase Analytics | App usage, events, device info |
| Firebase Crashlytics | Crash logs, device info, app state |
| Firebase Auth | Email, phone, auth tokens |
| Facebook Audience Network | Device info, location, app activity |
| Adjust / AppsFlyer | Install referrer, events, device ID |
| + more | See `skills/sdk-disclosure-rules.md` |

---

## Compliance Coverage

- **Google Play** — Data Safety form alignment
- **GDPR** — EEA data subject rights, legal basis, data transfers
- **CCPA** — California consumer rights
- **COPPA** — Children's privacy statement

---

## GitHub Pages URL Convention

PrivacyPilot follows this URL pattern:

```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

Examples (replace `johndoe` with your GitHub username):
- `MyFamilyTracker` → `https://johndoe.github.io/myfamilytracker-privacy-policy/`
- `BatteryGuard` → `https://johndoe.github.io/batteryguard-privacy-policy/`

This URL goes directly into Play Console → Store Presence → Store Settings → Privacy Policy.

---

## Requirements

- [Claude Code](https://claude.ai/code) installed
- Git installed and configured
- GitHub account with Pages enabled on target repos

---

## Plugin Structure

```
PrivacyPilot/                        # repo root = plugin root
├── .claude-plugin/
│   ├── marketplace.json             # Marketplace manifest
│   └── plugin.json                  # Plugin manifest
├── commands/
│   ├── generate.md                  # /privacypilot:generate
│   ├── audit.md                     # /privacypilot:audit
│   ├── github-page.md               # /privacypilot:github-page
│   └── update.md                    # /privacypilot:update
├── agents/
│   ├── manifest-reader.md           # Parses AndroidManifest.xml
│   ├── sdk-detector.md              # Detects third-party SDKs in Gradle
│   └── policy-writer.md             # Generates HTML privacy policy
├── skills/
│   ├── permission-data-map.md       # Permission → data type mapping
│   ├── sdk-disclosure-rules.md      # SDK disclosure requirements
│   ├── github-pages-deploy.md       # GitHub Pages deployment guide
│   ├── gdpr-compliance.md           # GDPR requirements reference
│   └── play-data-safety.md          # Play Console Data Safety guide
├── templates/
│   └── privacy-policy-template.html
├── README.md
├── LICENSE
└── CHANGELOG.md
```

---

## Troubleshooting

**"Permission not found in policy"**
Run `/privacypilot:audit` to identify which permissions are missing. Then run `/privacypilot:update` to regenerate.

**"GitHub Pages returning 404"**
After pushing, go to: Repo Settings → Pages → Source → Deploy from branch → `main` → `/root`. Takes 1–2 minutes to go live.

**"SDK not detected"**
If your Gradle uses a version catalog (`libs.versions.toml`), provide that file path in addition to `build.gradle.kts`. The `sdk-detector` agent reads both.

**"Policy URL not accepted by Play Console"**
The URL must be publicly accessible. Verify GitHub Pages is enabled and the URL loads before submitting to Play Console.

---

## Author

**SUDARSHANCHAUDHARI** — [github.com/SUDARSHANCHAUDHARI](https://github.com/SUDARSHANCHAUDHARI)
SudarshanTechLabs | sudarshantechlabs@gmail.com

---

## License

MIT — see [LICENSE](https://github.com/SUDARSHANCHAUDHARI/PrivacyPilot/blob/main/LICENSE)
