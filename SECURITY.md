# Security Policy

This repository is part of the Amsterdam UMC research environment. To protect patient privacy and research integrity, we enforce strict security controls throughout the Git development lifecycle.

---

## Scope

This policy covers **GitHub-related threats**, including:

- Accidental commits of confidential data (patient data, credentials, research datasets)
- Unauthorized access to private repositories
- Secrets or API keys in code
- Sensitive data remaining in Git history

**Out of scope:** Data handling within secure processing environments (e.g., myDRE, SURF Research Cloud).

---

## Protection Layers

We use a multi-layered approach to prevent data leaks:

| Layer | When | What It Does |
|-------|------|--------------|
| `.gitignore` | Always | Prevents Git from tracking sensitive file types |
| `pre-commit` hook | On `git commit` | Blocks commits containing forbidden files locally |
| `pre-push` hook | On `git push` | Final local check before code leaves your machine |
| GitHub Actions | On push/PR | Server-side enforcement — fails if forbidden files detected |
| Telemetry alerts | On violation | Security team notified immediately of any blocked files |

---

## Forbidden File Types

The following file types are **blocked at all layers** and cannot be committed:

### Data Files
| Category | Extensions |
|----------|------------|
| Tabular data | `.csv`, `.tsv`, `.xlsx`, `.xls` |
| Statistical formats | `.sav`, `.dta`, `.RData`, `.rds`, `.por`, `.sas7bdat`, `.xpt` |
| Binary data | `.feather`, `.parquet`, `.pickle`, `.pkl` |
| HDF5 | `.h5`, `.hdf5`, `.he5`, `.fast5` |
| Databases | `.sqlite`, `.db` |

### Medical & Research Data
| Category | Extensions |
|----------|------------|
| Neuroimaging | `.nii`, `.nii.gz`, `.dcm` |
| Biosignals | `.edf`, `.bdf`, `.eeg`, `.vhdr`, `.vmrk` |
| Genomics | `.fastq`, `.fastq.gz`, `.fasta`, `.bam`, `.sam`, `.vcf`, `.gtf`, `.gff`, `.bed` |

### Credentials & Secrets
| Category | Extensions/Files |
|----------|------------------|
| Environment files | `.env`, `.env.*` |
| Keys & certificates | `.key`, `.pem`, `.pfx`, `.crt`, `.p12`, `.jks` |
| SSH keys | `id_rsa`, `id_ed25519` |

### Other
| Category | Extensions |
|----------|------------|
| Archives | `.zip`, `.tar.gz`, `.7z`, `.rar` |
| Media (may contain patient data) | `.mp4`, `.avi`, `.mov`, `.wav` |
| JSON/XML | Blocked by default with exceptions for config files |

For the complete list, see [`.gitignore`](.gitignore) (patterns between `# BEGIN FORBIDDEN` and `# END FORBIDDEN`).

---

## Setup Instructions

### 1. Install Pre-Commit Hooks (Required)

Every contributor must install the local hooks:

```bash
# Install pre-commit
pip install pre-commit

# Enable both commit and push hooks
pre-commit install
pre-commit install --hook-type pre-push
```

This provides two layers of local protection before code ever reaches GitHub.

### 2. Verify Your Setup

Test that the hooks are working:

```bash
# This should fail
echo "test" > test.csv
git add test.csv
git commit -m "test"  # Should be blocked!

# Clean up
rm test.csv
```

---

## Best Practices

### Data Handling

- **Never commit data files** — even "anonymized" data may contain identifiers
- **Use the `/data/` folder** for local data. It's excluded from Git by .gitignore.

### Secrets & Credentials

- **Never commit secrets** — no API keys, passwords, or tokens in code
- **Use `.env` files locally** — they're blocked from commits
- **Use GitHub Secrets** for CI/CD workflows
- **Rotate any exposed credential immediately**

### General Hygiene

- Start repositories as **private** by default
- Review changes before committing: `git diff --staged`
- Keep commits small and focused
- Write meaningful commit messages

---

## What Happens on Violation

If you try to commit a forbidden file:

### Locally (pre-commit/pre-push)

```
❌ Forbidden file detected: data.csv
   Blocked by: *.csv pattern in central-gitignore.txt

   This file cannot be committed. If you believe this is
   an error, contact your data steward.
```

Your commit/push will be blocked. No data leaves your machine.

### On GitHub (if local hooks bypassed)

1. The GitHub Action fails
2. The security team receives an automated alert with:
   - Repository name
   - File(s) that triggered the alert
   - Who pushed the commit
   - Timestamp
3. An issue is created for tracking
4. You will be contacted for remediation

---

## Accidentally Committed Sensitive Data?

**Act immediately.** Git keeps full history — deleting a file doesn't remove it from the repository.

### Step 1: Stop and Assess

- **Don't panic**, but don't delay
- **Don't try to fix it yourself** if you're unsure — you might make it worse
- Note down: what file, what repo, when committed, is the repo public or private?

### Step 2: Report

Contact the security team immediately:

- **Email:** [security contact email]
- **Do NOT open a public GitHub issue**

Include:

- Repository name
- Description of what was exposed
- Approximate time of commit

### Step 3: Remediation (with guidance)

The security team will help you:

1. **Contain:** Make repo private if public
2. **Remove from history:** Using `git filter-repo` or BFG Repo-Cleaner
3. **Force push:** Replace the repository history
4. **Notify collaborators:** They must re-clone
5. **Invalidate secrets:** Rotate any exposed credentials
6. **Document:** Record the incident per institutional policy

### Tools for History Cleaning

```bash
# Using BFG (easier)
bfg --delete-files "*.csv" my-repo.git

# Using git filter-repo (more powerful)
git filter-repo --path data.csv --invert-paths
```

**Warning:** These tools rewrite Git history. All collaborators must re-clone the repository afterward.

---

## Repository Access

- **Enable 2FA** on your GitHub account (required)
- **Keep repositories private** unless explicitly approved for public release
- **Review collaborator access** regularly
- **Before making a repo public:**
  - [ ] Security scan passes
  - [ ] Full history reviewed for sensitive data
  - [ ] README, LICENSE, and documentation complete
  - [ ] Approved by project lead / data steward

---

## Reporting a Vulnerability

If you discover a security issue:

1. **Do NOT open a public issue**
2. Contact the repository owner or security team privately
3. Include:
   - Repository name
   - Description of the vulnerability
   - Steps to reproduce (if applicable)
4. **Do NOT include actual sensitive data** in your report

---

## References

- [GitHub Security Best Practices](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- [git filter-repo](https://github.com/newren/git-filter-repo)
- [Amsterdam UMC Data Management Policy](#) <!-- Add your institutional link -->
- [GDPR Guidelines for Research](https://gdpr.eu/)

---

## Questions?

- Check [docs/faq.md](docs/faq.md) for common questions
- Contact your data steward or project lead
- See [CONTRIBUTING.md](CONTRIBUTING.md) for collaboration guidelines
