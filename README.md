# PrivacyPilot — Claude Code Plugin

> Scan your `AndroidManifest.xml` and Gradle files, generate a fully compliant privacy policy, and deploy it to GitHub Pages — all from one command.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Code-blueviolet)
![Target](https://img.shields.io/badge/target-Android%20%2F%20Google%20Play-brightgreen)

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
| `/privacypilot:generate` | Scan manifest + Gradle and generate a complete privacy policy |
| `/privacypilot:audit` | Audit an existing policy for compliance gaps |
| `/privacypilot:github-page` | Deploy the generated policy to GitHub Pages |
| `/privacypilot:update` | Update an existing policy when permissions or SDKs change |

---

## Full Documentation

See [`privacypilot/README.md`](privacypilot/README.md) for complete usage, examples, detected permissions/SDKs, troubleshooting, and plugin structure.

---

## Author

**SUDARSHANCHAUDHARI** — [github.com/SUDARSHANCHAUDHARI](https://github.com/SUDARSHANCHAUDHARI)
SudarshanTechLabs | sudarshantechlabs@gmail.com

## License

[MIT](LICENSE)
