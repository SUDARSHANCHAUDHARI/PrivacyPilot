Deploy the generated privacy policy to GitHub Pages.

## Steps

1. Ask for:
   - GitHub username
   - App name (e.g. MyApp)
   - Confirm the target repo name: `[appname-lowercase]-privacy-policy`

2. Show the deployment plan:
   ```
   DEPLOYMENT PLAN
   ────────────────
   Repo to create/update: [github-username]/[appname-lowercase]-privacy-policy
   GitHub Pages URL: https://[github-username].github.io/[appname-lowercase]-privacy-policy/
   Branch: main
   File: index.html
   ```

3. Ask for explicit confirmation before any git operations

4. After confirmation, run these commands:
   ```bash
   # Navigate to privacy-policy folder
   cd privacy-policy

   # Initialize git if not already done
   git init
   git remote add origin https://github.com/[github-username]/[appname-lowercase]-privacy-policy.git

   # Add, commit, push
   git add index.html
   git commit -m "Update privacy policy — $(date +%Y-%m-%d)"
   git push -u origin main
   ```

5. Remind user to enable GitHub Pages in repo settings:
   - Settings → Pages → Source: Deploy from branch → main → / (root)
   - It takes 1–2 minutes for the URL to go live

6. Verify the URL is accessible after deployment

7. Remind user to add this URL to:
   - Play Console → Store Presence → Store settings → Privacy policy URL
   - The app itself (Settings screen or About screen)
   - AndroidManifest meta-data if declared
