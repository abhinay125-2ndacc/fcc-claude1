# MCP Servers

Documentation for all configured MCP (Model Context Protocol) servers in the `fcc-claude1` repository.

## Table of Contents
- [Overview](#overview)
- [Installed MCP Servers](#installed-mcp-servers)
- [Installation Commands](#installation-commands)
- [Configuration Locations](#configuration-locations)
- [Usage Examples](#usage-examples)
- [Recovery Instructions](#recovery-instructions)

---

## Overview

MCP (Model Context Protocol) servers extend Claude Code's capabilities by providing access to external tools, APIs, and data sources. This repository configures three official MCP servers via plugins.

---

## Installed MCP Servers

| Server | Plugin | Purpose | Status |
|--------|--------|---------|--------|
| **Playwright** | `playwright@claude-plugins-official` | Browser automation, testing, scraping | ✅ Enabled |
| **Superpowers** | `superpowers@claude-plugins-official` | Development workflow skills | ✅ Enabled |
| **Context7** | `context7@claude-plugins-official` | Library documentation lookup | ✅ Enabled |

---

## Installation Commands

### Via Settings (Current Method)
MCP servers are enabled via `.claude/settings.local.json`:

```json
{
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}
```

### Manual Installation (Alternative)
```bash
# Install plugins globally (if needed)
# These are typically auto-installed by Claude Code

# Verify plugins
claude plugins list
```

---

## Configuration Locations

### Primary Configuration
```
.claude/settings.local.json
```
Contains the `enabledPlugins` object (see above).

### Plugin-Specific Config (If Needed)
```
~/.claude/plugins/playwright/
~/.claude/plugins/superpowers/
~/.claude/plugins/context7/
```

### Environment Variables (Optional)
```bash
# Playwright
export PLAYWRIGHT_BROWSERS_PATH="/path/to/browsers"

# Context7
export CONTEXT7_API_KEY="your-key"  # If using authenticated tier
```

---

## Usage Examples

### Playwright (Browser Automation)
```bash
# In Claude Code session:
> Navigate to https://example.com and take a screenshot
> Fill out the login form on the page
> Run end-to-end tests for the checkout flow
```

**Available Tools**:
- `browser_navigate` - Navigate to URL
- `browser_click` - Click elements
- `browser_type` - Type text
- `browser_snapshot` - Accessibility snapshot
- `browser_take_screenshot` - Capture screenshot
- `browser_evaluate` - Run JavaScript
- `browser_network_requests` - Monitor network

### Superpowers (Development Skills)
```bash
# In Claude Code session:
> Create a comprehensive setup guide for this project
> Generate a git commit message following conventions
> Debug this failing test systematically
> Create a todo list for this feature implementation
```

**Available Skills**:
- `using-superpowers` - Skill system overview
- `brainstorming` - Creative exploration
- `systematic-debugging` - Debugging methodology
- `test-driven-development` - TDD workflow
- `writing-plans` - Implementation planning
- `verification-before-completion` - Quality gates
- And more...

### Context7 (Documentation Lookup)
```bash
# In Claude Code session:
> Look up the latest React 18 hooks API
> Find examples for Prisma schema migrations
> Get Tailwind CSS v4 migration guide
```

**Available Tools**:
- `resolve-library-id` - Find library identifier
- `query-docs` - Search documentation

---

## Recovery Instructions

### If Plugins Stop Working

#### 1. Verify Plugin Status
```bash
claude plugins list
```

#### 2. Re-enable in Settings
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

#### 3. Restart Claude Code
```bash
# Exit current session
# Run claude again
claude
```

#### 4. Clear Plugin Cache (Nuclear)
```bash
rm -rf ~/.claude/plugins/
# Restart claude - plugins will re-download
```

### If Specific Server Fails

#### Playwright Issues
```bash
# Install browsers if missing
npx playwright install

# Verify
npx playwright --version
```

#### Context7 Issues
```bash
# Check API access
# Verify network connectivity to context7 API
```

#### Superpowers Issues
```bash
# Skills are built-in - no external deps
# Check .claude/skills/ directory exists
ls ~/.claude/skills/
```

---

## Verification Checklist

After setup/recovery:
- [ ] `claude plugins list` shows all three plugins
- [ ] Playwright: Can navigate to a URL
- [ ] Superpowers: Can invoke a skill (e.g., `/brainstorming`)
- [ ] Context7: Can resolve a library ID

---

## Adding New MCP Servers

### 1. Find Official Plugin
Check https://github.com/claude-plugins for available plugins.

### 2. Enable in Settings
```json
{
  "enabledPlugins": {
    "existing-plugins...": true,
    "new-plugin@marketplace": true
  }
}
```

### 3. Document
Add to this file under "Installed MCP Servers" table.

---

## Related Documentation
- [Claude Code Setup](claude-code-setup.md)
- [Plugins and Skills](plugins-and-skills.md)
- [Troubleshooting](troubleshooting.md)
