# Contributing to History as Code

We welcome contributions to History as Code! This document provides guidelines for contributing to the project.

## GitFlow Branching Strategy

We use GitFlow for managing our development process:

```
main (production)
  └── dev (development/integration)
        └── feature/feature-name (feature branches)
```

### Branch Types

1. **main**: Production branch
   - Contains production-ready code
   - Protected branch - requires PR review
   - Automatically deploys to production

2. **dev**: Development/Integration branch
   - Default branch for the repository
   - All features are merged here first
   - Used for integration testing
   - Automatically deploys to staging environment

3. **feature/\***: Feature branches
   - Created from `dev`
   - Named as `feature/descriptive-name`
   - Merged back into `dev` via PR

## Development Workflow

### Starting a New Feature

```bash
# Ensure you have the latest dev branch
git checkout dev
git pull origin dev

# Create a new feature branch
git checkout -b feature/your-feature-name

# Make your changes
# ...

# Commit your changes
git add .
git commit -m "feat: add your feature description"

# Push to GitHub
git push -u origin feature/your-feature-name
```

### Creating a Pull Request

1. Push your feature branch to GitHub
2. Create a PR from your feature branch to `dev`
3. Fill out the PR template with:
   - Description of changes
   - Related issues
   - Testing performed
   - Screenshots (if UI changes)
4. Request review from maintainers
5. Address any feedback
6. Once approved, the PR will be merged

### Release Process

1. When `dev` is stable and tested:
   ```bash
   # Create a PR from dev to main
   git checkout main
   git pull origin main
   git checkout dev
   git pull origin dev
   
   # Create PR via GitHub UI from dev to main
   ```

2. After review and approval, merge to `main`
3. Tag the release:
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```

## Commit Message Convention

We follow Conventional Commits:

- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation changes
- `style:` Code style changes (formatting, etc.)
- `refactor:` Code refactoring
- `test:` Test additions or changes
- `chore:` Maintenance tasks
- `perf:` Performance improvements

Examples:
```
feat: add user authentication with Supabase
fix: resolve navigation menu overflow on mobile
docs: update API documentation for Lab endpoints
```

## Code Quality Standards

### Before Submitting a PR

```bash
# Run linting
npm run lint

# Check TypeScript types
npm run type-check

# Format code
npm run format

# Run tests (when available)
npm test
```

### Code Style Guidelines

- Use TypeScript for all new code
- Follow ESLint and Prettier configurations
- Write self-documenting code with clear variable names
- Add JSDoc comments for complex functions
- Keep components small and focused
- Use meaningful file and folder names

## Pull Request Guidelines

### PR Title Format

Use the same convention as commit messages:
```
feat: implement Journal article submission flow
fix: correct responsive layout in Directory search
```

### PR Description Template

```markdown
## Description
Brief description of what this PR does.

## Related Issues
Closes #123
Relates to #456

## Changes Made
- Added X feature
- Fixed Y bug
- Updated Z documentation

## Testing
- [ ] Tested locally
- [ ] Added/updated tests
- [ ] Tested on mobile
- [ ] Tested cross-browser

## Screenshots
(If applicable)

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No console errors or warnings
```

## Getting Help

- Open an issue for bugs or feature requests
- Join discussions in GitHub Discussions
- Tag maintainers for urgent issues

## First-Time Contributors

Look for issues labeled `good first issue` or `help wanted`. These are great starting points for new contributors.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.