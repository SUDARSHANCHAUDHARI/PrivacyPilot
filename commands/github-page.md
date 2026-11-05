---
description: Deploy the generated privacy policy to GitHub Pages.
---

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

5. Before asking for confirmation, validate the remote repo is accessible:
   ```bash
   git ls-remote https://github.com/${user_config.github_username}/[appname-lowercase]-privacy-policy.git HEAD
   ```
   - If the command exits 0: repo exists and is reachable. Proceed.
   - If it exits non-zero or errors: stop and tell the user:
     > "The repo `[appname-lowercase]-privacy-policy` does not exist or is private. Please create it on GitHub (public, no README) and try again."
   Do NOT attempt a push until this check passes.

6. Ask for explicit confirmation before any git operations.

7. After confirmation, run these commands:
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

8. After push, remind the user to enable GitHub Pages if this is the first deployment:
   - Repo Settings → Pages → Source: Deploy from branch → main → / (root) → Save
   - URL goes live in 1–2 minutes

9. Verify the URL is accessible:
   `https://${user_config.github_username}.github.io/[appname-lowercase]-privacy-policy/`

10. Remind the user to add this URL to Play Console:
   `Store Presence → Store Settings → Privacy policy URL`
