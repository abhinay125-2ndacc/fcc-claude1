# Changelog

History of changes to the `fcc-claude1` repository.

## Table of Contents
- [Unreleased](#unreleased)
- [v0.1.0](#v010---2026-06-29)
- [Documentation Updates](#documentation-updates)

---

## Unreleased

### Added
- Comprehensive documentation system (15 markdown files)
- Complete setup, recovery, architecture, workflow guides
- Claude Code, FCC, MCP, plugins documentation
- VS Code workspace configuration
- Dependency and environment variable references
- Deployment and troubleshooting guides

### Changed
- Fixed `.claude/settings.json` - removed empty deny array
- Fixed `.claude/settings.local.json` - corrected malformed permission rule

### Fixed
- Settings validation errors
- Malformed bash permission pattern

---

## v0.1.0 - 2026-06-29

### Added
- Initial repository creation via GitHub CLI
- Python project structure with `pyproject.toml`
- Sample scripts: `hello.py`, `printhello.py`
- Code quality tooling:
  - Black formatter (v24.4.2)
  - isort import sorter (v5.13.2)
  - flake8 linter (v7.1.1)
  - pre-commit hooks (v4.0.1)
  - pytest test framework (v8.2.1)
- Pre-commit configuration with:
  - Black formatting
  - isort import sorting
  - flake8 linting
  - trailing-whitespace removal
  - end-of-file-fixer
  - YAML/TOML validation
  - large file detection
- VS Code workspace configuration:
  - Workspace settings (`.vscode/settings.json`)
  - Recommended extensions (`.vscode/extensions.json`)
- Git ignore rules (`.gitignore`)
- README.md with basic usage

### Configuration
- `.claude/settings.json` - Project permissions for Claude Code
- `.claude/settings.local.json` - Local overrides + MCP plugins:
  - playwright@claude-plugins-official
  - superpowers@claude-plugins-official
  - context7@claude-plugins-official

---

## Documentation Updates

### 2026-06-29 - Initial Documentation Suite
Created comprehensive documentation system with 15 files:

| File | Description |
|------|-------------|
| `setup.md` | Complete project setup from scratch |
| `recovery-guide.md` | Disaster recovery procedures |
| `project-overview.md` | Project purpose, goals, status, roadmap |
| `architecture.md` | System architecture and folder structure |
| `development-workflow.md` | Daily workflow, git, commit, branch practices |
| `claude-code-setup.md` | Claude Code installation and configuration |
| `fcc-claude-setup.md` | FCC Claude integration guide |
| `mcp-servers.md` | MCP server inventory and usage |
| `plugins-and-skills.md` | Plugin and skill catalog |
| `dependencies.md` | Complete dependency list |
| `environment-variables.md` | Environment variable reference |
| `vscode-setup.md` | VS Code workspace configuration |
| `deployment.md` | Deployment procedures |
| `troubleshooting.md` | Common issues and solutions |
| `changelog.md` | This file |

---

## Release History

| Version | Date | Notes |
|---------|------|-------|
| v0.1.0 | 2026-06-29 | Initial release |
| v0.1.1 | 2026-06-29 | Documentation suite added |

---

## Contributing to Changelog

When making changes, add entries under "Unreleased" following the format:

### Added
- New features

### Changed
- Changes to existing functionality

### Deprecated
- Soon-to-be removed features

### Removed
- Removed features

### Fixed
- Bug fixes

### Security
- Security improvements

---

## Versioning

This project follows [Semantic Versioning](https://semver.org/):
- **MAJOR**: Incompatible API changes
- **MINOR**: Backward-compatible functionality
- **PATCH**: Backward-compatible bug fixes

---

## Links

- [GitHub Releases](https://github.com/abhinay125-2ndacc/fcc-claude1/releases)
- [GitHub Commits](https://github.com/abhinay125-2ndacc/fcc-claude1/commits/main)
- [Issues](https://github.com/abhinay125-2ndacc/fcc-claude1/issues)
