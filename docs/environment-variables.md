# Environment Variables

Complete reference for all environment variables used in the `fcc-claude1` repository.

## Table of Contents
- [Required Variables](#required-variables)
- [Optional Variables](#optional-variables)
- [Development Variables](#development-variables)
- [CI/CD Variables](#cicd-variables)
- [Security Recommendations](#security-recommendations)
- [Placeholder Examples](#placeholder-examples)

---

## Required Variables

### None Currently Required
This repository has **no required environment variables** for basic operation. All tools run with defaults.

---

## Optional Variables

### Python Environment
| Variable | Purpose | Default | Example |
|----------|---------|---------|---------|
| `PYTHONPATH` | Python module search path | Current directory | `/workspaces/fcc-claude1` |
| `VIRTUAL_ENV` | Virtual environment path | Auto-detected | `/workspaces/fcc-claude1/.venv` |
| `PYTHONDONTWRITEBYTECODE` | Disable .pyc files | `0` | `1` |

### Tool Configuration
| Variable | Purpose | Default | Example |
|----------|---------|---------|---------|
| `BLACK_LINE_LENGTH` | Black formatter line length | `88` (from pyproject.toml) | `100` |
| `FLAKE8_MAX_LINE_LENGTH` | Flake8 max line length | `88` (from pyproject.toml) | `120` |
| `ISORT_PROFILE` | isort profile | `black` (from pyproject.toml) | `django` |
| `PRE_COMMIT_CONFIG` | Pre-commit config path | `.pre-commit-config.yaml` | `/custom/path.yaml` |

### Git/GitHub
| Variable | Purpose | Default | Example |
|----------|---------|---------|---------|
| `GITHUB_TOKEN` | GitHub API token | From `gh auth` | `ghp_xxxxxxxxxxxx` |
| `GIT_AUTHOR_NAME` | Git commit author name | Git config | `Your Name` |
| `GIT_AUTHOR_EMAIL` | Git commit author email | Git config | `you@example.com` |
| `GIT_COMMITTER_NAME` | Git committer name | Same as author | `CI Bot` |
| `GIT_COMMITTER_EMAIL` | Git committer email | Same as author | `ci@example.com` |

### MCP Servers
| Variable | Purpose | Default | Example |
|----------|---------|---------|---------|
| `PLAYWRIGHT_BROWSERS_PATH` | Browser install location | `~/.cache/ms-playwright` | `/custom/browsers` |
| `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD` | Skip browser download | `0` | `1` |
| `CONTEXT7_API_KEY` | Context7 authenticated tier | None | `ctx7_xxxxxxxxxxxx` |

### VS Code / Editor
| Variable | Purpose | Default | Example |
|----------|---------|---------|---------|
| `EDITOR` | Default editor | `code --wait` | `vim` |
| `VISUAL` | Visual editor | Same as EDITOR | `code --wait` |

---

## Development Variables

### Local Development (`.env` file - gitignored)
Create `.env` in project root:
```bash
# .env (DO NOT COMMIT)
# Python
PYTHONPATH=/workspaces/fcc-claude1
PYTHONDONTWRITEBYTECODE=1

# Tools (override pyproject.toml if needed)
# BLACK_LINE_LENGTH=100
# FLAKE8_MAX_LINE_LENGTH=120

# GitHub (if not using gh auth)
# GITHUB_TOKEN=ghp_your_token_here

# MCP Servers
# PLAYWRIGHT_BROWSERS_PATH=/custom/path
# CONTEXT7_API_KEY=ctx7_your_key_here
```

### Codespaces Secrets
Configure in GitHub Codespaces settings:
| Secret Name | Value | Scope |
|-------------|-------|-------|
| `GITHUB_TOKEN` | Auto-provided | Repository |
| `CUSTOM_SECRET` | Your value | Repository/User/Org |

---

## CI/CD Variables

### GitHub Actions (Future)
When CI/CD is configured, these will be used:
| Variable | Purpose | Source |
|----------|---------|--------|
| `GITHUB_TOKEN` | Auto-provided | GitHub Actions |
| `PYTHON_VERSION` | Matrix variable | Workflow |
| `NODE_VERSION` | Matrix variable | Workflow |

---

## Security Recommendations

### DO
- ✅ Use GitHub Codespaces Secrets for cloud secrets
- ✅ Use `.env` file locally (add to `.gitignore`)
- ✅ Use placeholder values in documentation
- ✅ Rotate tokens regularly
- ✅ Use least-privilege tokens (fine-grained PATs)
- ✅ Store secrets in password manager (1Password, Bitwarden)

### DON'T
- ❌ Commit `.env` files
- ❌ Hardcode secrets in source code
- ❌ Put real tokens in documentation
- ❌ Share tokens in chat/issues/PRs
- ❌ Use personal tokens for automation

### Secret Scanning
GitHub automatically scans for:
- GitHub tokens (`ghp_`, `github_pat_`)
- AWS keys (`AKIA`)
- GCP keys
- Generic API keys
- Private keys

---

## Placeholder Examples

### Documentation Examples (Safe to Commit)
```bash
# ✅ GOOD - Placeholders only
export API_KEY="YOUR_API_KEY_HERE"
export DATABASE_URL="postgresql://user:pass@host:5432/db"
export GITHUB_TOKEN="ghp_YOUR_TOKEN_HERE"
export AWS_ACCESS_KEY_ID="AKIA_YOUR_KEY_HERE"

# ❌ BAD - Real values (NEVER COMMIT)
export API_KEY="sk_live_abc123..."
export DATABASE_URL="postgresql://real:password@prod.db.com:5432/mydb"
export GITHUB_TOKEN="ghp_abcdef123456..."
```

### Template Files (Safe to Commit)
```bash
# .env.example (COMMIT THIS)
# Copy to .env and fill in your values
API_KEY=YOUR_API_KEY_HERE
DATABASE_URL=postgresql://user:pass@localhost:5432/db
GITHUB_TOKEN=ghp_YOUR_TOKEN_HERE
```

---

## Variable Priority

When multiple sources define the same variable:
```
1. Shell environment (highest)
2. .env file (via python-dotenv)
3. pyproject.toml / tool config
4. Tool defaults (lowest)
```

---

## Verification

### Check Current Environment
```bash
# Show all env vars (filter relevant)
env | grep -E '(PYTHON|GIT|GITHUB|PLAYWRIGHT|CONTEXT7|BLACK|FLAKE8|ISORT)'

# Check specific
echo $GITHUB_TOKEN
echo $PYTHONPATH
```

### Validate No Secrets Committed
```bash
# Check for potential secrets in repo
git log --all --full-history --oneline -- '*secret*' '*token*' '*key*' '*password*'

# Use git-secrets or truffleHog for deeper scan
```

---

## Related Documentation
- [Setup Guide](setup.md)
- [MCP Servers](mcp-servers.md)
- [Troubleshooting](troubleshooting.md)
- [Security Review](security-review.md)
