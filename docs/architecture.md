# System Architecture

Technical architecture documentation for the `fcc-claude1` repository.

## Table of Contents
- [Repository Architecture](#repository-architecture)
- [Folder Structure](#folder-structure-explanation)
- [Configuration Files](#configuration-files-explanation)
- [Development Environment Architecture](#development-environment-architecture)
- [Data Flow](#data-flow)
- [Security Architecture](#security-architecture)
- [Extensibility Points](#extensibility-points)

---

## Repository Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      fcc-claude1 Repository                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Source    │  │   Config    │  │    Docs     │             │
│  │   Code      │  │   Files     │  │             │             │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤             │
│  │ hello.py    │  │ pyproject.  │  │ setup.md    │             │
│  │ printhello. │  │   toml      │  │ recovery-   │             │
│  │             │  │ .pre-commit │  │   guide.md  │             │
│  │             │  │   config.   │  │ project-    │             │
│  │             │  │   yaml      │  │   overview. │             │
│  │             │  │ .gitignore  │  │   md        │             │
│  │             │  │ .claude/    │  │ ... (15)    │             │
│  │             │  │ .vscode/    │  │             │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│         │                │                │                      │
│         └────────────────┼────────────────┘                      │
│                          ▼                                       │
│              ┌─────────────────────┐                             │
│              │   Git Repository    │                             │
│              │   (Single Source    │                             │
│              │    of Truth)        │                             │
│              └─────────────────────┘                             │
│                          │                                       │
│          ┌───────────────┼───────────────┐                       │
│          ▼               ▼               ▼                       │
│   ┌────────────┐ ┌────────────┐ ┌────────────┐                  │
│   │ GitHub     │ │ Local      │ │ Codespace  │                  │
│   │ (Remote)   │ │ Clone      │ │ (Cloud)    │                  │
│   └────────────┘ └────────────┘ └────────────┘                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Architectural Principles

1. **Single Source of Truth** - Git repository contains everything needed
2. **Configuration as Code** - All settings in version-controlled files
3. **Environment Agnostic** - Works identically in Codespace, local, CI
4. **Reproducible Builds** - Deterministic dependency resolution
5. **Zero Local State** - No uncommitted configuration required

---

## Folder Structure Explanation

### Root Level
```
fcc-claude1/
├── hello.py                 # Entry point: Simple greeting script
├── printhello.py            # Alternative entry point
├── pyproject.toml           # Project metadata & tool configuration
├── .pre-commit-config.yaml  # Git hook definitions
├── .gitignore               # Version control exclusions
├── README.md                # GitHub landing page
└── docs/                    # Documentation (15 markdown files)
```

### Configuration Directories

#### `.claude/` - Claude Code Configuration
```
.claude/
├── settings.json            # Project-level permissions (tracked)
└── settings.local.json      # Local overrides (gitignored)
```

**Purpose**: Controls Claude Code behavior, permissions, and plugin configuration for this project.

#### `.vscode/` - VS Code Workspace
```
.vscode/
├── settings.json            # Editor behavior, formatting, linting
└── extensions.json          # Recommended extensions list
```

**Purpose**: Ensures consistent editor experience across all developers.

#### `docs/` - Documentation
```
docs/
├── setup.md                 # Initial setup guide
├── recovery-guide.md        # Disaster recovery
├── project-overview.md      # This file
├── architecture.md          # System architecture
├── development-workflow.md  # Daily practices
├── claude-code-setup.md     # AI assistant config
├── fcc-claude-setup.md      # FCC-specific config
├── mcp-servers.md           # MCP server details
├── plugins-and-skills.md    # Plugin/skill inventory
├── dependencies.md          # Dependency catalog
├── environment-variables.md # Env var reference
├── vscode-setup.md          # VS Code deep dive
├── deployment.md            # Deployment procedures
├── troubleshooting.md       # Issue resolution
└── changelog.md             # History log
```

#### `.github/` - GitHub Configuration (Future)
```
.github/
├── workflows/               # GitHub Actions CI/CD
├── dependabot.yml           # Dependency updates
├── ISSUE_TEMPLATE/          # Issue templates
└── PULL_REQUEST_TEMPLATE/   # PR templates
```

#### `tests/` - Test Suite (Future)
```
tests/
├── unit/                    # Unit tests
├── integration/             # Integration tests
├── fixtures/                # Test data
└── conftest.py              # Pytest configuration
```

### Generated/Ignored Directories

| Directory | Purpose | Git Status |
|-----------|---------|------------|
| `.venv/` | Python virtual environment | Ignored |
| `__pycache__/` | Python bytecode cache | Ignored |
| `.pytest_cache/` | Pytest cache | Ignored |
| `.mypy_cache/` | MyPy cache | Ignored |
| `*.egg-info/` | Package metadata | Ignored |
| `dist/` | Build artifacts | Ignored |
| `build/` | Build artifacts | Ignored |

---

## Configuration Files Explanation

### `pyproject.toml` - Project Manifest
**Location**: Root
**Purpose**: Single source of truth for project metadata and tool configuration

```toml
[build-system]           # Build backend
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]                # Package metadata
name = "9router-claude-test1"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = []        # Runtime dependencies (none)

[project.optional-dependencies]
dev = [                  # Development dependencies
  "black==24.4.2",
  "isort==5.13.2",
  "flake8==7.1.1",
  "pre-commit==4.0.1",
  "pytest==8.2.1",
  "pytest-cov==5.0.0",
]

[tool.black]             # Black formatter config
line-length = 88
target-version = ['py310']

[tool.isort]             # Import sorter config
profile = "black"
line_length = 88

[tool.flake8]            # Linter config
max-line-length = 88
extend-ignore = "E203"

[tool.pytest.ini_options] # Test config
testpaths = ["tests"]
```

### `.pre-commit-config.yaml` - Git Hooks
**Location**: Root
**Purpose**: Automated code quality checks on commit

```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 24.4.2
    hooks:
      - id: black
        args: ["--line-length=88"]

  - repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
      - id: isort
        args: ["--profile=black", "--line-length=88"]

  - repo: https://github.com/pycqa/flake8
    rev: 7.1.1
    hooks:
      - id: flake8
        args: ["--max-line-length=88", "--extend-ignore=E203"]

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-toml
      - id: check-added-large-files
```

### `.gitignore` - Version Control Exclusions
**Location**: Root
**Purpose**: Prevents committing generated/local files

Key patterns:
- `__pycache__/`, `*.py[cod]` - Python cache
- `.venv/`, `venv/` - Virtual environments
- `.pytest_cache/`, `.mypy_cache/` - Tool caches
- `*.egg-info/`, `dist/`, `build/` - Build artifacts
- `.env`, `.env.local` - Secrets
- `.vscode/*` (except settings.json, extensions.json) - Local VS Code state

### `.claude/settings.json` - Project Permissions
**Location**: `.claude/`
**Purpose**: Grants Claude Code permissions for this project

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)", "Bash(gh *)", "Bash(npm *)",
      "Bash(python *)", "Bash(pip *)", "Bash(pytest *)",
      "Bash(black *)", "Bash(isort *)", "Bash(flake8 *)",
      "Bash(pre-commit *)", "Bash(cat *)", "Bash(ls *)",
      "Bash(find *)", "Bash(grep *)", "Bash(mkdir *)",
      "Bash(rm *)", "Bash(cp *)", "Bash(mv *)",
      "Bash(chmod *)", "Read(*)", "Glob(*)", "Grep(*)",
      "Task(*)", "Edit(*)", "Write(*)", "NotebookEdit(*)"
    ]
  }
}
```

### `.claude/settings.local.json` - Local Overrides
**Location**: `.claude/`
**Purpose**: User-specific settings (not shared)

```json
{
  "permissions": {
    "allow": [
      "Bash(python *)", "Bash(git add *)", "Bash(git commit *)",
      "Bash(git push *)", "Bash(gh repo *)", "Bash(gh auth *)",
      "Bash(git remote *)", "Bash(git init *)", "Bash(gh api *)",
      "Bash(unset GITHUB_TOKEN)", "Bash(curl *)", "Bash(~)",
      "Bash(git config *)", "Bash(gh config *)"
    ]
  },
  "enabledPlugins": {
    "playwright@claude-plugins-official": true,
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true
  }
}
```

### `.vscode/settings.json` - Editor Configuration
**Location**: `.vscode/`
**Purpose**: Workspace-specific editor behavior

Key settings:
- Format on save/paste/type with Black
- Organize imports on save
- Rulers at 88 and 120 characters
- Python linting with flake8
- Test discovery with pytest
- YAML/TOML formatting

### `.vscode/extensions.json` - Extension Recommendations
**Location**: `.vscode/`
**Purpose**: Prompts users to install recommended extensions

---

## Development Environment Architecture

### Environment Layers

```
┌────────────────────────────────────────────────────────────────┐
│                     DEVELOPMENT ENVIRONMENT                     │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    HOST MACHINE                            │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │  │
│  │  │   Git    │  │  Python  │  │   gh     │  │  Node.js │  │  │
│  │  │  (CLI)   │  │  3.10+   │  │  (CLI)   │  │  18+     │  │  │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │  │
│  └──────────────────────────────────────────────────────────┘  │
│                           │                                     │
│         ┌─────────────────┼─────────────────┐                   │
│         ▼                 ▼                 ▼                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │  LOCAL      │  │  CODESPACE  │  │    CI/CD    │              │
│  │  CLONE      │  │  (CLOUD)    │  │  (FUTURE)   │              │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤              │
│  │ .venv/      │  │ .venv/      │  │ GitHub      │              │
│  │ (isolated)  │  │ (auto)      │  │ Actions     │              │
│  │             │  │             │  │             │              │
│  │ VS Code     │  │ VS Code     │  │ pytest      │              │
│  │ (local)     │  │ (browser)   │  │ black       │              │
│  │             │  │             │  │ isort       │              │
│  │ Git hooks   │  │ Git hooks   │  │ flake8      │              │
│  │ (manual)    │  │ (auto)      │  │             │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### Codespace Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                  GITHUB CODESPACE                              │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              DOCKER CONTAINER (Ubuntu-based)             │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │  │
│  │  │  Runtime    │  │  Tools      │  │  Config     │       │  │
│  │  │  Python 3.10│  │  git, gh,   │  │  .devcontainer/    │  │
│  │  │  Node 18    │  │  docker     │  │  (optional)       │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘       │  │
│  │         │                │                │                │  │
│  │         └────────────────┼────────────────┘                │  │
│  │                          ▼                                 │  │
│  │  ┌─────────────────────────────────────────────────────┐   │  │
│  │  │              WORKSPACE (/workspaces/fcc-claude1)    │   │  │
│  │  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │   │  │
│  │  │  │  .venv  │ │  .git   │ │  src    │ │  docs   │   │   │  │
│  │  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘   │   │  │
│  │  └─────────────────────────────────────────────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### Tool Chain

| Layer | Tools | Purpose |
|-------|-------|---------|
| **Runtime** | Python 3.10+, Node 18+ | Execution environment |
| **Version Control** | Git, GitHub CLI (gh) | Source control |
| **Package Management** | pip, pyproject.toml | Dependency management |
| **Code Quality** | Black, isort, flake8 | Formatting & linting |
| **Git Hooks** | pre-commit | Automated checks |
| **Testing** | pytest, pytest-cov | Test execution |
| **Editor** | VS Code + extensions | Development UI |
| **AI Assistant** | Claude Code + plugins | AI-powered development |
| **MCP Servers** | Playwright, Context7, Superpowers | Extended capabilities |

---

## Data Flow

### Code Change Flow
```
Developer Edit
      │
      ▼
┌─────────────┐
│ VS Code     │ ──► Format on Save (Black, isort)
│ Editor      │
└─────────────┘
      │
      ▼
┌─────────────┐
│ Git Add     │
└─────────────┘
      │
      ▼
┌─────────────┐
│ Pre-commit  │ ──► Black, isort, flake8, trailing-whitespace,
│ Hooks       │     end-of-file-fixer, check-yaml, check-toml
└─────────────┘
      │
      ▼
┌─────────────┐
│ Git Commit  │
└─────────────┘
      │
      ▼
┌─────────────┐
│ Git Push    │
└─────────────┘
      │
      ▼
┌─────────────┐
│ GitHub      │ ──► (Future) CI/CD Pipeline
│ Remote      │
└─────────────┘
```

### Dependency Resolution Flow
```
pyproject.toml
      │
      ▼
┌─────────────┐
│ pip install │
│ -e .[dev]   │
└─────────────┘
      │
      ▼
┌─────────────┐
│ Resolve     │ ──► PyPI → Download wheels
│ Dependencies│
└─────────────┘
      │
      ▼
┌─────────────┐
│ Install to  │
│ .venv/      │
└─────────────┘
      │
      ▼
┌─────────────┐
│ Lock file   │ (not committed - reproducible via versions)
└─────────────┘
```

---

## Security Architecture

### Secrets Management

```
┌────────────────────────────────────────────────────────────────┐
│                    SECRETS HANDLING                             │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ❌ NEVER IN REPOSITORY:                                       │
│     - API keys, tokens, passwords                              │
│     - Private keys, certificates                               │
│     - Database connection strings                              │
│     - Personal access tokens                                   │
│                                                                 │
│  ✅ ACCEPTABLE IN REPOSITORY:                                  │
│     - Placeholder examples (YOUR_API_KEY_HERE)                 │
│     - Configuration templates (.env.example)                   │
│     - Public keys                                              │
│     - Documentation with fake values                           │
│                                                                 │
│  🔐 SECRETS STORED IN:                                         │
│     - GitHub Codespaces Secrets (for cloud)                    │
│     - Local .env files (gitignored)                            │
│     - Password managers (1Password, Bitwarden)                 │
│     - CI/CD secret stores (GitHub Actions secrets)             │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### Permission Model

| Component | Permissions | Scope |
|-----------|-------------|-------|
| `.claude/settings.json` | Project-level allow list | All project users |
| `.claude/settings.local.json` | User-specific allow list | Current user only |
| GitHub Codespaces Secrets | Environment variables | Codespace only |
| GitHub Actions Secrets | Environment variables | CI/CD only |

---

## Extensibility Points

### Adding New Tools
1. Add to `pyproject.toml` `[project.optional-dependencies.dev]`
2. Add pre-commit hook to `.pre-commit-config.yaml`
3. Add VS Code settings to `.vscode/settings.json`
4. Document in `dependencies.md`

### Adding MCP Servers
1. Enable in `.claude/settings.local.json`
2. Document in `mcp-servers.md`
3. Add usage examples

### Adding Skills/Plugins
1. Enable in `.claude/settings.local.json`
2. Document in `plugins-and-skills.md`
3. Add configuration details

### Adding Documentation
1. Create new `.md` file in `docs/`
2. Link from `project-overview.md` and relevant files
3. Update `changelog.md`

---

## Related Documentation

- [Project Overview](project-overview.md) - High-level summary
- [Setup Guide](setup.md) - Initial configuration
- [Development Workflow](development-workflow.md) - Daily practices
- [VS Code Setup](vscode-setup.md) - Editor details
- [Dependencies](dependencies.md) - Complete dependency list
