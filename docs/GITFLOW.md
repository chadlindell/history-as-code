# GitFlow Branching Strategy

This document describes the GitFlow branching strategy used in the History as Code project.

## Overview

GitFlow is a branching model that provides a robust framework for managing larger projects. It's particularly well-suited for projects with scheduled releases and multiple developers.

## Branch Structure

```
main
│
├── dev
│   │
│   ├── feature/user-authentication
│   ├── feature/journal-submission
│   ├── feature/directory-search
│   └── feature/lab-integration
│
└── hotfix/critical-bug-fix (created from main for emergencies)
```

## Branch Purposes

### Main Branch (`main`)
- **Purpose**: Production-ready code
- **Deployment**: Automatically deploys to production
- **Access**: Protected, requires PR review
- **Merges from**: `dev` (for releases) or `hotfix/*` (for critical fixes)

### Development Branch (`dev`)
- **Purpose**: Integration branch for features
- **Deployment**: Automatically deploys to staging
- **Access**: Protected, requires PR review
- **Merges from**: `feature/*` branches
- **Default branch**: Yes

### Feature Branches (`feature/*`)
- **Purpose**: Development of new features
- **Naming**: `feature/descriptive-kebab-case-name`
- **Created from**: `dev`
- **Merges to**: `dev`
- **Lifetime**: Deleted after merge

### Hotfix Branches (`hotfix/*`)
- **Purpose**: Emergency fixes for production
- **Naming**: `hotfix/issue-description`
- **Created from**: `main`
- **Merges to**: Both `main` and `dev`
- **Lifetime**: Deleted after merge

## Workflow Examples

### Standard Feature Development

```bash
# 1. Start from updated dev branch
git checkout dev
git pull origin dev

# 2. Create feature branch
git checkout -b feature/add-lab-projects

# 3. Work on feature
# ... make changes ...
git add .
git commit -m "feat: add project creation in Lab module"

# 4. Keep feature branch updated
git fetch origin
git rebase origin/dev

# 5. Push feature branch
git push origin feature/add-lab-projects

# 6. Create PR to dev branch
# ... PR review and merge ...

# 7. Clean up
git checkout dev
git pull origin dev
git branch -d feature/add-lab-projects
```

### Release to Production

```bash
# 1. Ensure dev is stable
# ... run tests, check staging ...

# 2. Create PR from dev to main
# ... through GitHub UI ...

# 3. After merge, tag the release
git checkout main
git pull origin main
git tag -a v1.2.0 -m "Release: Journal module and bug fixes"
git push origin v1.2.0
```

### Emergency Hotfix

```bash
# 1. Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/fix-authentication-error

# 2. Fix the issue
# ... make changes ...
git commit -m "fix: resolve authentication token expiry"

# 3. Push and create PRs
git push origin hotfix/fix-authentication-error

# 4. Create two PRs:
# - hotfix -> main (for immediate deployment)
# - hotfix -> dev (to include fix in development)

# 5. After both merges, clean up
git branch -d hotfix/fix-authentication-error
```

## Best Practices

1. **Always create feature branches from `dev`**
   - Ensures you're working with the latest integrated code

2. **Keep feature branches small and focused**
   - Easier to review and less likely to have conflicts

3. **Regularly sync with `dev`**
   - Use rebase to keep a clean history
   - Resolve conflicts early

4. **Write descriptive branch names**
   - Good: `feature/add-journal-peer-review`
   - Bad: `feature/fix-stuff`

5. **Delete branches after merge**
   - Keeps the repository clean
   - Use GitHub's auto-delete feature

6. **Never commit directly to `main` or `dev`**
   - Always use pull requests
   - Ensures code review and CI checks

## CI/CD Integration

- **Feature branches**: Run tests and linting
- **dev branch**: Deploy to staging environment
- **main branch**: Deploy to production environment

## Common Commands

```bash
# View all branches
git branch -a

# Delete local branch
git branch -d feature/branch-name

# Delete remote branch
git push origin --delete feature/branch-name

# Rebase feature branch
git checkout feature/my-feature
git rebase dev

# Check branch tracking
git branch -vv
```