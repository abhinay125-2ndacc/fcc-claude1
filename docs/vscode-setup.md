# VS Code Setup

Complete VS Code workspace configuration for the `fcc-claude1` repository.

## Table of Contents
- [Workspace Settings](#workspace-settings)
- [Recommended Extensions](#recommended-extensions)
- [Workspace Configuration](#workspace-configuration)
- [Recovery Instructions](#recovery-instructions)
- [Customization](#customization)

---

## Workspace Settings

### Settings File (`.vscode/settings.json`)
```json
{
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,
  "editor.defaultFormatter": "ms-python.black-formatter",
  "editor.codeActionsOnSave": {
    "source.organizeImports": "explicit",
    "source.fixAll": "explicit"
  },
  "editor.rulers": [88, 120],
  "editor.tabSize": 4,
  "editor.insertSpaces": true,
  "editor.detectIndentation": true,
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,
  "python.formatting.provider": "black",
  "python.formatting.blackArgs": ["--line-length=88"],
  "python.linting.enabled": true,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Args": ["--max-line-length=88", "--extend-ignore=E203"],
  "python.sortImports.args": ["--profile=black", "--line-length=88"],
  "python.testing.pytestEnabled": true,
  "python.testing.pytestArgs": ["-v", "--tb=short"],
  "python.testing.unittestEnabled": false,
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  },
  "[yaml]": {
    "editor.defaultFormatter": "redhat.vscode-yaml",
    "editor.tabSize": 2
  },
  "[toml]": {
    "editor.defaultFormatter": "tamasfe.even-better-toml",
    "editor.tabSize": 2
  },
  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "git.confirmSync": false
}
```

### Settings Breakdown

#### Editor Behavior
| Setting | Value | Purpose |
|---------|-------|---------|
| `editor.formatOnSave` | `true` | Auto-format on save |
| `editor.formatOnPaste` | `true` | Format pasted code |
| `editor.formatOnType` | `true` | Format while typing |
| `editor.defaultFormatter` | `ms-python.black-formatter` | Use Black for Python |

#### Code Actions on Save
| Setting | Value | Purpose |
|---------|-------|---------|
| `source.organizeImports` | `explicit` | Sort imports on save |
| `source.fixAll` | `explicit` | Fix all linting issues |

#### Visual Guides
| Setting | Value | Purpose |
|---------|-------|---------|
| `editor.rulers` | `[88, 120]` | Vertical lines at 88/120 chars |
| `editor.tabSize` | `4` | 4 spaces per tab |
| `editor.insertSpaces` | `true` | Spaces not tabs |

#### File Handling
| Setting | Value | Purpose |
|---------|-------|---------|
| `files.trimTrailingWhitespace` | `true` | Remove trailing spaces |
| `files.insertFinalNewline` | `true` | Ensure newline at EOF |
| `files.trimFinalNewlines` | `true` | Remove extra newlines |

#### Python Configuration
| Setting | Value | Purpose |
|---------|-------|---------|
| `python.formatting.provider` | `black` | Use Black formatter |
| `python.formatting.blackArgs` | `["--line-length=88"]` | Match pyproject.toml |
| `python.linting.enabled` | `true` | Enable linting |
| `python.linting.flake8Enabled` | `true` | Use flake8 |
| `python.linting.flake8Args` | `["--max-line-length=88", "--extend-ignore=E203"]` | Match config |
| `python.sortImports.args` | `["--profile=black", "--line-length=88"]` | Match isort config |
| `python.testing.pytestEnabled` | `true` | Use pytest |
| `python.testing.pytestArgs` | `["-v", "--tb=short"]` | Verbose, short traceback |
| `python.testing.unittestEnabled` | `false` | Disable unittest |

#### Language-Specific Overrides
| Language | Formatter | Tab Size |
|----------|-----------|----------|
| Python | Black | 4 |
| YAML | redhat.vscode-yaml | 2 |
| TOML | tamasfe.even-better-toml | 2 |

#### Git Integration
| Setting | Value | Purpose |
|---------|-------|---------|
| `git.enableSmartCommit` | `true` | Stage from editor |
| `git.autofetch` | `true` | Auto-fetch remotes |
| `git.confirmSync` | `false` | No confirm on sync |

---

## Recommended Extensions

### Extensions File (`.vscode/extensions.json`)
```json
{
  "recommendations": [
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
```

### Extension Details

| Extension | ID | Purpose | Auto-Install |
|-----------|-----|---------|--------------|
| **Python** | `ms-python.python` | Core Python support (IntelliSense, debugging) | ✅ |
| **Black Formatter** | `ms-python.black-formatter` | Black integration | ✅ |
| **isort** | `ms-python.isort` | Import sorting | ✅ |
| **Flake8** | `ms-python.flake8` | Linting integration | ✅ |
| **GitLens** | `eamodio.gitlens` | Git history, blame, navigation | ✅ |
| **Error Lens** | `usernamehw.errorlens` | Inline error diagnostics | ✅ |
| **Todo Tree** | `gruntfuggly.todo-tree` | Track TODO/FIXME comments | ✅ |
| **GitHub PR** | `github.vscode-pull-request-github` | PR review in editor | ✅ |
| **GitHub Actions** | `github.vscode-github-actions` | Workflow syntax highlighting | ✅ |
| **Spell Checker** | `streetsidesoftware.code-spell-checker` | Code spelling check | ✅ |
| **YAML** | `redhat.vscode-yaml` | YAML language support | ✅ |
| **TOML** | `tamasfe.even-better-toml` | TOML language support | ✅ |

---

## Workspace Configuration

### Opening the Workspace
```bash
# From terminal
code /workspaces/fcc-claude1

# Or from VS Code
# File → Open Folder → /workspaces/fcc-claude1
```

### First Open Actions
1. **Install Recommended Extensions** - Popup appears, click "Install All"
2. **Select Python Interpreter** - `Ctrl+Shift+P` → "Python: Select Interpreter" → `.venv/bin/python`
3. **Trust Workspace** - Click "Yes, I trust the authors" if prompted

### Keybindings (Useful Additions)
Add to `keybindings.json` (`Ctrl+Shift+P` → "Open Keyboard Shortcuts (JSON)"):

```json
[
  {
    "key": "ctrl+shift+f",
    "command": "editor.action.formatDocument",
    "when": "editorHasDocumentFormattingProvider"
  },
  {
    "key": "ctrl+shift+p",
    "command": "python.testing.runAllTests",
    "when": "editorTextFocus"
  },
  {
    "key": "ctrl+shift+t",
    "command": "workbench.action.terminal.new",
    "when": "terminalFocus"
  }
]
```

### Tasks (`.vscode/tasks.json` - Optional)
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Format All",
      "type": "shell",
      "command": "black . && isort .",
      "group": "build",
      "presentation": {
        "reveal": "always"
      }
    },
    {
      "label": "Lint All",
      "type": "shell",
      "command": "flake8 .",
      "group": "test",
      "presentation": {
        "reveal": "always"
      }
    },
    {
      "label": "Test All",
      "type": "shell",
      "command": "pytest -v",
      "group": "test",
      "presentation": {
        "reveal": "always"
      }
    }
  ]
}
```

---

## Recovery Instructions

### If Settings Are Lost
```bash
# Settings are in Git - just pull
git pull origin main

# Or restore from Git
git restore .vscode/settings.json .vscode/extensions.json
```

### If Extensions Don't Install
```bash
# 1. Check extensions.json exists
cat .vscode/extensions.json

# 2. Install manually
code --install-extension ms-python.python
code --install-extension ms-python.black-formatter
# ... repeat for all

# 3. Or use VS Code UI
# Extensions panel → "Show Recommended Extensions"
```

### If Python Interpreter Not Detected
```bash
# 1. Ensure venv exists
ls -la .venv/bin/python

# 2. Reload window
# Ctrl+Shift+P → "Developer: Reload Window"

# 3. Manually select
# Ctrl+Shift+P → "Python: Select Interpreter" → Enter path
```

### If Formatting Doesn't Work
```bash
# 1. Check Black installed in venv
.venv/bin/black --version

# 2. Check formatter setting
# Settings → search "defaultFormatter" → should be "ms-python.black-formatter"

# 3. Run manually
.venv/bin/black .
.venv/bin/isort .
```

### Complete Workspace Reset
```bash
# 1. Close VS Code
# 2. Remove local VS Code state
rm -rf .vscode/launch.json .vscode/tasks.json  # If customized

# 3. Reopen - will use tracked settings
code .
```

---

## Customization

### Per-User Overrides (Not Tracked)
Create `.vscode/settings.local.json` (add to `.gitignore`):
```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "Fira Code",
  "editor.fontLigatures": true,
  "workbench.colorTheme": "GitHub Dark",
  "python.linting.mypyEnabled": true
}
```

### Adding Extensions
1. Install extension in VS Code
2. Add to `.vscode/extensions.json` recommendations array
3. Commit and push

### Modifying Settings
1. Edit `.vscode/settings.json`
2. Test changes work
3. Commit and push

---

## Verification Checklist

After setup:
- [ ] All 12 recommended extensions installed
- [ ] Python interpreter points to `.venv/bin/python`
- [ ] Format on save works (save a .py file)
- [ ] Linting shows inline errors
- [ ] GitLens shows blame annotations
- [ ] Spell checker underlines typos
- [ ] Tasks available (if tasks.json added)

---

## Related Documentation
- [Setup Guide](setup.md)
- [Architecture](architecture.md#vscode-workspace-configuration)
- [Development Workflow](development-workflow.md)
- [Troubleshooting](troubleshooting.md)
