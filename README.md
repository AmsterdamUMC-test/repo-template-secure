# Research Project Template (Secure & Reproducible)

This repository is based on a GitHub template designed for researchers. It helps you:

- Organize your analysis
- Protect sensitive data from being uploaded by accident
- Make your work easier to understand and repeat

---

## What's Included

| File or Folder             | Purpose                                                   |
|----------------------------|-----------------------------------------------------------|
| `.github/workflows/`       | Automated checks to block sensitive files                 |
| `.pre-commit-config.yaml`  | Optional local safety checks before committing            |
| `.gitignore`               | Prevents Git from tracking large or confidential files    |
| `.github/CODEOWNERS`       | Defines repository maintainers and reviewers              |
| `CITATION.cff`             | Machine-readable citation file for citing your repo       |
| `/data/`                   | Local-only folder for sensitive data (excluded from Git)  |
| `README-template.md`       | Fill-in template for your actual project documentation    |
| `SECURITY.md`              | How data and code are protected                           |
| `CONTRIBUTING.md`          | Guidance for contributors                                 |
| `docs/`                    | Plain-language guides on security, data, and reproducibility |

---

## Getting Started

1. Click the green **"Use this template"** button on GitHub.
2. Create a **new, private** repository for your research project.
3. Clone your new repository to your local machine.

## First Steps After Creating Your Repo

- [ ] Replace this README with [`README-template.md`](README-template.md), filling in your project details. Keep this README somewhere else for reference.
- [ ] Update `CITATION.cff` with your project details
- [ ] Install pre-commit hooks (see below)
- [ ] Add collaborators if needed (Settings → Collaborators)
- [ ] Review `.gitignore` and add project-specific patterns

---

## Recommended: Enable Pre-Commit and Pre-Push Hooks

We strongly recommend installing `pre-commit` to catch sensitive files **before** they're committed or pushed.

```bash
# Install pre-commit
pip install pre-commit

# Enable both hooks
pre-commit install
pre-commit install --hook-type pre-push
```

This gives you two layers of protection:
- **Pre-commit**: Checks files when you run `git commit`
- **Pre-push**: Final check when you run `git push`

See [docs/security-workflows.md](docs/security-workflows.md) for details.

---

## More Help

- [docs/overview.md](docs/overview.md): what this template is for
- [docs/data-handling.md](docs/data-handling.md): how to work safely with data
- [docs/security-workflows.md](docs/security-workflows.md): what the automatic checks do
- [docs/faq.md](docs/faq.md): common questions, answered

---

## Security Notice

This template helps prevent common mistakes, but always double-check before committing anything sensitive.
If you're not sure, ask someone on your team or see [`SECURITY.md`](SECURITY.md).

## Accidentally Committed Sensitive Data?

**Don't panic, but act fast:**

1. Do NOT try to fix it yourself — this can make it worse
2. Contact [security team / data steward email] immediately
3. See [`SECURITY.md`](SECURITY.md) for detailed steps

Time matters — the sooner you report, the easier it is to fix.

### Note on Public Repos

We recommend that all new repositories start private by default.
Only make a repo public once:

- You're sure no sensitive data has ever been committed
- The security-scan GitHub Action passes
- You've reviewed your README.md, LICENSE, and code

---

_This template was created to support responsible, open, and reproducible research using GitHub._
