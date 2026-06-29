# Troubleshooting

Common issues and solutions for the `fcc-claude1` repository.

## Table of Contents
- [Quick Diagnostics](#quick Diagnostics](#quick-diagnostics)
- [Python Environment Issues](#python-environment-issues)
- [Git Issues](#git-issues)
- [Pre-commit Issues](#pre-commit-issues)
- [VS Code Issues](#vs-code-issues)
- [Claude Code Issues](#claude-code-issues)
- [MCP Server Issues](#mcp-server-issues)
- [Codespace Issues](#codespace-issues)
- [Dependency Issues](#dependency-issues)
- [Recovery Commands](#recovery-commands)

---

## Quick Diagnostics

Run these first to identify the problem:

```bash
# Check environment
cd /workspaces/fcc-claude1
python3 --version
git status
pip list | grep -E '(black|isort|flake8|pytest|pre-commit)'

# Run all checks
pre-commit run --all-files
pytest -v
```

---

## Python Environment Issues

### Issue: `python3: command not found`
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install python3.10 python3.10-venv

# macOS
brew install python@3.10

# Verify
python3 --version
```

### Issue: `ModuleNotFoundError` for installed packages
```bash
# Wrong Python environment
which python3
# Should show .venv/bin/python3

# Fix: Activate venv
source .venv/bin/activate

# Or reinstall in correct env
pip install -e ".[dev]"
```

### Issue: Virtual environment corrupted
```bash
# Recreate
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install
```

### Issue: Permission denied on pip install
```bash
# Use virtual environment (recommended)
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"

# Or use --user flag
pip install --user -e ".[dev]"
```

---

## Git Issues

### Issue: `git push` rejected (non-fast-forward)
```bash
# Pull first
git pull origin main

# If conflicts, resolve then
git add .
git commit -m "Merge remote changes"
git push origin main
```

### Issue: Local changes overwritten by pull
```bash
# Stash local changes first
git stash
git pull origin main
git stash pop
# Resolve conflicts if any
```

### Issue: Accidentally committed secrets
```bash
# 1. Remove from history (DANGEROUS)
# Use BFG Repo-Cleaner or git filter-repo

# 2. Rotate the secret immediately!
# GitHub → Settings → Developer settings → Personal access tokens

# 3. Force push cleaned history (coordinate with team)
git push --force-with-lease origin main
```

### Issue: Detached HEAD state
```bash
# Create branch from current commit
git checkout -b recover-my-work

# Or return to main
git checkout main
```

---

## Pre-commit Issues

### Issue: Pre-commit fails on Black formatting
```bash
# Auto-fix
black .
isort .

# Then commit
git add .
git commit -m "style: format code"
```

### Issue: Pre-commit fails on flake8
```bash
# Check errors
flake8 .

# Fix manually or
# Some errors auto-fixable with:
# black . && isort .
```

### Issue: Pre-commit hook not running
```bash
# Reinstall hooks
pre-commit uninstall
pre-commit install
pre-commit install --hook-type commit-msg

# Run manually
pre-commit run --all-files
```

### Issue: Pre-commit cache issues
```bash
# Clear cache
pre-commit clean

# Re-run
pre-commit run --all-files
```

---

## VS Code Issues

### Issue: Extensions not installing automatically
```bash
# Manual install
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

### Issue: Python interpreter not found
```bash
# 1. Reload VS Code
# Ctrl+Shift+P → "Developer: Reload Window"

# 2. Select interpreter manually
# Ctrl+Shift+P → "Python: Select Interpreter"
# Enter: /workspaces/fcc-claude1/.venv/bin/python
```

### Issue: Formatting not working on save
```bash
# 1. Check settings
# Ctrl+Shift+P → "Open Workspace Settings (JSON)"
# Verify "editor.formatOnSave": true

# 2. Check formatter
# Ctrl+Shift+P → "Python: Select Default Formatter" → Black

# 3. Run manually
# Ctrl+Shift+P → "Format Document"
```

### Issue: Linting not showing
```bash
# 1. Check flake8 installed in venv
.venv/bin/flake8 --version

# 2. Reload window
# Ctrl+Shift+P → "Developer: Reload Window"

# 3. Check output panel
# View → Output → Python
```

---

## Claude Code Issues

### Issue: "Permission denied" for commands
```bash
# Add to .claude/settings.local.json allow list
{
  "permissions": {
    "allow": [
      "Bash(your-command *)",
      ...
    ]
  }
}

# Restart Claude Code
claude
```

### Issue: Plugins not loading
```bash
# 1. Check settings.local.json
cat .claude/settings.local.json

# 2. Verify plugins enabled
{
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}

# 3. Restart Claude
claude --clear-context
claude
```

### Issue: Settings not taking effect
```bash
# Validate JSON syntax
jq . .claude/settings.json
jq . .claude/settings.local.json

# Fix any syntax errors, then restart
claude
```

---

## MCP Server Issues

### Issue: Playwright browsers not found
```bash
# Install browsers
playwright install

# Or with system deps
playwright install --with-deps
```

### Issue: Context7 not resolving libraries
```bash
# Check network connectivity
curl -I https://context7.com

# Verify API key if using authenticated tier
echo $CONTEXT7_API_KEY
```

### Issue: Superpowers skills not available
```bash
# Verify superpowers plugin enabled
claude plugins list

# Skills should auto-load from plugin
# Try invoking directly
# In Claude: /brainstorming
```

---

## Codespace Issues

### Issue: Codespace fails to start
```bash
# 1. Check GitHub status
# https://www.githubstatus.com

# 2. Delete and recreate
gh codespace delete --codespace NAME
gh codespace create --repo abhinay125-2ndacc/fcc-claude1
```

### Issue: Codespace slow/frozen
```bash
# 1. Check resource usage in Codespace
# Top-right menu → "Codespace details"

# 2. Restart Codespace
# Bottom-left → "Restart Codespace"

# 3. Recreate if persistent
gh codespace delete --codespace NAME
gh codespace create --repo abhinay125-2ndacc/fcc-claude1
```

### Issue: Port forwarding not working
```bash
# 1. Check Ports tab in VS Code
# 2. Ensure port is forwarded
# 3. Try different port
```

---

## Dependency Issues

### Issue: Package version conflicts
```bash
# Recreate clean environment
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
```

### Issue: `pip install` hangs
```bash
# Use faster index
pip install -e ".[dev]" -i https://pypi.org/simple

# Or increase timeout
pip install -e ".[dev]" --timeout 120
```

### Issue: Pre-commit hook versions outdated
```bash
# Update hook versions
pre-commit autoupdate

# Commit changes
git add .pre-commit-config.yaml
git commit -m "chore: update pre-commit hooks"
```

---

## Recovery Commands

### Nuclear Reset (Clean Slate)
```bash
# 1. Backup any uncommitted work!
cp -r /workspaces/fcc-claude1 /tmp/backup-$(date +%s)

# 2. Fresh clone
cd /tmp
git clone https://github.com/abhinay125-2ndacc/fcc-claude1.git
cd fcc-claude1

# 3. Full setup
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pre-commit install
playwright install

# 4. Verify
pre-commit run --all-files
pytest -v
```

### Reset Specific Components
```bash
# Python only
rm -rf .venv __pycache__ .pytest_cache
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"

# Git hooks only
pre-commit uninstall
pre-commit install
pre-commit install --hook-type commit-msg

# VS Code only
rm -rf .vscode/launch.json .vscode/tasks.json
# Reopen VS Code

# Claude only
claude --clear-context
# Restart claude
```

---

## Getting Help

| Resource | Link |
|----------|------|
| GitHub Issues | https://github.com/abhinay125-2ndacc/fcc-claude1/issues |
| GitHub Discussions | https://github.com/abhinay125-2ndacc/fcc-claude1/discussions |
| Claude Code Docs | https://docs.claude.com/claude-code |
| VS Code Docs | https://code.visualstudio.com/docs |

---

## Related Documentation
- [Setup Guide](setup.md)
- [Recovery Guide](recovery-guide.md)
- [Claude Code Setup](claude-code-setup.md)
- [VS Code Setup](vscode-setup.md)
