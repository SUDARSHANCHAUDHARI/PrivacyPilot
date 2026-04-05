---
name: permission-data-map
description: Complete mapping of Android permissions to data types collected
trigger: when reading AndroidManifest.xml or generating a privacy policy
---

## Android Permission → Data Collection Mapping

| Permission | Data Type | Sensitivity | Must Disclose |
|---|---|---|---|
| ACCESS_FINE_LOCATION | Precise GPS location | High | Yes |
| ACCESS_COARSE_LOCATION | Approximate location | High | Yes |
| ACCESS_BACKGROUND_LOCATION | Continuous location | Very High | Yes — prominent disclosure required |
| CAMERA | Photos/videos | High | Yes |
| READ_CONTACTS | Contact names, numbers, emails | High | Yes |
| READ_PHONE_STATE | Device ID, IMEI | High | Yes |
| READ_EXTERNAL_STORAGE | Files, photos, documents | Medium-High | Yes |
| READ_MEDIA_IMAGES | Photos | Medium | Yes |
| SEND_SMS | SMS messages | High | Yes |
| READ_SMS | SMS content | Very High | Yes |
| RECORD_AUDIO | Audio/voice | High | Yes |
| USE_BIOMETRIC | Biometric patterns | Very High | Yes |
| GET_ACCOUNTS | Account list | Medium | Yes |
| INTERNET | Network access | Low | Only if sending data to your servers |
| VIBRATE | Haptic feedback | None | No |
| RECEIVE_BOOT_COMPLETED | Boot awareness | Low | No |
| ACCESS_WIFI_STATE | WiFi network name | Low | Mention if used for features |
| BLUETOOTH | Nearby devices | Medium | Yes if collecting device info |
| NFC | NFC tag data | Medium | Yes if reading NFC data |

## Play Data Safety categories
These map to the Data Safety form in Play Console:
- Location → Location (precise or approximate)
- Camera → Photos and videos
- Contacts → Contacts
- Storage → Files and docs / Photos and videos
- Phone state → Device or other IDs
- Microphone → Audio files
- Biometric → Health and fitness / Personal info
