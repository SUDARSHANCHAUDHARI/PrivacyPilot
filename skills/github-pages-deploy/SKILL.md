---
name: github-pages-deploy
description: Deploying privacy policy HTML to GitHub Pages for Android apps. Use when deploying or updating a privacy policy to GitHub Pages, or explaining the deployment workflow.
---

## GitHub Pages Deployment Pattern

### URL convention
`https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/`

Example:
- App `YourAppName` → `https://your-username.github.io/yourappname-privacy-policy/`

### Repository naming
`[appname-lowercase]-privacy-policy`

Example: `yourapp-privacy-policy`

### One-time setup per app
1. Create repo on GitHub: `${user_config.github_username}/[appname-lowercase]-privacy-policy`
2. Push `index.html` to main branch
3. Enable GitHub Pages: repo Settings → Pages → Source: main branch / root
4. URL goes live in 1–2 minutes

### Update workflow
```bash
# Run this whole block as ONE Bash call: variables do not carry between calls.
# The subshell fails closed if split, and set -eu stops on any error or empty $TMP.
(
set -eu
# Fresh temp clone of the Pages repo. Never run git inside the app repo.
# pwd -P: git reports the resolved path (/private/var/...), mktemp returns /var/...
TMP="$(cd "$(mktemp -d)" && pwd -P)"
: "${TMP:?}"
git clone https://github.com/${user_config.github_username}/[appname-lowercase]-privacy-policy.git "$TMP"

# Copy only the policy file (copy any asset files by name the same way)
cp "$PWD/privacy-policy/index.html" "$TMP/"

# Abort unless the temp dir is its own repo root
[ "$(git -C "$TMP" rev-parse --show-toplevel)" = "$TMP" ] || { echo "ABORT: $TMP is not its own git repo" >&2; exit 1; }

git -C "$TMP" branch -M main
git -C "$TMP" add index.html
git -C "$TMP" commit -m "Update privacy policy — $(date +%Y-%m-%d)"
git -C "$TMP" push -u origin main
)
```

### Verify deployment
After push, check:
`https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/`

If 404, check GitHub Pages is enabled in repo Settings.

### Single file setup
The entire privacy policy should be one self-contained `index.html`:
- No external CSS files
- No external JS files
- No CDN links
- All styling inline or in `<style>` tag
- Loads fast, works offline, no dependencies

### Play Console URL field
In Play Console → Store Presence → Store Settings → Privacy policy:
Enter: `https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/`
