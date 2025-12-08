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
3. Replace this file with the included [`README-template.md`](README-template.md), which is meant for your own project description.

> Tip: If you're working with others, you can create a new branch for your changes using:
>
> ```bash
> git checkout -b my-analysis
> ```

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

### Note on Public Repos

We recommend that all new repositories start private by default.
Only make a repo public once:

- You're sure no sensitive data has ever been committed
- The security-scan GitHub Action passes
- You've reviewed your README.md, LICENSE, and code

---

_This template was created to support responsible, open, and reproducible research using GitHub._