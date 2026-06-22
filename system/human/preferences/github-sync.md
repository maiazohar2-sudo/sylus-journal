---
description: Maia's requirement to push all memory changes to GitHub main
---
# GitHub Sync — Push to Main

Maia requires that EVERY time I write something new or update existing files (journal entries, system files, songs, relationships, preferences — anything), I MUST push the changes to GitHub `main` branch immediately. This is non-negotiable and applies to ALL file changes in memory.

**Recorded June 22, 2026** — Maia's instruction: "when you update system or journal its important to push to github ok"

## Push Method

1. Commit to git locally (master branch)
2. Push to GitHub `main`: use token in URL to push master:main
   ```
   git push https://maiazohar2-sudo:$GITHUB_TOKEN@github.com/maiazohar2-sudo/sylus-journal.git master:main
   ```
3. Also push to `letta-journal` branch for redundancy
4. Don't leave the token in the remote URL after pushing

## Handling Divergent Histories

The GitHub `main` branch and the local `master` branch have unrelated histories (the `main` branch was created by the Railway backend with its own files). When a direct push is rejected because `main` has newer commits, use the cherry-pick approach:

1. Fetch the remote `main`: `git fetch https://maiazohar2-sudo:$GITHUB_TOKEN@github.com/maiazohar2-sudo/sylus-journal.git main`
2. Create a temp branch from FETCH_HEAD: `git checkout -b temp-main FETCH_HEAD`
3. Cherry-pick the new commit: `git cherry-pick <commit-hash>`
4. Push to main: `git push https://maiazohar2-sudo:$GITHUB_TOKEN@github.com/maiazohar2-sudo/sylus-journal.git temp-main:main`
5. Switch back to master: `git checkout master`
6. Clean up: `git branch -D temp-main`

**Avoid** `git rebase` on top of `main` — it causes merge conflicts in journal files. The cherry-pick approach is cleaner.

### When Cherry-Pick Conflicts on Journal Files

Journal files (especially `personal.md`) frequently cause cherry-pick conflicts because both branches contain the same file with different appended content. When this happens, **copy files directly** instead of cherry-picking:

1. Fetch remote `main` and create temp branch: `git fetch ... main && git checkout -b temp-main FETCH_HEAD`
2. Copy the files directly from master: `git checkout master -- <file>` (or `git show master:<file> > <file>` for individual files)
3. Commit on temp-main: `git add -A && git commit -m "..."` 
4. Push: `git push ... temp-main:main`
5. Switch back to master: `git checkout master`
6. Clean up: `git branch -D temp-main`

This avoids the three-way merge that causes conflicts. The local `master` branch is the source of truth — just copy its current state of each file onto the temp branch.
