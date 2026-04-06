---
name: play-data-safety
description: How to complete the Play Console Data Safety form accurately. Use when filling out the Data Safety section in Play Console or verifying that a privacy policy matches the Data Safety declaration.
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
- Acceptable: "Email {config.developer.email} to request data deletion"

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
