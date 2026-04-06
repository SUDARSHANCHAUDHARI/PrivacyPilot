---
name: github-pages-deploy
description: Deploying privacy policy HTML to GitHub Pages for Android apps
trigger: when deploying or updating a privacy policy to GitHub Pages
---

## GitHub Pages Deployment Pattern

### URL convention
`https://{config.developer.github_username}.github.io/[appname-lowercase]-privacy-policy/`

Examples (replace `johndoe` with your GitHub username):
- App `MyFamilyTracker` → `https://johndoe.github.io/myfamilytracker-privacy-policy/`
- App `BatteryGuard` → `https://johndoe.github.io/batteryguard-privacy-policy/`

### Repository naming
`[appname-lowercase]-privacy-policy`

Example: `myfamilytracker-privacy-policy`

### One-time setup per app
1. Create repo on GitHub: `{config.developer.github_username}/[appname-lowercase]-privacy-policy`
2. Push `index.html` to main branch
3. Enable GitHub Pages: repo Settings → Pages → Source: main branch / root
4. URL goes live in 1–2 minutes

### Update workflow
```bash
# In the policy repo directory
git add index.html
git commit -m "Update privacy policy — $(date +%Y-%m-%d)"
git push origin main
```

### Verify deployment
After push, check:
`https://{config.developer.github_username}.github.io/[appname-lowercase]-privacy-policy/`

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
Enter: `https://{config.developer.github_username}.github.io/[appname-lowercase]-privacy-policy/`
