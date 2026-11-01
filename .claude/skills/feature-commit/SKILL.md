---
name: feature-commit
description: Commit and push every new feature (or fix) in this repo as soon as it is done, opening a feature branch when the change is large, risky or experimental. Use whenever you finish writing or changing a feature of index.html (מעגל החלטה), or when the user asks to save, commit, push or branch.
---

# Commit and push each feature

Every finished feature gets its own commit and is pushed right away. Don't batch several features into one commit, and don't leave finished work only in the container: the session is ephemeral.

## 1. Pick where the work goes

Start from an up-to-date `main`:

```bash
git fetch origin main
git checkout main && git pull --ff-only origin main
```

- **Small, safe change** (text, colors, a small bug fix, a contained addition): work directly on `main`.
- **Open a branch** `feature/<short-english-name>` (e.g. `feature/export-csv`) when any of these apply:
  - the change is large or touches many parts of the page (data structure, db paths, permissions/capabilities)
  - it is experimental, or the user isn't sure they want it yet
  - it will take several steps or sessions
  - the user asked for a branch

  ```bash
  git checkout -b feature/<name>
  ```

If unsure, prefer a branch and say so in one line.

## 2. Commit

One feature = one commit (several commits are fine on a long branch, each one a working state).

- Stage only the feature's files: `git add index.html README.md` — never `git add -A` blindly.
- Message: a short English summary line in the imperative ("Add CSV export of opinions"), a blank line, then what changed and why if it isn't obvious.
- End the message with the attribution lines from the session's system reminder (Co-Authored-By / Claude-Session), when present.
- If the feature changes what the page does, update `README.md` ("מה יש בדף") in the same commit.

## 3. Push

```bash
git push -u origin <branch>
```

On a network failure retry up to 4 times, waiting 2s, 4s, 8s, 16s. If the push is refused for permissions, stop and tell the user (the Claude GitHub App may need access to `fefrat/maagal-hachlata`).

## 4. Keep the live page in sync

The live page is the artifact https://claude.ai/artifact/GPt1xhEtxMP18JsRs7dSzM, built from `index.html`.

- Change pushed to `main` → republish the artifact from `index.html` to that same URL (read it first if this session hasn't), so the site and the repo match.
- Change on a feature branch → don't touch the live page; it stays on what's in `main`.

## 5. Finishing a branch

When the user approves the branch's feature:

```bash
git checkout main && git pull --ff-only origin main
git merge --no-ff feature/<name> -m "Merge feature/<name>"
git push origin main
git push origin --delete feature/<name>
```

Then republish the artifact (step 4). Open a pull request instead only if the user asks for one.

## 6. Tell the user (in Hebrew, briefly)

After each push: what the feature does, which branch it went to, and whether the live site was updated. Link the commit or branch on GitHub, e.g. `https://github.com/fefrat/maagal-hachlata/tree/feature/<name>`.
