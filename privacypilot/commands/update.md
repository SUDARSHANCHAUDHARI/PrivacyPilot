Update an existing privacy policy when app permissions or SDKs change.

## Steps

1. Ask for:
   - Current privacy policy file location or URL
   - What changed: new permissions / new SDKs / removed features / other

2. Fetch current policy (from file or URL)

3. Re-scan the current manifest and build.gradle

4. Compare:
   - New permissions added since last policy update
   - New SDKs added
   - Permissions/SDKs removed (may need removal from policy)
   - Any policy section that's now outdated

5. Generate a diff of what needs to change:
   ```
   POLICY UPDATE REQUIRED
   ───────────────────────
   ADD these sections:
   - Camera permission disclosure (new in this version)
   - AdMob integration (newly added SDK)
   
   UPDATE these sections:
   - Data retention: update to reflect new 90-day limit
   
   REMOVE these sections:
   - Location permission (removed from manifest)
   ```

6. Generate the updated policy and ask for confirmation before overwriting

7. After update, run github-page command to redeploy
