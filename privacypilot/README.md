# PrivacyPilot — Claude Code Plugin

Scan your AndroidManifest.xml and Gradle files, generate a compliant privacy policy, deploy to GitHub Pages — all in one command.

## Install

```bash
/plugin marketplace add SUDARSHANCHAUDHARI/privacypilot-claude-plugin-privacy-policy
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
