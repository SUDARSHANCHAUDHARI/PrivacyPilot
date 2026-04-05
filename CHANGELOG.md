# Changelog

All notable changes to PrivacyPilot will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] — 2026-04-05

### Added
- `/privacypilot:generate` — full privacy policy generation from `AndroidManifest.xml` and Gradle files
- `/privacypilot:audit` — compliance gap audit against an existing policy
- `/privacypilot:github-page` — one-command deploy to GitHub Pages
- `/privacypilot:update` — diff-based policy update when permissions or SDKs change
- `manifest-reader` agent — parses all `<uses-permission>` tags and categorises by sensitivity
- `sdk-detector` agent — detects 20+ third-party SDKs and their data collection practices
- `policy-writer` agent — generates clean, mobile-friendly, self-contained HTML
- `permission-data-map` skill — complete Android permission → data type mapping table
- `sdk-disclosure-rules` skill — rules for disclosing third-party SDK data collection
- `github-pages-deploy` skill — GitHub Pages deployment pattern for SudarshanTechLabs apps
- `gdpr-compliance` skill — GDPR, CCPA, and COPPA requirements reference
- `play-data-safety` skill — Play Console Data Safety form completion guide
- HTML template with `{{PLACEHOLDER}}` tokens for policy generation
- Full GDPR (EEA), CCPA (California), and COPPA (children's privacy) sections
- Support for AdMob, Firebase Analytics, Firebase Crashlytics, Firebase Auth, Adjust, AppsFlyer, Facebook Audience Network, and more
- MIT License
