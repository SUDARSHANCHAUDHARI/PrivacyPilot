Deploy the generated privacy policy to GitHub Pages.

## Steps

1. Ask for:
   - App name (e.g. YourApp)
   - Confirm the target repo name: `[appname-lowercase]-privacy-policy`

2. Check that `privacy-policy/index.html` exists in the current project. If not, stop and tell the user to run `/privacypilot:generate` first.

3. Show the deployment plan:
   ```
   DEPLOYMENT PLAN
   ────────────────
   Repo: ${user_config.github_username}/[appname-lowercase]-privacy-policy
   GitHub Pages URL: https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/
   Branch: main
   File: index.html
   ```

4. Remind the user to create the GitHub repo first if it doesn't exist:
   - Go to github.com/new
   - Name: `[appname-lowercase]-privacy-policy`
   - Set to Public (required for GitHub Pages)
   - Do NOT initialise with README

5. Ask for explicit confirmation before any git operations.

6. After confirmation, run these commands:
   ```bash
   cd privacy-policy

   # Initialise git only if not already a git repo
   git rev-parse --git-dir > /dev/null 2>&1 || git init

   # Set remote — update if it already exists
   git remote get-url origin > /dev/null 2>&1 \
     && git remote set-url origin https://github.com/${user_config.github_username}/[appname-lowercase]-privacy-policy.git \
     || git remote add origin https://github.com/${user_config.github_username}/[appname-lowercase]-privacy-policy.git

   git add index.html
   git commit -m "Update privacy policy — $(date +%Y-%m-%d)"
   git push -u origin main
   ```

7. After push, remind the user to enable GitHub Pages if this is the first deployment:
   - Repo Settings → Pages → Source: Deploy from branch → main → / (root) → Save
   - URL goes live in 1–2 minutes

8. Verify the URL is accessible:
   `https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/`

9. Remind the user to add this URL to Play Console:
   `Store Presence → Store Settings → Privacy policy URL`
