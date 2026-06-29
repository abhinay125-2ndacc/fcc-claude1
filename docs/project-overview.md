# Project Overview

Comprehensive overview of the `fcc-claude1` repository.

## Table of Contents
- [Project Purpose](#project-purpose)
- [Goals](#goals)
- [Current Status](#current-status)
- [Roadmap](#roadmap)
- [Directory Structure](#directory-structure-explanation)
- [Key Files](#key-files)
- [Technology Stack](#technology-stack)

---

## Project Purpose

**fcc-claude1** is a test repository created with GitHub CLI to explore and demonstrate:
- GitHub Codespaces development workflow
- Claude Code integration for AI-assisted development
- VS Code workspace configuration
- Python project structure with modern tooling
- Pre-commit hooks for code quality
- Documentation-driven development

This serves as a **template/reference repository** for setting up new projects with:
- Consistent development environment
- Automated code formatting and linting
- AI-assisted development with Claude Code
- Comprehensive documentation system

---

## Goals

### Primary Goals
1. **Reproducible Environment** - Any developer can spin up identical environment in minutes
2. **Code Quality** - Automated formatting, linting, and testing
3. **AI Integration** - Seamless Claude Code workflow
4. **Documentation** - Self-documenting repository with comprehensive guides
5. **Disaster Recovery** - Full rebuild capability from Git alone

### Secondary Goals
- Demonstrate GitHub Codespaces best practices
- Showcase VS Code workspace configuration
- Provide MCP server integration examples
- Establish commit/PR workflow standards

---

## Current Status

| Component | Status | Details |
|-----------|--------|---------|
| Repository | ✅ Active | `main` branch, up to date |
| Python Setup | ✅ Complete | 3.10+, pyproject.toml configured |
| Code Quality | ✅ Complete | Black, isort, flake8, pre-commit |
| Testing | ✅ Configured | pytest with coverage |
| VS Code | ✅ Configured | Settings + extensions |
| Documentation | 🚧 In Progress | 15 files planned |
| Claude Code | ✅ Configured | Settings + plugins |
| MCP Servers | ✅ Configured | Playwright, Context7, Superpowers |
| CI/CD | ❌ Not Configured | No GitHub Actions yet |

### Recent Changes
- Added VS Code workspace configuration
- Added comprehensive documentation structure
- Fixed settings.json permissions

---

## Roadmap

### Phase 1: Foundation (Current)
- [x] Repository initialization
- [x] Python project structure
- [x] Code quality tooling
- [x] VS Code configuration
- [x] Basic documentation (this file)

### Phase 2: Documentation (In Progress)
- [ ] Complete all 15 documentation files
- [ ] Add architecture diagrams
- [ ] Create setup verification script

### Phase 3: Enhancement (Planned)
- [ ] GitHub Actions CI/CD pipeline
- [ ] Automated dependency updates (Dependabot)
- [ ] Release automation
- [ ] Test coverage improvements

### Phase 4: Template Features (Future)
- [ ] Cookiecutter template generation
- [ ] Multi-environment configuration
- [ ] Docker support
- [ ] Kubernetes manifests

---

## Directory Structure Explanation

```
fcc-claude1/
├── .github/                    # GitHub-specific configuration (future)
│   └── workflows/              # GitHub Actions (planned)
├── .claude/                    # Claude Code configuration
│   ├── settings.json           # Project-level permissions
│   └── settings.local.json     # Local overrides (gitignored)
├── .vscode/                    # VS Code workspace configuration
│   ├── settings.json           # Workspace settings
│   └── extensions.json         # Recommended extensions
├── docs/                       # Documentation (this folder)
│   ├── setup.md                # Setup guide
│   ├── recovery-guide.md       # Disaster recovery
│   ├── project-overview.md     # This file
│   ├── architecture.md         # System architecture
│   ├── development-workflow.md # Daily workflow
│   ├── claude-code-setup.md    # Claude Code config
│   ├── fcc-claude-setup.md     # FCC Claude specific
│   ├── mcp-servers.md          # MCP servers
│   ├── plugins-and-skills.md   # Plugins & skills
│   ├── dependencies.md         # All dependencies
│   ├── environment-variables.md # Env vars reference
│   ├── vscode-setup.md         # VS Code details
│   ├── deployment.md           # Deployment guide
│   ├── troubleshooting.md      # Common issues
│   └── changelog.md            # History
├── tests/                      # Test files (future)
├── .gitignore                  # Git ignore rules
├── .pre-commit-config.yaml     # Pre-commit hooks
├── pyproject.toml              # Python project config
├── hello.py                    # Sample script
├── printhello.py               # Sample script
└── README.md                   # Project README
```

### Directory Details

| Directory | Purpose | Tracked in Git |
|-----------|---------|----------------|
| `.claude/` | Claude Code settings, hooks, skills | Yes (settings.json) |
| `.vscode/` | VS Code workspace config | Yes |
| `docs/` | All documentation | Yes |
| `tests/` | Unit/integration tests | Yes (when created) |
| `.pytest_cache/` | Pytest cache | No (gitignored) |
| `__pycache__/` | Python bytecode | No (gitignored) |
| `.venv/` | Virtual environment | No (gitignored) |
| `*.egg-info/` | Package metadata | No (gitignored) |

---

## Key Files

### Configuration Files

| File | Purpose | Format |
|------|---------|--------|
| `pyproject.toml` | Python project metadata, dependencies, tool config | TOML |
| `.pre-commit-config.yaml` | Pre-commit hook definitions | YAML |
| `.gitignore` | Git ignore patterns | Plain text |
| `.claude/settings.json` | Project permissions for Claude Code | JSON |
| `.claude/settings.local.json` | Local Claude Code overrides | JSON |
| `.vscode/settings.json` | VS Code workspace settings | JSON |
| `.vscode/extensions.json` | Recommended VS Code extensions | JSON |

### Source Files

| File | Purpose |
|------|---------|
| `hello.py` | Simple "Hello, World!" script |
| `printhello.py` | Alternative hello script with project name |

### Documentation Files

| File | Purpose |
|------|---------|
| `README.md` | Project overview for GitHub |
| `docs/*.md` | Comprehensive documentation (15 files) |

---

## Technology Stack

### Core
- **Language**: Python 3.10+
- **Package Manager**: pip with pyproject.toml
- **Build System**: setuptools

### Code Quality
- **Formatter**: Black (line-length: 88)
- **Import Sorter**: isort (Black profile)
- **Linter**: flake8 (max-line-length: 88)
- **Pre-commit**: pre-commit framework
- **Type Checking**: (planned - mypy)

### Testing
- **Framework**: pytest
- **Coverage**: pytest-cov
- **Test Discovery**: tests/ directory, test_*.py pattern

### Development Environment
- **Editor**: VS Code
- **Container**: GitHub Codespaces
- **AI Assistant**: Claude Code
- **MCP Servers**: Playwright, Context7, Superpowers
- **Git**: GitHub CLI (gh)

### Plugins & Skills (Claude Code)
- `playwright@claude-plugins-official` - Browser automation
- `superpowers@claude-plugins-official` - Development workflow skills
- `context7@claude-plugins-official` - Documentation lookup

---

## Repository Metadata

| Property | Value |
|----------|-------|
| Name | 9router-claude-test1 |
| Version | 0.1.0 |
| License | MIT |
| Author | abhinay125-2ndacc |
| Python Requirement | >=3.10 |
| Default Branch | main |
| Remote Origin | https://github.com/abhinay125-2ndacc/fcc-claude1.git |

---

## Related Documentation

- [Setup Guide](setup.md) - Getting started
- [Architecture](architecture.md) - Technical architecture
- [Development Workflow](development-workflow.md) - Daily practices
- [Claude Code Setup](claude-code-setup.md) - AI configuration
- [Recovery Guide](recovery-guide.md) - Disaster recovery
