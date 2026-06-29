# Dependencies

Complete dependency catalog for the `fcc-claude1` repository.

## Table of Contents
- [Python Dependencies](#python-dependencies)
- [System Dependencies](#system-dependencies)
- [VS Code Extension Dependencies](#vs-code-extension-dependencies)
- [MCP Server Dependencies](#mcp-server-dependencies)
- [Installation Commands](#installation-commands)
- [Dependency Management](#dependency-management)

---

## Python Dependencies

### Runtime Dependencies
**From `pyproject.toml` `[project.dependencies]`**:
```
# None - this is a script-based project
```

### Development Dependencies
**From `pyproject.toml` `[project.optional-dependencies.dev]`**:

| Package | Version | Purpose |
|---------|---------|---------|
| `black` | `==24.4.2` | Code formatter |
| `isort` | `==5.13.2` | Import sorter |
| `flake8` | `==7.1.1` | Linter |
| `pre-commit` | `==4.0.1` | Git hook framework |
| `pytest` | `==8.2.1` | Test framework |
| `pytest-cov` | `==5.0.0` | Coverage reporting |

### Transitive Dependencies (Auto-installed)
Key transitive dependencies from the above:
- `click` (Black, pytest)
- `tomli` (Black, pyproject.toml parsing)
- `pathspect` (Black)
- `platformdirs` (Black)
- `pyproject-hooks` (build)
- `setuptools` (build)
- `wheel` (build)
- `iniconfig` (pytest)
- `pluggy` (pytest)
- `exceptiongroup` (pytest)
- `attrs` (pytest)

---

## System Dependencies

### Required (For Development)
| Tool | Minimum Version | Install Command |
|------|-----------------|-----------------|
| `git` | 2.30+ | `apt install git` / `brew install git` |
| `python3` | 3.10+ | `apt install python3.10` / `brew install python@3.10` |
| `python3-venv` | 3.10+ | `apt install python3.10-venv` |
| `gh` (GitHub CLI) | 2.0+ | `apt install gh` / `brew install gh` |

### Required (For MCP Servers)
| Tool | Minimum Version | Install Command |
|------|-----------------|-----------------|
| `node` | 18+ | `apt install nodejs` / `brew install node` |
| `npm` | 9+ | Included with Node |

### Optional (For Full Features)
| Tool | Purpose | Install Command |
|------|---------|-----------------|
| `docker` | Container testing | `apt install docker.io` |
| `make` | Build automation | `apt install make` |
| `jq` | JSON processing | `apt install jq` |

---

## VS Code Extension Dependencies

### Recommended Extensions (from `.vscode/extensions.json`)

| Extension ID | Name | Purpose |
|--------------|------|---------|
| `ms-python.python` | Python | Core Python support |
| `ms-python.black-formatter` | Black Formatter | Black integration |
| `ms-python.isort` | isort | Import sorting |
| `ms-python.flake8` | Flake8 | Linting integration |
| `eamodio.gitlens` | GitLens | Git visualization |
| `usernamehw.errorlens` | Error Lens | Inline diagnostics |
| `gruntfuggly.todo-tree` | Todo Tree | TODO comment tracking |
| `github.vscode-pull-request-github` | GitHub PR | PR review |
| `github.vscode-github-actions` | GitHub Actions | Workflow editing |
| `streetsidesoftware.code-spell-checker` | Code Spell Checker | Spelling check |
| `redhat.vscode-yaml` | YAML | YAML support |
| `tamasfe.even-better-toml` | Even Better TOML | TOML support |

### Install All Recommended
```bash
# Via VS Code: Extensions panel → "Show Recommended Extensions" → Install All

# Or via CLI (if code CLI available)
code --install-extension ms-python.python
code --install-extension ms-python.black-formatter
code --install-extension ms-python.isort
code --install-extension ms-python.flake8
code --install-extension eamodio.gitlens
code --install-extension usernamehw.errorlens
code --install-extension gruntfuggly.todo-tree
code --install-extension github.vscode-pull-request-github
code --install-extension github.vscode-github-actions
code --install-extension streetsidesoftware.code-spell-checker
code --install-extension redhat.vscode-yaml
code --install-extension tamasfe.even-better-toml
```

---

## MCP Server Dependencies

### Playwright
| Dependency | Version | Purpose |
|------------|---------|---------|
| `playwright` | Latest | Browser automation |
| `playwright-python` | Latest | Python bindings |
| Browsers: Chromium, Firefox, WebKit | Latest | Test targets |

**Install**:
```bash
pip install playwright
playwright install
```

### Superpowers
| Dependency | Version | Purpose |
|------------|---------|---------|
| Built-in | - | No external deps |

### Context7
| Dependency | Version | Purpose |
|------------|---------|---------|
| Built-in | - | No external deps (uses API) |

---

## Installation Commands

### Complete Setup (Fresh Environment)

```bash
# 1. System dependencies (Ubuntu/Debian)
sudo apt update && sudo apt install -y \
    git python3.10 python3.10-venv gh nodejs npm

# 2. Clone repository
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# 3. Python environment
python3 -m venv .venv
source .venv/bin/activate

# 4. Python dependencies
pip install --upgrade pip
pip install -e ".[dev]"

# 5. Pre-commit hooks
pre-commit install
pre-commit install --hook-type commit-msg

# 6. Playwright browsers (for MCP)
playwright install

# 7. Verify
pre-commit run --all-files
pytest -v
```

### Minimal Setup (Codespace - Auto)
```bash
# In GitHub Codespace, most is pre-installed
cd /workspaces/fcc-claude1
pip install -e ".[dev]"
pre-commit install
playwright install  # If using Playwright MCP
```

### Update Dependencies
```bash
# Update Python packages to latest compatible
pip install --upgrade -e ".[dev]"

# Update pre-commit hooks
pre-commit autoupdate

# Update Playwright browsers
playwright install --with-deps
```

---

## Dependency Management

### Version Pinning Strategy
- **Direct dependencies**: Pinned to exact versions (`==`)
- **Transitive dependencies**: Not pinned (resolved by pip)
- **Security updates**: Manual review via `pip-audit` or GitHub Dependabot

### Adding New Dependencies
```bash
# Runtime dependency
# Edit pyproject.toml [project.dependencies]
# Then:
pip install -e ".[dev]"

# Dev dependency
# Edit pyproject.toml [project.optional-dependencies.dev]
# Then:
pip install -e ".[dev]"
```

### Removing Dependencies
```bash
# Edit pyproject.toml to remove
# Then recreate venv:
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
```

### Security Auditing
```bash
# Install audit tool
pip install pip-audit

# Scan for vulnerabilities
pip-audit

# Or use GitHub's Dependabot (configure in .github/dependabot.yml)
```

### Lock File (Not Committed)
```bash
# Generate for reproducible builds
pip freeze > requirements-lock.txt

# Install from lock
pip install -r requirements-lock.txt
```

---

## Verification Checklist

After installation:
- [ ] `python3 --version` ≥ 3.10
- [ ] `black --version` = 24.4.2
- [ ] `isort --version` = 5.13.2
- [ ] `flake8 --version` = 7.1.1
- [ ] `pytest --version` = 8.2.1
- [ ] `pre-commit --version` = 4.0.1
- [ ] `playwright --version` works
- [ ] `gh --version` works
- [ ] `git --version` works

---

## Related Documentation
- [Setup Guide](setup.md)
- [Architecture](architecture.md#configuration-files)
- [MCP Servers](mcp-servers.md)
- [Troubleshooting](troubleshooting.md)
