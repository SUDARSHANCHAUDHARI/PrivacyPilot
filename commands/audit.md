Audit an Android app's manifest and existing privacy policy for compliance gaps.

## Steps

1. Ask for:
   - AndroidManifest.xml path
   - Existing privacy policy URL or file (optional)
   - Build gradle file path

2. Scan and report:

   **Permissions declared but not disclosed**
   - List permissions in manifest that are not explained in privacy policy

   **SDKs detected but not disclosed**
   - Third-party libraries found in gradle that collect data but aren't in the policy

   **Missing required sections**
   - Contact information
   - Data deletion/retention policy
   - Third-party links
   - Children's privacy section (if targeting minors)
   - GDPR section (if distributing in EU)

   **Play Data Safety mismatches**
   - Data collected doesn't match Data Safety form declarations

3. Output audit report:
   ```
   PRIVACY POLICY AUDIT
   ─────────────────────
   App: [name]

   GAPS FOUND:
   ✗ [issue] — [how to fix]

   WARNINGS:
   ⚠ [warning] — [recommendation]

   COMPLIANT:
   ✓ [items that are correctly disclosed]

   VERDICT: [Compliant / Needs Update / Significant Gaps]
   ```

4. Offer to auto-fix by running `/privacypilot:generate`
