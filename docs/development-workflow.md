# Development Workflow

Daily development practices and workflows for the `fcc-claude1` repository.

## Table of Contents
- [Daily Workflow](#daily-workflow)
- [Git Workflow](#git-workflow)
- [Commit Workflow](#commit-workflow)
- [Branch Workflow](#branch-workflow)
- [Backup Workflow](#backup-workflow)
- [Code Review Workflow](#code-review-workflow)
- [Release Workflow](#release-workflow)
- [Best Practices](#best-practices)

---

## Daily Workflow

### Morning Setup (Codespace)
```bash
# 1. Open Codespace - auto-starts in /workspaces/fcc-claude1
# 2. Verify environment
cd /workspaces/fcc-claude1
git status

# 3. Pull latest changes
git pull origin main

# 4. Check for dependency updates (weekly)
pip list --outdated
```

### During Development
```bash
# 1. Create feature branch
git checkout -b feature/your-feature-name

# 2. Make changes
# Edit files in VS Code (auto-formats on save)

# 3. Run quality checks frequently
black .
isort .
flake8 .
pytest -v

# 4. Stage and commit
git add .
git commit -m "feat: descriptive message"
```

### End of Day
```bash
# 1. Push work-in-progress
git push origin feature/your-feature-name

# 2. Create backup bundle (optional)
git bundle create ../fcc-claude1-backup-$(date +%Y%m%d).bundle --all
```

---

## Git Workflow

### Repository Structure
```
main (protected)
  │
  ├── feature/*     # Feature branches
  ├── fix/*         # Bug fix branches
  ├── docs/*        # Documentation branches
  ├── refactor/*    # Refactoring branches
  └── chore/*       # Maintenance branches
```

### Branch Naming Convention
| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/<short-description>` | `feature/add-user-auth` |
| Bug Fix | `fix/<issue-description>` | `fix/login-timeout` |
| Documentation | `docs/<what-updated>` | `docs/update-setup-guide` |
| Refactor | `refactor/<what-refactored>` | `refactor/config-structure` |
| Chore | `chore/<maintenance-task>` | `chore/update-dependencies` |

### Branch Lifecycle
```bash
# 1. Start from main
git checkout main
git pull origin main

# 2. Create branch
git checkout -b feature/awesome-feature

# 3. Work on branch (commit often)
git add .
git commit -m "feat: add awesome feature"

# 4. Push branch
git push origin feature/awesome-feature

# 5. Create PR on GitHub
gh pr create --title "Add awesome feature" --body "Description..."

# 6. After review & merge
git checkout main
git pull origin main
git branch -d feature/awesome-feature
git push origin --delete feature/awesome-feature
```

---

## Commit Workflow

### Commit Message Format
Follows [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code restructuring |
| `perf` | Performance improvement |
| `test` | Adding tests |
| `chore` | Maintenance |
| `ci` | CI/CD changes |

### Examples
```bash
# Good commits
git commit -m "feat: add user authentication module"
git commit -m "fix: resolve login timeout on slow networks"
git commit -m "docs: update setup guide for Codespaces"
git commit -m "refactor: simplify config loading logic"
git commit -m "test: add unit tests for auth module"

# With scope
git commit -m "feat(auth): add OAuth2 provider support"
git commit -m "fix(api): handle 429 rate limit responses"

# With body
git commit -m "feat: add user authentication

- Implement JWT token generation
- Add refresh token rotation
- Include unit tests for auth flows

Closes #123"
```

### Commit Checklist
Before committing:
- [ ] Code formatted (`black . && isort .`)
- [ ] Linting passes (`flake8 .`)
- [ ] Tests pass (`pytest -v`)
- [ ] Commit message follows convention
- [ ] Only related changes in commit
- [ ] No secrets or credentials

---

## Branch Workflow

### Main Branch Protection
- **Protected**: Requires PR review
- **Required checks**: Pre-commit, tests (when CI configured)
- **No force push**: History is immutable
- **Linear history**: Squash merge preferred

### Feature Branch Workflow
```bash
# 1. Create from main
git checkout main && git pull
git checkout -b feature/new-feature

# 2. Develop with small commits
git add -p  # Stage interactively
git commit -m "feat: implement core logic"

# 3. Keep updated with main
git fetch origin
git rebase origin/main  # Or merge if preferred

# 4. Push for review
git push origin feature/new-feature

# 5. Create PR
gh pr create --fill
```

### Hotfix Workflow
```bash
# 1. Create from main
git checkout main && git pull
git checkout -b fix/critical-bug

# 2. Fix and test
git commit -m "fix: resolve critical production issue"

# 3. Fast-track PR
gh pr create --title "Hotfix: Critical Bug" --label "hotfix"

# 4. After merge, tag release
git tag -a v0.1.1 -m "Hotfix release"
git push origin v0.1.1
```

---

## Backup Workflow

### Automated Backup (Local Machine)
```bash
#!/bin/bash
# backup-repo.sh - Run daily via cron

REPO_DIR="$HOME/projects/fcc-claude1"
BACKUP_DIR="$HOME/backups/fcc-claude1"
DATE=$(date +%Y%m%d-%H%M%S)

mkdir -p "$BACKUP_DIR"
cd "$REPO_DIR"

# Create git bundle (complete history)
git bundle create "$BACKUP_DIR/fcc-claude1-$DATE.bundle" --all

# Verify bundle
git bundle verify "$BACKUP_DIR/fcc-claude1-$DATE.bundle"

# Cleanup old backups (keep 30 days)
find "$BACKUP_DIR" -name "*.bundle" -mtime +30 -delete

echo "Backup completed: $BACKUP_DIR/fcc-claude1-$DATE.bundle"
```

### Manual Backup Before Risky Operations
```bash
# Before rebase, reset, or force push
git bundle create ../fcc-claude1-pre-rebase-$(date +%Y%m%d).bundle --all

# Verify
git bundle verify ../fcc-claude1-pre-rebase-$(date +%Y%m%d).bundle
```

### Codespace Backup
- **Automatic**: GitHub persists repository in Codespace
- **Manual**: Push all branches before closing
```bash
git push origin --all
git push origin --tags
```

---

## Code Review Workflow

### As Author
1. **Self-review first**: `git diff main..HEAD`
2. **Write clear PR description**: What, Why, How
3. **Keep PRs small**: < 400 lines changed
4. **Respond to feedback promptly**
5. **Update branch**: `git rebase` or `git merge` main

### As Reviewer
1. **Check CI status** (when configured)
2. **Review for**: Logic, security, performance, style
3. **Use GitHub review tools**: Comments, suggestions
4. **Approve or request changes**
5. **Merge when approved**: Squash and merge

### Review Checklist
- [ ] Code solves the stated problem
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
- [ ] Follows project conventions
- [ ] No secrets exposed
- [ ] Performance acceptable

---

## Release Workflow

### Versioning
Follows [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

### Release Process
```bash
# 1. Ensure main is clean
git checkout main && git pull

# 2. Update version in pyproject.toml
# [project]
# version = "0.2.0"

# 3. Update changelog
# Edit docs/changelog.md

# 4. Commit version bump
git commit -am "chore: release v0.2.0"

# 5. Create tag
git tag -a v0.2.0 -m "Release v0.2.0"

# 6. Push
git push origin main --tags

# 7. Create GitHub Release
gh release create v0.2.0 --generate-notes
```

---

## Best Practices

### Code Quality
| Practice | Tool | Frequency |
|----------|------|-----------|
| Format code | Black | On save / pre-commit |
| Sort imports | isort | On save / pre-commit |
| Lint code | flake8 | On save / pre-commit |
| Type check | mypy | Pre-commit / CI |
| Run tests | pytest | Pre-push / CI |

### Git Hygiene
- **Commit early, commit often**
- **Write meaningful commit messages**
- **Keep branches short-lived**
- **Rebase instead of merge** (personal preference)
- **Delete merged branches**
- **Tag releases**

### Collaboration
- **Small, focused PRs**
- **Descriptive PR titles and bodies**
- **Link issues in PR description**
- **Request reviews from relevant people**
- **Address all feedback before merge**

### Documentation
- **Update docs with code changes**
- **Keep README current**
- **Document decisions in PR/commits**
- **Maintain changelog**

### Security
- **Never commit secrets**
- **Use .env for local secrets**
- **Rotate credentials regularly**
- **Review dependencies for vulnerabilities**

---

## Quick Reference

### Common Commands
```bash
# Start feature
git checkout main && git pull && git checkout -b feature/xyz

# Quality check
black . && isort . && flake8 . && pytest -v

# Commit
git add -p && git commit -m "feat: description"

# Push & PR
git push origin feature/xyz && gh pr create --fill

# Sync with main
git fetch origin && git rebase origin/main

# Cleanup
git branch -d feature/xyz && git push origin --delete feature/xyz
```

### Aliases (Add to ~/.gitconfig)
```ini
[alias]
    co = checkout
    cob = checkout -b
    st = status
    aa = add -A
    cm = commit -m
    ps = push
    pl = pull
    lg = log --oneline --graph --decorate
    br = branch
    bd = branch -d
    bD = branch -D
    rs = reset --hard
    rb = rebase
    mt = mergetool
```

---

**Related**: [Git Workflow](git-workflow.md) | [Commit Guidelines](commit-guidelines.md) | [Code Review](code-review.md)
