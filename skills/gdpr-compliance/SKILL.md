---
name: gdpr-compliance
description: GDPR and international privacy law requirements for Android apps. Use when generating a privacy policy, assessing GDPR/CCPA/COPPA compliance, or writing data subject rights sections.
---

## GDPR Compliance for Android Apps

### Who GDPR applies to
GDPR applies if your app is distributed to users in the European Union, regardless of where you (the developer) are based. Even if you are based outside the EU, GDPR applies to your EU users.

### Required elements in GDPR-compliant privacy policy

**Data controller identity**
- Name / company name: {config.developer.company}
- Contact: {config.developer.email}
- Country: {config.developer.country}

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
"Residents of the European Economic Area (EEA) have additional rights under the General Data Protection Regulation (GDPR). You may contact us at {config.developer.email} to exercise your rights of access, correction, deletion, or portability."

### CCPA (California)
For California users:
"California residents may have additional rights under the California Consumer Privacy Act (CCPA). We do not sell your personal information."

### COPPA (Children under 13)
If app is NOT targeted at children under 13:
"Our app is not directed to children under 13 years of age. We do not knowingly collect personal information from children under 13."

If app IS targeted at children under 13 — additional COPPA compliance required. Contact legal counsel.
