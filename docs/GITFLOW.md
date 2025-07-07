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
│   ├── feature/user-authentication (short-lived)
│   ├── feature/journal-submission (short-lived)
│   ├── feature/directory-search (short-lived)
│   └── feature/lab-integration (short-lived)
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
- **Lifetime**: SHORT-LIVED - deleted immediately after merge
- **Duration**: Ideally 1-2 weeks maximum

### Hotfix Branches (`hotfix/*`)
- **Purpose**: Emergency fixes for production
- **Naming**: `hotfix/issue-description`
- **Created from**: `main`
- **Merges to**: Both `main` and `dev`
- **Lifetime**: VERY SHORT-LIVED - deleted immediately after merge

## Feature Branch Best Practices

### Keep Feature Branches Short-Lived

1. **Small, Focused Features**
   - Break large features into smaller, mergeable chunks
   - Each feature branch should represent 1-2 weeks of work maximum
   - If a feature takes longer, consider splitting it

2. **Regular Merging**
   - Merge to `dev` as soon as the feature is complete and tested
   - Don't let feature branches live for months
   - Long-lived branches lead to:
     - Merge conflicts
     - Integration issues
     - Divergence from the main codebase
     - Difficult code reviews

3. **Feature Flags for Large Features**
   - Use feature flags for features that need incremental deployment
   - Merge code regularly even if the feature isn't user-visible yet
   - This keeps branches short-lived while allowing gradual rollout

## Workflow Examples

### Standard Feature Development (Short-Lived)

```bash
# Day 1: Start feature
git checkout dev
git pull origin dev
git checkout -b feature/add-user-profiles

# Days 1-5: Development
# Make small, focused commits
git add .
git commit -m "feat: add user profile schema"
# ... more commits ...
git commit -m "feat: add profile edit form"

# Day 3: Stay synchronized
git fetch origin
git rebase origin/dev

# Day 5: Feature complete, create PR
git push origin feature/add-user-profiles
# Create PR to dev

# Day 6: After PR approval and merge
git checkout dev
git pull origin dev
git branch -d feature/add-user-profiles
git push origin --delete feature/add-user-profiles
```

### Breaking Down Large Features

Instead of one long-lived branch:
```
❌ feature/complete-journal-module (lives for 6 weeks)
```

Create multiple short-lived branches:
```
✅ feature/journal-article-schema (Week 1)
✅ feature/journal-submission-api (Week 2)
✅ feature/journal-review-workflow (Week 3)
✅ feature/journal-publishing-ui (Week 4)
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

### Emergency Hotfix (Also Short-Lived)

```bash
# 1. Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/fix-authentication-error

# 2. Fix the issue quickly
# ... make minimal changes ...
git commit -m "fix: resolve authentication token expiry"

# 3. Push and create PRs immediately
git push origin hotfix/fix-authentication-error

# 4. Create two PRs:
# - hotfix -> main (for immediate deployment)
# - hotfix -> dev (to include fix in development)

# 5. After both merges, clean up immediately
git branch -d hotfix/fix-authentication-error
git push origin --delete hotfix/fix-authentication-error
```

## Branch Lifetime Guidelines

| Branch Type | Maximum Lifetime | Ideal Lifetime |
|------------|-----------------|----------------|
| feature/*   | 2 weeks        | 3-5 days       |
| hotfix/*    | 24 hours       | 2-4 hours      |
| dev         | Permanent      | -              |
| main        | Permanent      | -              |

## Red Flags: When Branches Live Too Long

- Feature branch older than 2 weeks
- More than 50 commits in a single feature branch
- Repeated merge conflicts when rebasing
- PR with more than 500 lines changed
- Multiple developers working on the same feature branch

## Best Practices Summary

1. **Create small, focused feature branches**
2. **Merge early and often**
3. **Delete branches immediately after merge**
4. **Use feature flags for gradual rollouts**
5. **Break large features into smaller pieces**
6. **Rebase frequently to avoid conflicts**
7. **Keep PRs reviewable (< 500 lines)**

## Common Commands

```bash
# View all branches with last commit date
git for-each-ref --sort='-committerdate' --format='%(refname:short) %(committerdate:relative)' refs/heads/

# Find old feature branches (> 2 weeks)
git for-each-ref --format='%(refname:short) %(committerdate:relative)' refs/heads/feature/* | grep -E 'weeks|months'

# Delete merged branches
git branch --merged dev | grep -E 'feature/' | xargs -n 1 git branch -d

# Clean up remote tracking branches
git remote prune origin
```

## Automation

Consider setting up:
- Automated branch deletion after PR merge
- Warnings for branches older than 1 week
- PR size checks to encourage smaller changes
- Daily reminders for old PRs