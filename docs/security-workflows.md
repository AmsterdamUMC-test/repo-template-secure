# How This Project Helps Protect Your Data

This project includes automated checks and setup files that help reduce the risk of accidentally uploading sensitive data to GitHub.

These checks are not perfect — but together, they catch most common mistakes before they cause problems.

---

## 1. `.gitignore`: Prevent Files from Being Tracked

This project includes a `.gitignore` file that tells Git:

> "Don't track or upload any files with these extensions."

This includes file types like:

- `.csv`, `.xlsx`, `.sav`, `.RData` (data files)
- `.env`, `.key`, `.pem` (secrets and credentials)
- `.nii`, `.dcm`, `.fastq` (imaging and genomics data)

If you try to add one of these files with a normal `git add`, Git will ignore it silently.

---

## 2. Optional: Pre-Commit and Pre-Push Hooks (Before Upload)

If you want extra safety **before** files are ever uploaded to GitHub, you can install `pre-commit`. This runs automatic checks:

- **Pre-commit hook**: Runs when you type `git commit` — checks the files you're about to commit
- **Pre-push hook**: Runs when you type `git push` — checks everything before it leaves your computer

### How to enable it (once per project)

1. Install `pre-commit`:

   ```bash
   pip install pre-commit
   ```

2. Enable both hooks in your project folder:

   ```bash
   pre-commit install
   pre-commit install --hook-type pre-push
   ```

After that, the hooks will run automatically. You can always bypass them with `--no-verify` (not recommended).

### What the hooks check

| Hook | When it runs | What it does |
|------|--------------|--------------|
| `check-forbidden-filetypes` | Before commit | Blocks data files (`.csv`, `.nii`, etc.) |
| `check-forbidden-filetypes-prepush` | Before push | Final check before code leaves your machine |
| `trailing-whitespace` | Before commit | Cleans up extra spaces |
| `end-of-file-fixer` | Before commit | Ensures files end with newline |
| `check-added-large-files` | Before commit | Warns about files > 100KB |
| `check-merge-conflict` | Before commit | Blocks unresolved merge conflicts |

---

## 3. GitHub Actions: Automatic Checks (After Upload)

Each time you upload changes to GitHub (a "push" or pull request), GitHub will run an automated check.

- If everything looks okay, your changes are accepted.
- If you accidentally added a file with a sensitive extension (like `.csv` or `.env`), the check will fail, and GitHub will show a red ❌ with a message.

> ⚠️ Important: This check runs **after** your code is already uploaded. It does **not prevent** you from uploading files — it just notifies you if something went wrong.

That's why we recommend:
1. Keeping repositories **private by default**
2. Installing the pre-push hook as an extra safety net

---

## Summary: Three Layers of Safety

| Layer | When it runs | What it catches | Can be bypassed? |
|-------|--------------|-----------------|------------------|
| `.gitignore` | Before `git add` | Prevents tracking sensitive file types | Yes (`git add -f`) |
| Pre-commit hook | Before `git commit` | Blocks forbidden files, large files, formatting | Yes (`--no-verify`) |
| Pre-push hook | Before `git push` | Final check before upload | Yes (`--no-verify`) |
| GitHub Actions | After `git push` | Blocks PRs with forbidden files | No |

No single step is perfect — but together, they make mistakes much less likely.

If you ever have doubts, ask your project lead or data steward before pushing your code.