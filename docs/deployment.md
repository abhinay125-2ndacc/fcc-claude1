# Deployment

Deployment procedures for the `fcc-claude1` repository.

## Table of Contents
- [Overview](#overview)
- [Environment Setup](#environment-setup)
- [Deployment Procedures](#deployment-procedures)
- [Validation Steps](#validation-steps)
- [Rollback Procedures](#rollback-procedures)

---

## Overview

This repository is a development environment/template project. There is no traditional "application deployment" - instead, deployment refers to:

1. **Codespace Deployment** - Spinning up development environments
2. **Template Distribution** - Sharing as a project starter
3. **CI/CD Pipeline** - Automated quality checks (future)

---

## Environment Setup

### GitHub Codespaces (Primary)

#### Creating a Codespace
```bash
# Via CLI
gh codespace create --repo abhinay125-2ndacc/fcc-claude1 --branch main

# Via Web
# GitHub.com → Repository → Code → Codespaces → Create codespace
```

#### Codespace Configuration
- **Base Image**: `mcr.microsoft.com/devcontainers/python:3.10`
- **Auto-install**: Python packages from `pyproject.toml`
- **Post-create**: Runs `pip install -e ".[dev]"` and `pre-commit install`

#### Custom Dev Container (Optional)
Create `.devcontainer/devcontainer.json`:
```json
{
  "name": "fcc-claude1",
  "image": "mcr.microsoft.com/devcontainers/python:3.10",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {},
    "ghcr.io/devcontainers/features/node:1": { "version": "18" }
  },
  "postCreateCommand": "pip install -e \".[dev]\" && pre-commit install",
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python",
        "ms-python.black-formatter",
        "ms-python.isort",
        "ms-python.flake8",
        "eamodio.gitlens",
        "usernamehw.errorlens",
        "gruntfuggly.todo-tree",
        "github.vscode-pull-request-github",
        "github.vscode-github-actions",
        "streetsidesoftware.code-spell-checker",
        "redhat.vscode-yaml",
        "tamasfe.even-better-toml"
      ]
    }
  }
}
```

### Local Development
```bash
# Clone
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# Setup
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install

# Open in VS Code
code .
```

---

## Deployment Procedures

### 1. Template Distribution
To use as a template for new projects:

```bash
# Option A: GitHub Template Repository
# 1. Go to repo Settings → Template repository → Enable
# 2. Users click "Use this template"

# Option B: Manual copy
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git new-project
cd new-project
# Remove .git and re-init
rm -rf .git
git init
# Update pyproject.toml, README.md for new project
```

### 2. Sharing Configuration
```bash
# Export key configs for other projects
cp .pre-commit-config.yaml /other/project/
cp pyproject.toml /other/project/
cp -r .vscode /other/project/
cp -r .claude /other/project/
```

### 3. CI/CD Pipeline (Future)
When GitHub Actions are added:

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: '3.10' }
      - run: pip install -e ".[dev]"
      - run: pre-commit run --all-files
      - run: pytest -v
```

---

## Validation Steps

### Pre-Deployment Checklist
- [ ] All tests pass (`pytest -v`)
- [ ] Code formatted (`black --check . && isort --check .`)
- [ ] Linting clean (`flake8 .`)
- [ ] Pre-commit passes (`pre-commit run --all-files`)
- [ ] No secrets in code (`git secrets --scan` or similar)
- [ ] Documentation updated
- [ ] Changelog updated

### Codespace Validation
```bash
# In new Codespace
cd /workspaces/fcc-claude1

# 1. Verify environment
python3 --version
git status

# 2. Verify dependencies
pip list | grep -E '(black|isort|flake8|pytest|pre-commit)'

# 3. Run quality checks
pre-commit run --all-files

# 4. Run tests
pytest -v

# 5. Verify VS Code extensions
# Check Extensions panel for recommended
```

### Template Validation
```bash
# Test as template
cd /tmp
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git test-template
cd test-template
rm -rf .git
git init
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit run --all-files
pytest -v
```

---

## Rollback Procedures

### Codespace Rollback
```bash
# 1. Delete problematic Codespace
gh codespace delete --codespace CODESPACE_NAME

# 2. Create fresh Codespace
gh codespace create --repo abhinay125-2ndacc/fcc-claude1
```

### Repository Rollback
```bash
# 1. Check history
git log --oneline -10

# 2. Reset to known good commit (local)
git reset --hard GOOD_COMMIT_SHA

# 3. Force push (DANGEROUS - coordinate with team)
git push --force-with-lease origin main
```

### Configuration Rollback
```bash
# Restore specific config files
git restore .vscode/settings.json
git restore .claude/settings.json
git restore pyproject.toml
git restore .pre-commit-config.yaml

# Or from specific commit
git restore --source=abc1234 -- pyproject.toml
```

---

## Related Documentation
- [Setup Guide](setup.md)
- [Recovery Guide](recovery-guide.md)
- [Development Workflow](development-workflow.md)
- [Troubleshooting](troubleshooting.md)
