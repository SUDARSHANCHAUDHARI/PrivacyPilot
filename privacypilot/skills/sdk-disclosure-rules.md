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
