# Plugins and Skills

Complete inventory of installed plugins and skills for Claude Code in the `fcc-claude1` repository.

## Table of Contents
- [Installed Plugins](#installed-plugins)
- [Installed Skills](#installed-skills)
- [Installation Commands](#installation-commands)
- [Configuration Details](#configuration-details)
- [Recovery Process](#recovery-process)

---

## Installed Plugins

### Official Plugins (from `.claude/settings.local.json`)

| Plugin | Version | Purpose | Status |
|--------|---------|---------|--------|
| `playwright@claude-plugins-official` | Latest | Browser automation, E2E testing | ✅ Enabled |
| `superpowers@claude-plugins-official` | Latest | Development workflow skills | ✅ Enabled |
| `context7@claude-plugins-official` | Latest | Library documentation lookup | ✅ Enabled |

### Plugin Details

#### Playwright Plugin
- **Repository**: Official Claude plugin marketplace
- **Capabilities**: Full Playwright API access
- **Tools**: browser_navigate, browser_click, browser_type, browser_snapshot, browser_take_screenshot, browser_evaluate, browser_network_requests, browser_fill_form, browser_select_option, browser_hover, browser_drag, browser_drop, browser_file_upload, browser_handle_dialog, browser_press_key, browser_wait_for, browser_tabs, browser_console_messages, browser_network_request, browser_run_code_unsafe
- **Use Cases**: Web scraping, E2E testing, screenshot capture, form automation

#### Superpowers Plugin
- **Repository**: Official Claude plugin marketplace
- **Capabilities**: Skill system for development workflows
- **Skills**: using-superpowers, brainstorming, dispatching-parallel-agents, executing-plans, finishing-a-development-branch, receiving-code-review, requesting-code-review, subagent-driven-development, systematic-debugging, test-driven-development, using-git-worktrees, verification-before-completion, writing-plans, writing-skills, update-config, keybindings-help, verify, code-review, simplify, fewer-permission-prompts, loop, claude-api, run, init, review, security-review
- **Use Cases**: Structured development workflows, planning, debugging, code review

#### Context7 Plugin
- **Repository**: Official Claude plugin marketplace
- **Capabilities**: Real-time library documentation
- **Tools**: resolve-library-id, query-docs
- **Use Cases**: API lookup, version migration, configuration examples

---

## Installed Skills

### Available Skills (from Superpowers Plugin)

#### Process Skills (Follow Exactly)
| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `test-driven-development` | TDD workflow | Before writing implementation |
| `systematic-debugging` | Debugging methodology | When encountering bugs |
| `writing-plans` | Implementation planning | Before multi-step tasks |
| `verification-before-completion` | Quality gates | Before claiming done |

#### Flexible Skills (Adapt Principles)
| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `brainstorming` | Creative exploration | Starting new features |
| `dispatching-parallel-agents` | Parallel task execution | 2+ independent tasks |
| `subagent-driven-development` | Implementation with agents | Executing plans |
| `using-git-worktrees` | Isolated workspaces | Feature work |

#### Utility Skills
| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `update-config` | Settings.json management | Config changes |
| `keybindings-help` | Keyboard customization | Key binding changes |
| `verify` | Runtime verification | Testing changes |
| `code-review` | Code quality review | PR review |
| `simplify` | Code simplification | Refactoring |
| `fewer-permission-prompts` | Reduce prompts | Permission optimization |
| `loop` | Recurring tasks | Scheduled work |

---

## Installation Commands

### Enable Plugins (Current Method)
Edit `.claude/settings.local.json`:
```json
{
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}
```

### Verify Installation
```bash
# List all plugins
claude plugins list

# List all skills
claude skills list
```

### Manual Plugin Install (If Needed)
```bash
# Plugins are typically auto-managed
# Manual install only if marketplace changes
claude plugins install playwright@claude-plugins-official
claude plugins install superpowers@claude-plugins-official
claude plugins install context7@claude-plugins-official
```

---

## Configuration Details

### Plugin Settings Location
```
.claude/settings.local.json          # Enabled plugins list
~/.claude/plugins/                   # Plugin binaries/cache
~/.claude/skills/                    # Skill definitions (superpowers)
```

### Per-Plugin Configuration (If Supported)

#### Playwright Config
```json
// ~/.claude/plugins/playwright/config.json
{
  "browser": "chromium",
  "headless": true,
  "timeout": 30000
}
```

#### Context7 Config
```json
// ~/.claude/plugins/context7/config.json
{
  "apiKey": "optional-for-authenticated-tier",
  "cacheDir": "~/.claude/plugins/context7/cache"
}
```

#### Superpowers Config
```json
// ~/.claude/plugins/superpowers/config.json
{
  "skillsDir": "~/.claude/skills",
  "autoLoad": true
}
```

---

## Recovery Process

### Complete Plugin Recovery

#### 1. Verify Settings
```bash
cat .claude/settings.local.json | jq .enabledPlugins
```

#### 2. Re-enable All Plugins
```json
{
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}
```

#### 3. Restart Claude Code
```bash
claude --clear-context
claude
```

#### 4. Verify Functionality
```bash
# Test each plugin
claude -p "Use playwright to navigate to example.com"
claude -p "Use brainstorming skill to plan a feature"
claude -p "Use context7 to find React 18 docs"
```

### Nuclear Recovery (Cache Clear)
```bash
# Remove all plugin data
rm -rf ~/.claude/plugins/
rm -rf ~/.claude/skills/

# Restart - plugins re-download
claude
```

### Skill Recovery
```bash
# Skills are part of superpowers plugin
# Re-enable superpowers plugin triggers skill reload

# Verify skills directory
ls ~/.claude/skills/

# List available skills
claude skills list
```

---

## Verification Checklist

After installation/recovery:
- [ ] `claude plugins list` shows 3 plugins enabled
- [ ] Playwright: Can execute browser commands
- [ ] Superpowers: Can invoke skills (e.g., `/brainstorming`)
- [ ] Context7: Can resolve library IDs
- [ ] All skills listed in `claude skills list`

---

## Adding New Plugins

### 1. Find Plugin
Browse: https://github.com/claude-plugins

### 2. Enable
Add to `.claude/settings.local.json`:
```json
{
  "enabledPlugins": {
    "existing...": true,
    "new-plugin@marketplace": true
  }
}
```

### 3. Document
Update this file's "Installed Plugins" table.

---

## Related Documentation
- [MCP Servers](mcp-servers.md)
- [Claude Code Setup](claude-code-setup.md)
- [Troubleshooting](troubleshooting.md)
