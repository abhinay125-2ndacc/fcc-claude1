# Project Setup Guide

Complete setup instructions for the `fcc-claude1` repository from scratch.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Repository Cloning](#repository-cloning)
- [Python Setup](#python-setup)
- [Dependency Installation](#dependency-installation)
- [Codespace Initialization](#codespace-initialization)
- [Verification Checklist](#verification-checklist)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting, ensure you have:
- **Git** (v2.30+)
- **Python** (v3.10+)
- **GitHub CLI** (`gh`) (v2.0+)
- **Node.js** (v18+) - for MCP servers
- **VS Code** with **Remote - Containers** extension (for local development)

---

## Repository Cloning

### Option 1: HTTPS (Recommended)
```bash
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1
```

### Option 2: SSH
```bash
git clone git@github.com:abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1
```

### Option 3: GitHub CLI
```bash
gh repo clone abhinay125-2ndacc/fcc-claude1
cd fcc-claude1
```

---

## Python Setup

### Verify Python Version
```bash
python3 --version
# Should output: Python 3.10.x or higher
```

### Create Virtual Environment (Recommended)
```bash
python3 -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows
```

### Upgrade pip
```bash
pip install --upgrade pip
```

---

## Dependency Installation

### Install Development Dependencies
```bash
# Install all dev dependencies from pyproject.toml
pip install -e ".[dev]"
```

### Install Pre-commit Hooks
```bash
pre-commit install
pre-commit install --hook-type commit-msg
```

### Verify Installation
```bash
# Check all tools are available
black --version
isort --version
flake8 --version
pytest --version
pre-commit --version
```

---

## Codespace Initialization

When opening a **new GitHub Codespace**, run these commands in order:

### 1. Initial Setup (Run Once)
```bash
# Ensure we're in the right directory
cd /workspaces/fcc-claude1

# Verify Python and tools
python3 --version
which python3

# Install dependencies if not already installed
pip install -e ".[dev]"

# Install pre-commit hooks
pre-commit install
pre-commit install --hook-type commit-msg
```

### 2. Verify Environment
```bash
# Run pre-commit on all files to verify
pre-commit run --all-files

# Run tests
pytest -v

# Run formatters
black --check .
isort --check .
flake8 .
```

### 3. Configure Git (If Needed)
```bash
# Set user info if not configured
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Verify remote
git remote -v
```

### 4. Verify VS Code Extensions
The workspace recommends these extensions (auto-prompted on open):
- `ms-python.python`
- `ms-python.black-formatter`
- `ms-python.isort`
- `ms-python.flake8`
- `eamodio.gitlens`
- `usernamehw.errorlens`
- `gruntfuggly.todo-tree`
- `github.vscode-pull-request-github`
- `github.vscode-github-actions`
- `streetsidesoftware.code-spell-checker`
- `redhat.vscode-yaml`
- `tamasfe.even-better-toml`

Install all recommended extensions when prompted.

---

## Verification Checklist

After setup, verify everything works:

| Check | Command | Expected Result |
|-------|---------|-----------------|
| Python version | `python3 --version` | 3.10+ |
| Git configured | `git config user.name` | Your name |
| Remote correct | `git remote -v` | origin → github.com/... |
| Virtual env active | `which python` | .venv/bin/python |
| Black installed | `black --version` | 24.4.2 |
| isort installed | `isort --version` | 5.13.2 |
| flake8 installed | `flake8 --version` | 7.1.1 |
| pytest installed | `pytest --version` | 8.2.1 |
| pre-commit works | `pre-commit run --all-files` | All pass |
| Tests pass | `pytest -v` | All pass |
| Code formatted | `black --check .` | No changes needed |
| Imports sorted | `isort --check .` | No changes needed |
| Linting clean | `flake8 .` | No errors |
| VS Code extensions | Open Extensions panel | All recommended installed |

---

## Troubleshooting

### Python Version Issues
```bash
# If Python 3.10+ not available
# On Ubuntu/Debian:
sudo apt update && sudo apt install python3.10 python3.10-venv

# On macOS with Homebrew:
brew install python@3.10
```

### Permission Errors
```bash
# If pip install fails with permissions
pip install --user -e ".[dev]"
# Or use virtual environment
python3 -m venv .venv && source .venv/bin/activate
```

### Pre-commit Failures
```bash
# Fix formatting issues automatically
black .
isort .

# Fix trailing whitespace and EOF
pre-commit run trailing-whitespace end-of-file-fixer --all-files
```

### Codespace-Specific Issues
```bash
# If Codespace has stale state
rm -rf .venv __pycache__ .pytest_cache
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install
```

---

## Quick Reference Commands

```bash
# Format code
black .
isort .

# Lint code
flake8 .

# Run tests
pytest -v
pytest --cov=.

# Pre-commit all
pre-commit run --all-files

# Update dependencies
# Update dependencies
pip install --upgrade -e ".[dev]"

# Clean cache
rm -rf __pycache__ .pytest_cache .mypy_cache
find . -name "*.pyc" -delete
```

---

**Next Steps:**
- Read [Development Workflow](development-workflow.md) for daily development practices
- Read [Claude Code Setup](claude-code-setup.md) for AI assistant configuration
- Read [VS Code Setup](vscode-setup.md) for editor configuration
