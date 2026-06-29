# Disaster Recovery Guide

Complete disaster recovery procedures for the `fcc-claude1` repository.

## Table of Contents
- [Overview](#overview)
- [Scenario 1: Codespace Deleted](#scenario-1-codespace-deleted)
- [Scenario 2: GitHub Account Lost](#scenario-2-github-account-lost)
- [Scenario 3: Local Machine Changes](#scenario-3-local-machine-changes)
- [Scenario 4: Repository Corruption](#scenario-4-repository-corruption)
- [Repository Restoration Process](#repository-restoration-process)
- [Backup Strategy](#backup-strategy)
- [Emergency Contacts](#emergency-contacts)

---

## Overview

This guide covers recovery from various disaster scenarios. The repository is designed to be fully reconstructible from GitHub alone.

**Key Principle:** All configuration, code, and documentation live in the Git repository. No local-only state is required.

---

## Scenario 1: Codespace Deleted

### Symptoms
- Codespace no longer exists in GitHub Codespaces dashboard
- All local files in Codespace are gone
- Repository on GitHub is intact

### Recovery Steps

```bash
# 1. Create new Codespace from GitHub
# Go to: https://github.com/abhinay125-2ndacc/fcc-claude1
# Click "Code" → "Codespaces" → "Create codespace on main"

# 2. Wait for Codespace to initialize (2-3 minutes)

# 3. Run setup verification
cd /workspaces/fcc-claude1
./docs/scripts/verify-setup.sh  # If script exists, otherwise run manual checks

# 4. Verify all components
pre-commit run --all-files
pytest -v
black --check .
isort --check .
flake8 .
```

### What You Lose (and How to Recover)
| Item | Stored In | Recovery |
|------|-----------|----------|
| Source code | Git repo | `git clone` |
| Configuration | Git repo | `git clone` |
| Documentation | Git repo | `git clone` |
| VS Code settings | Git repo (`.vscode/`) | Auto-applied |
| Python packages | Reinstallable | `pip install -e ".[dev]"` |
| Pre-commit hooks | Reinstallable | `pre-commit install` |
| **Uncommitted changes** | **NOWHERE** | **Lost permanently** |

### Time to Recovery: ~5 minutes

---

## Scenario 2: GitHub Account Lost

### Symptoms
- Cannot access GitHub account
- Repository access lost
- No access to GitHub Codespaces

### Recovery Steps

#### If You Have Local Clone
```bash
# 1. Verify local repo has all history
cd /path/to/local/fcc-claude1
git log --oneline -10
git branch -a

# 2. Push to new GitHub account
# Create new repo on new GitHub account
# Then:
git remote rename origin old-origin
git remote add origin https://github.com/NEW_USERNAME/fcc-claude1.git
git push -u origin --all
git push -u origin --tags
```

#### If You Have NO Local Clone
**CRITICAL:** Without a local clone or backup, the repository is **permanently lost**.

### Prevention
- Maintain local clones on multiple machines
- Enable GitHub account recovery (2FA, recovery codes)
- Use GitHub organization (not personal account) for critical repos

---

## Scenario 3: Local Machine Changes

### Symptoms
- Switched to new computer
- OS reinstallation
- Want to work locally instead of Codespace

### Recovery Steps (New Machine)

```bash
# 1. Install prerequisites
# Ubuntu/Debian:
sudo apt update && sudo apt install git python3.10 python3.10-venv gh

# macOS:
brew install git python@3.10 gh

# Windows:
# Install Git for Windows, Python 3.10+, GitHub CLI

# 2. Clone repository
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# 3. Setup Python environment
python3 -m venv .venv
source .venv/bin/activate

# 4. Install dependencies
pip install --upgrade pip
pip install -e ".[dev]"

# 5. Install pre-commit hooks
pre-commit install
pre-commit install --hook-type commit-msg

# 6. Verify
pre-commit run --all-files
pytest -v

# 7. Open in VS Code
code .
# Install recommended extensions when prompted
```

### Recovery Steps (OS Reinstall - Same Machine)
```bash
# If you have the repo folder preserved
cd /path/to/fcc-claude1

# Recreate virtual environment
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install
```

---

## Scenario 4: Repository Corruption

### Symptoms
- Git errors on clone/fetch
- Missing objects
- Corrupted `.git` directory

### Recovery Steps

#### Option 1: Fresh Clone (Recommended)
```bash
# Backup any uncommitted work first!
cp -r /path/to/fcc-claude1 /tmp/fcc-claude1-backup

# Fresh clone
cd /tmp
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# Restore uncommitted work if needed
# cp /tmp/fcc-claude1-backup/your-file.py .
```

#### Option 2: Repair Local Repo
```bash
# Check corruption
git fsck --full

# Try to repair
git gc --prune=now
git fsck --full

# If still corrupted, use Option 1
```

---

## Repository Restoration Process

### Complete Restore from GitHub (Clean State)

```bash
# 1. Clone fresh
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# 2. Setup environment
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -e ".[dev]"

# 3. Install hooks
pre-commit install
pre-commit install --hook-type commit-msg

# 4. Verify everything works
pre-commit run --all-files
pytest -v
black --check .
isort --check .
flake8 .

# 5. Open in VS Code
code .
```

### Restore Specific Files from History

```bash
# Restore a deleted file
git restore --source=HEAD~5 -- path/to/file.py

# Restore file to specific commit
git restore --source=abc1234 -- path/to/file.py

# View file at specific commit
git show abc1234:path/to/file.py > restored_file.py
```

### Restore Entire Repository State

```bash
# Reset to specific commit (DANGEROUS - loses uncommitted work)
git reset --hard abc1234

# Force push to GitHub (DANGEROUS - rewrites history)
git push --force-with-lease origin main
```

---

## Backup Strategy

### Automated Backups (Recommended)

#### 1. Local Machine Cron Job
```bash
# Add to crontab (crontab -e)
# Daily at 2 AM
0 2 * * * /home/user/scripts/backup-fcc-claude1.sh
```

```bash
#!/bin/bash
# /home/user/scripts/backup-fcc-claude1.sh
REPO_DIR="/home/user/projects/fcc-claude1"
BACKUP_DIR="/home/user/backups/fcc-claude1"
DATE=$(date +%Y%m%d-%H%M%S)

mkdir -p "$BACKUP_DIR"
cd "$REPO_DIR"

# Create bundle (includes all refs and objects)
git bundle create "$BACKUP_DIR/fcc-claude1-$DATE.bundle" --all

# Keep last 30 days
find "$BACKUP_DIR" -name "*.bundle" -mtime +30 -delete

# Also backup to cloud (example with rclone)
# rclone copy "$BACKUP_DIR" remote:backups/fcc-claude1/
```

#### 2. GitHub Repository Mirror
```bash
# Create mirror on another Git hosting service
git clone --mirror https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1.git
git remote add backup https://gitlab.com/username/fcc-claude1-mirror.git
git push backup --mirror

# Schedule regular mirror updates
# */6 * * * * cd /path/to/mirror && git fetch origin && git push backup --mirror
```

#### 3. GitHub Codespaces Backup
Codespaces automatically persist the repository. No additional backup needed for committed work.

### What to Backup

| Priority | Item | Method |
|----------|------|--------|
| **Critical** | Git repository (all history) | `git bundle`, mirror |
| **Critical** | Uncommitted work | Manual copy before risky operations |
| **High** | Local config (`.env`, IDE settings) | Manual backup |
| **Medium** | Virtual environment | Recreatable from `pyproject.toml` |
| **Low** | Cache directories | Recreatable |

### Backup Verification

```bash
# Verify bundle integrity
git bundle verify /path/to/backup.bundle

# Test restore from bundle
cd /tmp
git clone /path/to/backup.bundle test-restore
cd test-restore
git log --oneline -5
```

---

## Emergency Contacts

| Resource | URL/Contact |
|----------|-------------|
| GitHub Support | https://support.github.com |
| GitHub Status | https://www.githubstatus.com |
| Repository Issues | https://github.com/abhinay125-2ndacc/fcc-claude1/issues |
| Owner Email | abhinay125-2ndacc@users.noreply.github.com |

---

## Quick Reference: Recovery Commands

```bash
# === FRESH START (Codespace or new machine) ===
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1
python3 -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install
pre-commit run --all-files
pytest -v

# === RECOVER UNCOMMITTED WORK (if any) ===
# Check git status first!
git status
git stash list
git stash pop

# === RESTORE FILE FROM HISTORY ===
git restore --source=HEAD~3 -- path/to/file.py

# === VERIFY REPO INTEGRITY ===
git fsck --full
git bundle verify backup.bundle

# === NUCLEAR OPTION: FRESH CLONE ===
cd /tmp
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
# Copy any uncommitted work from old location
```

---

**Remember:** The only irreplaceable data is **uncommitted work**. Commit early, commit often, push frequently.
