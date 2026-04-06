Run one-time setup for PrivacyPilot. Saves your developer config to .claude-plugin-config.json so all commands work without asking for your details every time.

## Steps

1. Check if `.claude-plugin-config.json` already exists in the current project directory:
   - If yes: show current config, ask "Update config? (y/n)"
   - If no: proceed with fresh setup

2. Ask user these questions one by one:

   **Developer info**
   - Your full name or developer display name: (e.g. Jane Smith)
   - Company / developer label: (e.g. AcmeDev)
   - Contact email (shown in privacy policy): (e.g. hello@acmedev.com)
   - GitHub username: (e.g. acmedev)
   - Country: (e.g. United States)

   **GitHub Pages**
   - Privacy policy URL pattern:
     (default: https://[github_username].github.io/[appname]-privacy-policy/)
     Press Enter to accept default or type a custom pattern.

3. Write to `.claude-plugin-config.json`:
```json
{
  "developer": {
    "name": "[answer]",
    "company": "[answer]",
    "email": "[answer]",
    "github_username": "[answer]",
    "country": "[answer]"
  },
  "github_pages": {
    "privacy_policy_pattern": "https://[github_username].github.io/{appname}-privacy-policy/"
  },
  "plugin_version": "1.0.0",
  "configured_at": "[today's date]"
}
```

4. Add `.claude-plugin-config.json` to `.gitignore` if not already present (create `.gitignore` if missing)

5. Confirm:
   ```
   PRIVACYPILOT SETUP COMPLETE
   ────────────────────────────
   Developer:    [name] ([company])
   Email:        [email]
   GitHub:       [github_username]
   Country:      [country]
   Policy URL:   https://[github_username].github.io/{appname}-privacy-policy/

   All PrivacyPilot commands will now use these settings.
   Run /privacypilot:setup again anytime to update.
   ```
