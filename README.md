# PrivacyPilot — Claude Code Plugin

> Scan your `AndroidManifest.xml` and Gradle files, generate a Play Store-compliant privacy policy, and deploy it to GitHub Pages — all from Claude Code.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Code-blueviolet)
![Target](https://img.shields.io/badge/target-Android%20%2F%20Google%20Play-brightgreen)

---

## What It Does

1. Reads `AndroidManifest.xml` — detects every permission
2. Reads `build.gradle` / `libs.versions.toml` — detects every third-party SDK
3. Maps permissions and SDKs to the exact data they collect
4. Generates a complete, mobile-friendly HTML privacy policy
5. Deploys it to GitHub Pages and gives you the URL for Play Console

---

## Install

**Step 1 — Add the marketplace**
```bash
/plugin marketplace add SUDARSHANCHAUDHARI/PrivacyPilot
```

**Step 2 — Install the plugin**
```bash
/plugin install privacypilot
```

When you enable the plugin, Claude Code prompts you for your developer details once (name, company, email, GitHub username, country). These are stored globally and used automatically in every project.

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

**1. Generate your policy**
```
/privacypilot:generate
```
Provide your `AndroidManifest.xml` path, `build.gradle` path, app name, and package name. Output is saved to `privacy-policy/index.html`.

**2. Deploy to GitHub Pages**
```
/privacypilot:github-page
```
Reviews the deployment plan, asks for confirmation, then pushes to GitHub Pages.

**3. Add the URL to Play Console**

`Store Presence → Store Settings → Privacy policy URL`

---

## How It Works

```
AndroidManifest.xml ──► manifest-reader ──┐
                                           ├──► policy-writer ──► privacy-policy/index.html
build.gradle ────────► sdk-detector ───────┘
```

- **manifest-reader** — extracts and categorizes all permissions
- **sdk-detector** — identifies third-party SDKs and their data collection practices
- **policy-writer** — generates a complete, Play Store-compliant HTML privacy policy

---

## GitHub Pages URL Convention

```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

This URL goes directly into Play Console → Store Settings → Privacy Policy.

---

## Compliance Coverage

- Google Play Data Safety form
- GDPR (EU users)
- CCPA (California users)
- COPPA (children's privacy statement)

---

## Requirements

- [Claude Code](https://claude.ai/code) installed and authenticated
- Git installed and configured
- GitHub account
- Android project with `AndroidManifest.xml` and `build.gradle`

---

## Plugin Structure

```
PrivacyPilot/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest + userConfig
│   └── marketplace.json         # Marketplace manifest
├── commands/                    # /privacypilot:generate|audit|github-page|update
├── agents/                      # manifest-reader, sdk-detector, policy-writer
├── skills/                      # permission-data-map, sdk-disclosure-rules,
│                                #   gdpr-compliance, play-data-safety, github-pages-deploy
├── templates/
│   └── privacy-policy-template.html
├── README.md
├── CHANGELOG.md
└── LICENSE
```

---

## Troubleshooting

**SDK not detected** — If using a version catalog, provide the `libs.versions.toml` path alongside `build.gradle.kts`.

**GitHub Pages 404** — Go to repo Settings → Pages → Source: main / root → Save. Takes 1–2 minutes.

**Policy URL rejected by Play Console** — Verify the URL returns HTTP 200 before submitting.

---

## Author

**SUDARSHANCHAUDHARI** — [github.com/SUDARSHANCHAUDHARI](https://github.com/SUDARSHANCHAUDHARI)
SudarshanTechLabs | sudarshantechlabs@gmail.com

---

## Documentation

For full details — all detected permissions, all SDKs, compliance coverage, GitHub Pages setup guide, and plugin architecture — see [DOCUMENTATION.md](DOCUMENTATION.md).

---

## License

MIT — see [LICENSE](LICENSE)
