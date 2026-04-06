# PrivacyPilot — Full Documentation

This document covers everything needed to use PrivacyPilot effectively. See [README.md](README.md) for a quick overview.

---

## Table of Contents

1. [Installation & Configuration](#installation--configuration)
2. [Commands](#commands)
   - [/privacypilot:generate](#privacypilotgenerate)
   - [/privacypilot:audit](#privacypilotaudit)
   - [/privacypilot:github-page](#privacypilotgithub-page)
   - [/privacypilot:update](#privacypilotupdate)
3. [Permissions Detected](#permissions-detected)
4. [SDKs Detected](#sdks-detected)
5. [Agent Pipeline](#agent-pipeline)
6. [Configuration Reference](#configuration-reference)
7. [Compliance Coverage](#compliance-coverage)
8. [GitHub Pages Setup Guide](#github-pages-setup-guide)
9. [Plugin Architecture](#plugin-architecture)

---

## Installation & Configuration

### Install the plugin

```bash
/plugin install privacypilot
```

### First-time setup

After installing, Claude Code prompts you to enter your developer details. These are stored globally and reused across all your projects — you set them once.

| Field | Description | Example |
|---|---|---|
| Developer Name | Your full name or display name | `John Smith` |
| Company | Developer label shown in the policy | `YourCompany` |
| Contact Email | Email address shown in the policy | `you@example.com` |
| GitHub Username | Used to construct the GitHub Pages URL | `yourhandle` |
| Country | Your country of operation | `United States` |

To update these values at any time, go to Claude Code Settings → Plugins → PrivacyPilot → Configure.

---

## Commands

### /privacypilot:generate

Scans your `AndroidManifest.xml` and Gradle files, then generates a complete Play Store-compliant privacy policy as `privacy-policy/index.html` in your project.

**What it asks for:**
- Path to `AndroidManifest.xml` (default: `app/src/main/AndroidManifest.xml`)
- Path to `build.gradle.kts` or `build.gradle` (can also provide `libs.versions.toml`)
- App display name (e.g. `My App`)
- Package name (e.g. `com.example.myapp`)

Developer name, email, and country are filled automatically from your plugin configuration.

**What it produces:**
- `privacy-policy/index.html` in your current project directory
- A console summary listing every permission found, every SDK detected, and all data categories that must be disclosed

**Example output:**
```
PRIVACY POLICY GENERATED
─────────────────────────
Permissions found: ACCESS_FINE_LOCATION, CAMERA, INTERNET
SDKs detected: Google AdMob, Firebase Analytics, Firebase Crashlytics
Data collected: Precise location, Photos/videos, Device advertising ID, App usage events
Third-party sharing: Google (AdMob, Firebase)

File saved: privacy-policy/index.html
Privacy policy URL: https://yourhandle.github.io/myapp-privacy-policy/

Next: Run /privacypilot:github-page to deploy to GitHub Pages
```

---

### /privacypilot:audit

Audits an existing privacy policy (or generates one from scratch) against the current state of your manifest and Gradle files. Identifies gaps before you submit to Play Console.

**What it asks for:**
- Path to `AndroidManifest.xml`
- Path to `build.gradle` / `build.gradle.kts`
- Existing privacy policy URL or file path (optional — skipped if you don't have one yet)

**What it checks:**
- Permissions declared in the manifest but not disclosed in the policy
- Third-party SDKs in Gradle that collect data but are not mentioned in the policy
- Required sections: contact information, data retention, third-party links, children's privacy, GDPR rights
- Play Data Safety form alignment — checks that declared data types match what the manifest and SDKs actually collect

**Example output:**
```
PRIVACY POLICY AUDIT
─────────────────────
App: My App

GAPS FOUND:
✗ CAMERA permission declared — not disclosed in policy
✗ AdMob SDK detected — advertising data collection not mentioned

WARNINGS:
⚠ No data retention period stated — recommended for GDPR compliance
⚠ No data deletion/opt-out mechanism described

COMPLIANT:
✓ Contact information present
✓ INTERNET permission (non-sensitive) — no disclosure required

VERDICT: Needs Update
```

If gaps are found, the command offers to run `/privacypilot:generate` to auto-fix them.

---

### /privacypilot:github-page

Deploys your `privacy-policy/index.html` to a dedicated GitHub Pages repository, making the policy publicly accessible at a stable URL.

**Prerequisites:**
- `privacy-policy/index.html` must already exist (run `/privacypilot:generate` first)
- A public GitHub repository named `[appname-lowercase]-privacy-policy` must exist
  - Go to github.com/new → name it `myapp-privacy-policy` → set to Public → do NOT initialise with README

**What it does:**
1. Shows the deployment plan including the target repo, URL, and branch
2. Asks for your explicit confirmation before running any git commands
3. Initialises git in the `privacy-policy/` folder (if not already a repo)
4. Sets the remote to your GitHub repository
5. Commits and pushes `index.html` to the `main` branch
6. Reminds you to enable GitHub Pages in repo Settings if this is the first deployment

**Resulting URL format:**
```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

This URL goes directly into Play Console → Store Presence → Store Settings → Privacy policy URL.

**First deployment only:** After pushing, go to the repo on GitHub → Settings → Pages → Source: Deploy from branch → Branch: main → Folder: / (root) → Save. The page goes live within 1–2 minutes.

---

### /privacypilot:update

Updates an existing privacy policy when your app's permissions or third-party SDKs change between versions.

**What it asks for:**
- Location of the current policy (file path or URL)
- What changed: new permissions, new SDKs, removed features, or other

**What it does:**
1. Fetches the current policy
2. Re-scans the manifest and Gradle files
3. Compares the current state against what the policy already discloses
4. Produces a diff showing exactly what sections need to be added, updated, or removed
5. Generates the updated policy and asks for confirmation before overwriting the file
6. After update, prompts you to run `/privacypilot:github-page` to redeploy

**Example diff output:**
```
POLICY UPDATE REQUIRED
───────────────────────
ADD these sections:
- Camera permission disclosure (new in this version)
- AdMob integration (newly added SDK)

UPDATE these sections:
- Data retention: update to reflect 90-day limit

REMOVE these sections:
- Location permission (removed from manifest)
```

---

## Permissions Detected

The `manifest-reader` agent reads every `<uses-permission>` tag and maps each permission to the exact data it collects. Permissions are grouped by category.

### Sensitive (require explicit disclosure in policy)

| Category | Permission | Data Collected |
|---|---|---|
| Location | ACCESS_FINE_LOCATION | Precise GPS location |
| Location | ACCESS_COARSE_LOCATION | Approximate location |
| Location | ACCESS_BACKGROUND_LOCATION | Continuous background location |
| Camera & Media | CAMERA | Photos and videos |
| Camera & Media | READ_EXTERNAL_STORAGE | Files, photos, documents |
| Camera & Media | READ_MEDIA_IMAGES / VIDEO / AUDIO | Media files (Android 13+) |
| Contacts & Identity | READ_CONTACTS | Contact names, phone numbers, emails |
| Contacts & Identity | WRITE_CONTACTS | Contact modifications |
| Contacts & Identity | GET_ACCOUNTS | Account list on device |
| Contacts & Identity | READ_PHONE_STATE | Device ID, IMEI |
| Contacts & Identity | READ_PHONE_NUMBERS | Phone numbers |
| Communication | SEND_SMS | SMS messages |
| Communication | READ_SMS | SMS content |
| Communication | RECEIVE_SMS | Incoming SMS |
| Communication | CALL_PHONE | Call initiation |
| Communication | READ_CALL_LOG | Call history |
| Biometric & Security | USE_BIOMETRIC | Biometric patterns (fingerprint, face) |
| Biometric & Security | USE_FINGERPRINT | Fingerprint data (legacy) |
| Audio | RECORD_AUDIO | Voice and audio |
| Bluetooth | BLUETOOTH_CONNECT / BLUETOOTH_SCAN | Nearby device identifiers |
| NFC | NFC | NFC tag data |

### Non-sensitive (no disclosure required)

| Permission | Reason |
|---|---|
| INTERNET | Network access — disclose only if sending user data to your server |
| VIBRATE | Haptic feedback — no data |
| POST_NOTIFICATIONS | Notification permission — not data collection |
| FOREGROUND_SERVICE | Service execution — not data collection |
| RECEIVE_BOOT_COMPLETED | Boot startup — no user data |
| ACCESS_WIFI_STATE | WiFi state — mention if used for a feature |

### Play Data Safety mapping

These permission categories map directly to the Google Play Data Safety form:

| Permission Category | Data Safety Form Section |
|---|---|
| Location | Location (Precise / Approximate) |
| Camera | Photos and videos |
| Contacts | Contacts |
| Storage / Media | Files and docs / Photos and videos |
| Phone state | Device or other IDs |
| Microphone / Audio | Audio files |
| Biometric | Personal info |

---

## SDKs Detected

The `sdk-detector` agent reads `build.gradle`, `build.gradle.kts`, and `libs.versions.toml` to identify third-party libraries and their data collection practices.

### Advertising

| SDK | Gradle Identifier | Data Collected | Must Disclose |
|---|---|---|---|
| Google AdMob | `com.google.android.gms:play-services-ads` | Advertising ID, device info, IP address, app usage | Yes |
| Google AdMob Lite | `com.google.android.gms:play-services-ads-lite` | Same as AdMob | Yes |
| Facebook Audience Network | `com.facebook.android:audience-network-sdk` | Device info, location, app activity | Yes |
| Unity Ads | `com.unity3d.ads:unity-ads` | Device ID, gameplay data | Yes |
| AppLovin | `com.applovin:applovin-sdk` | Device info, location, behavioral data | Yes |
| ironSource | `com.ironsource.sdk:mediationsdk` | Device info, ad interactions | Yes |

### Analytics & Crash Reporting

| SDK | Gradle Identifier | Data Collected | Must Disclose |
|---|---|---|---|
| Firebase Analytics | `com.google.firebase:firebase-analytics` | App usage events, device info | Yes |
| Firebase Crashlytics | `com.google.firebase:firebase-crashlytics` | Crash logs, device info, app state | Yes |
| Firebase Performance | `com.google.firebase:firebase-perf` | Performance metrics, network requests | Yes |

### Core Firebase

| SDK | Gradle Identifier | Data Collected | Notes |
|---|---|---|---|
| Firebase Auth | `com.google.firebase:firebase-auth` | Email, phone, social auth tokens | Depends on sign-in method |
| Firebase Firestore | `com.google.firebase:firebase-firestore` | User-provided data | Disclose what you store |
| Firebase Storage | `com.google.firebase:firebase-storage` | Files uploaded by user | Disclose what you accept |
| Firebase Messaging | `com.google.firebase:firebase-messaging` | Device token | Push notifications |
| Firebase Remote Config | `com.google.firebase:firebase-config` | App configuration only | No user data |

### Attribution & Marketing

| SDK | Gradle Identifier | Data Collected | Must Disclose |
|---|---|---|---|
| Adjust | `com.adjust.sdk:adjust-android` | Install referrer, device ID, events | Yes |
| AppsFlyer | `com.appsflyer:af-android-sdk` | Install source, events, device info | Yes |
| Branch | `io.branch.sdk.android:library` | Device ID, app events | Yes |

### Networking

| SDK | Gradle Identifier | Notes |
|---|---|---|
| Retrofit | `com.squareup.retrofit2:retrofit` | Sends whatever data your app specifies to your server |
| OkHttp | `com.squareup.okhttp3:okhttp` | Network transport — disclose based on what data you send |

### Image Loading

| SDK | Gradle Identifier | Notes |
|---|---|---|
| Coil | `io.coil-kt:coil` / `io.coil-kt:coil-compose` | Loads URLs you specify — no user data sent to third parties |
| Glide | `com.github.bumptech.glide:glide` | Same as Coil |
| Picasso | `com.squareup.picasso:picasso` | Same as Coil |

### Machine Learning

| SDK | Gradle Identifier | Notes |
|---|---|---|
| Google ML Kit | `com.google.mlkit:*` | On-device processing — no user data sent to Google by default |

---

## Agent Pipeline

PrivacyPilot uses three specialized agents that run in sequence:

```
AndroidManifest.xml ──► manifest-reader ──┐
                                           ├──► policy-writer ──► privacy-policy/index.html
build.gradle ────────► sdk-detector ───────┘
```

### manifest-reader

- Reads `AndroidManifest.xml`
- Extracts every `<uses-permission>` tag
- Categorizes permissions into sensitivity levels
- Flags `android:exported` components (can indicate data exposure)
- Outputs a structured list of permissions with data collected per permission

### sdk-detector

- Reads `build.gradle`, `build.gradle.kts`, and/or `libs.versions.toml`
- Matches dependency identifiers against a curated table of known SDKs
- For each match, identifies what data the SDK collects and whether disclosure is required
- Outputs a structured list with third-party data sharing requirements

### policy-writer

- Receives the full output from both agents, plus app name, package, and developer details
- Generates a complete HTML privacy policy covering all 10 required sections (see below)
- Saves to `privacy-policy/index.html` in the current project directory
- HTML is self-contained (no CDN dependencies), mobile-friendly, with inline CSS

### Policy sections generated

1. Introduction — what the app does, who operates it
2. Information we collect — broken into: directly provided, automatically collected, third-party collected
3. How we use your information — purpose for each data type
4. Third-party services — each SDK named with a link to its own privacy policy
5. Data retention — how long data is kept
6. Data security — how data is protected
7. Children's privacy — COPPA compliance statement
8. Your rights — data access, deletion, opt-out instructions
9. Changes to this policy — how users will be notified
10. Contact us — developer contact details

---

## Configuration Reference

Configuration is stored globally via Claude Code's plugin `userConfig` system. Values are substituted automatically into every command and agent prompt using `${user_config.KEY}` tokens.

| Key | Title | Description | Used in |
|---|---|---|---|
| `developer_name` | Developer Name | Full name or display name shown in policy | Policy header, Contact section |
| `company` | Company | Developer/company label | Policy header |
| `email` | Contact Email | Email address in policy Contact section | Contact section |
| `github_username` | GitHub Username | Constructs the GitHub Pages URL | `/privacypilot:github-page` |
| `country` | Country | Country of operation for legal compliance context | Policy legal section |

All fields are plain strings. None are marked sensitive (they appear in a public HTML file by design).

---

## Compliance Coverage

### Google Play Data Safety

PrivacyPilot maps each detected permission and SDK directly to the Data Safety form categories in Play Console:

- Identifies which data types are collected
- Identifies which are shared with third parties
- Identifies which are required vs optional
- Generates policy text that matches what you should declare in the form

### GDPR (EU)

The generated policy includes:

- What data is collected and why (legal basis)
- User rights: access, rectification, erasure, portability, objection
- How to contact the controller (developer)
- Data retention periods

### CCPA (California)

The generated policy includes:

- What personal information is collected
- Whether it is sold or shared with third parties
- California residents' right to know, delete, and opt out

### COPPA (Children's Privacy)

All generated policies include a standard COPPA statement: the app does not knowingly collect data from children under 13. If your app targets children, manual review is recommended beyond what PrivacyPilot generates.

---

## GitHub Pages Setup Guide

### Step 1 — Generate the policy

```
/privacypilot:generate
```

This creates `privacy-policy/index.html` in your project directory.

### Step 2 — Create the GitHub repository

1. Go to [github.com/new](https://github.com/new)
2. Name the repo: `[appname-lowercase]-privacy-policy`
   - Example: for an app called "My App" → `myapp-privacy-policy`
3. Set visibility to **Public** (required for GitHub Pages)
4. Do **not** initialise with a README or any files
5. Click **Create repository**

### Step 3 — Deploy

```
/privacypilot:github-page
```

Provide the app name when asked. The command will show a deployment plan and ask for your confirmation before running any git operations.

### Step 4 — Enable GitHub Pages (first time only)

1. Open the repository on GitHub
2. Go to **Settings → Pages**
3. Under **Source**, select **Deploy from a branch**
4. Branch: `main` — Folder: `/ (root)`
5. Click **Save**

The page goes live at:
```
https://[github-username].github.io/[appname-lowercase]-privacy-policy/
```

Takes 1–2 minutes after the first push.

### Step 5 — Add to Play Console

1. Open [Google Play Console](https://play.google.com/console)
2. Select your app
3. Go to **Store presence → Store settings**
4. Paste the GitHub Pages URL into the **Privacy policy** field
5. Save and submit

### Updating after app changes

When you update your app's permissions or add new SDKs:

```
/privacypilot:update
```

After the policy is regenerated, run:

```
/privacypilot:github-page
```

The updated policy is pushed to the same repository and the URL stays the same — no need to update Play Console.

---

## Plugin Architecture

```
PrivacyPilot/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest, metadata, userConfig schema
│   └── marketplace.json         # Marketplace listing metadata
├── commands/
│   ├── generate.md              # /privacypilot:generate
│   ├── audit.md                 # /privacypilot:audit
│   ├── github-page.md           # /privacypilot:github-page
│   └── update.md                # /privacypilot:update
├── agents/
│   ├── manifest-reader.md       # Parses AndroidManifest.xml
│   ├── sdk-detector.md          # Detects SDKs in Gradle files
│   └── policy-writer.md         # Generates HTML privacy policy
├── skills/
│   ├── permission-data-map/     # Android permission → data type mapping table
│   ├── sdk-disclosure-rules/    # SDK disclosure requirement rules
│   ├── gdpr-compliance/         # GDPR section generation rules
│   ├── play-data-safety/        # Play Data Safety form alignment
│   └── github-pages-deploy/     # GitHub Pages deployment logic
├── templates/
│   └── privacy-policy-template.html  # Base HTML structure for generated policy
├── README.md                    # Quick overview
├── DOCUMENTATION.md             # This file — full reference
├── CHANGELOG.md                 # Version history
└── LICENSE                      # MIT
```

### How commands, agents, and skills interact

- **Commands** (`commands/*.md`) — define the user-facing workflow step by step. They orchestrate which agents to spawn and what inputs to pass.
- **Agents** (`agents/*.md`) — specialized workers that handle a specific task (parsing, detecting, writing). Spawned by commands via Claude's multi-agent system.
- **Skills** (`skills/*/SKILL.md`) — reference knowledge loaded by agents when needed. They provide data tables and rules (e.g. the full permission-to-data-type mapping) without requiring agents to carry that knowledge themselves.

This separation means each agent stays focused and the knowledge base (skills) can be updated independently of the workflow logic (commands).
