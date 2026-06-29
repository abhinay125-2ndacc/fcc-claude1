# Claude Code Setup

Complete configuration guide for Claude Code in the `fcc-claude1` repository.

## Table of Contents
- [Installation](#installation)
- [Configuration Files](#configuration-files)
- [Settings Explanation](#settings-explanation)
- [Common Commands](#common-commands)
- [Troubleshooting](#troubleshooting)
- [Advanced Configuration](#advanced-configuration)

---

## Installation

### GitHub Codespace (Automatic)
Claude Code is pre-installed in GitHub Codespaces. Just run:
```bash
claude --version
```

### Local Machine
```bash
# macOS
brew install claude-code

# Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows
# Download from https://claude.ai/download
```

### Verify Installation
```bash
claude --version
# Should output version like: claude-code/1.0.0 ...
```

---

## Configuration Files

### File Locations
```
~/.claude/settings.json          # Global user settings
.workspaces/fcc-claude1/.claude/settings.json      # Project settings (tracked)
.workspaces/fcc-claude1/.claude/settings.local.json # Local overrides (gitignored)
```

### Settings Hierarchy
```
Global (~) → Project (.claude/) → Local (.claude/local) → CLI Flags
     │            │                    │                   │
     ▼            ▼                    ▼                   ▼
  Lowest      Overridden           Overridden         Highest
  Priority    by Project           by Local           Priority
```

---

## Settings Explanation

### Project Settings (`.claude/settings.json`)
**Tracked in Git** - Shared across all developers

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(gh *)",
      "Bash(npm *)",
      "Bash(python *)",
      "Bash(pip *)",
      "Bash(pytest *)",
      "Bash(black *)",
      "Bash(isort *)",
      "Bash(flake8 *)",
      "Bash(pre-commit *)",
      "Bash(cat *)",
      "Bash(ls *)",
      "Bash(find *)",
      "Bash(grep *)",
      "Bash(mkdir *)",
      "Bash(rm *)",
      "Bash(cp *)",
      "Bash(mv *)",
      "Bash(chmod *)",
      "Read(*)",
      "Glob(*)",
      "Grep(*)",
      "Task(*)",
      "Edit(*)",
      "Write(*)",
      "NotebookEdit(*)"
    ]
  }
}
```

**Purpose**: Grants permission for common development commands without prompting.

### Local Settings (`.claude/settings.local.json`)
**NOT tracked in Git** - User-specific overrides

```json
{
  "permissions": {
    "allow": [
      "Bash(python *)",
      "Bash(git add *)",
      "Bash(git commit *)",
      "Bash(git push *)",
      "Bash(gh repo *)",
      "Bash(gh auth *)",
      "Bash(git remote *)",
      "Bash(git init *)",
      "Bash(gh api *)",
      "Bash(unset GITHUB_TOKEN)",
      "Bash(curl *)",
      "Bash(~)",
      "Bash(git config *)",
      "Bash(gh config *)"
    ]
  },
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}
```

**Purpose**:
- Additional permissions for git push, gh auth, etc.
- Enables MCP server plugins

### Permission Rule Syntax
| Pattern | Matches |
|---------|---------|
| `Bash(git *)` | `git`, `git status`, `git commit -m "msg"` |
| `Bash(python *)` | `python script.py`, `python -m pytest` |
| `Read(*)` | All Read operations |
| `Edit(*)` | All Edit operations |
| `*` | All tools (use sparingly) |

---

## Common Commands

### Basic Usage
```bash
# Start interactive session
claude

# Run single command
claude -p "Explain this codebase"

# Continue previous conversation
claude -c

# Show help
claude --help
```

### Project-Specific Commands
```bash
# In project directory
claude

# With specific model
claude --model sonnet

# With verbose output
claude --verbose

# Dangerous mode (bypass permissions)
claude --dangerously-skip-permissions
```

### Session Management
```bash
# List sessions
claude sessions list

# Resume session
claude sessions resume <session-id>

# Clear context
claude --clear-context
```

---

## Troubleshooting

### Permission Denied Errors
```bash
# Check current settings
cat .claude/settings.json
cat .claude/settings.local.json

# Add missing permission
# Edit .claude/settings.local.json and add to allow array
```

### Plugin Issues
```bash
# List enabled plugins
claude plugins list

# Disable problematic plugin
# Edit .claude/settings.local.json, set plugin to false

# Re-enable
# Set to true
```

### Settings Not Loading
```bash
# Check JSON syntax
jq . .claude/settings.json
jq . .claude/settings.local.json

# Restart Claude Code
# Exit and re-run claude
```

### Common Issues

| Issue | Solution |
|-------|----------|
| "Permission denied" | Add command to allow list in settings.local.json |
| Plugin not working | Verify plugin name in settings.local.json |
| Settings ignored | Check JSON syntax with `jq` |
| Model not found | Use correct model alias (sonnet, opus, haiku) |

---

## Advanced Configuration

### Environment Variables
```bash
# Set in shell profile or Codespaces secrets
export CLAUDE_API_KEY="your-key"          # Not needed for Codespaces
export CLAUDE_MODEL="sonnet"              # Default model
export CLAUDE_MAX_TOKENS="4096"           # Response limit
```

### Hooks Configuration (Project-level)
Add to `.claude/settings.json`:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $FILE"
          }
        ]
      }
    ]
  }
}
```

### Custom Commands
Create `.claude/commands/` directory:
```
.claude/
├── commands/
│   ├── test.md      # Custom test command
│   └── deploy.md    # Custom deploy command
```

---

## MCP Servers

### Enabled Plugins (from settings.local.json)
| Plugin | Purpose | Status |
|--------|---------|--------|
| `playwright@claude-plugins-official` | Browser automation | ✅ Enabled |
| `superpowers@claude-plugins-official` | Development skills | ✅ Enabled |
| `context7@claude-plugins-official` | Documentation lookup | ✅ Enabled |

### Using MCP Tools
```bash
# In Claude Code session, use tools like:
# - playwright: browser automation
# - context7: lookup library docs
# - superpowers: workflow skills
```

---

## Verification Checklist

After setup, verify:
- [ ] `claude --version` works
- [ ] No permission prompts for common commands
- [ ] Plugins listed in `claude plugins list`
- [ ] Can read/write files in project
- [ ] Can run bash commands (git, python, etc.)

---

## Related Documentation
- [Project Settings](project-overview.md#configuration-files)
- [MCP Servers](mcp-servers.md)
- [Plugins and Skills](plugins-and-skills.md)
- [Troubleshooting](troubleshooting.md)
