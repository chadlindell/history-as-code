# Contributing to History as Code

We welcome contributions to History as Code! This document provides guidelines for contributing to the project.

## Development Workflow

1. Fork the repository
2. Create a feature branch from `dev`: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Run tests and ensure code quality:
   ```bash
   npm run lint
   npm run type-check
   npm run format
   ```
5. Commit your changes using conventional commits:
   - `feat:` for new features
   - `fix:` for bug fixes
   - `docs:` for documentation changes
   - `style:` for formatting changes
   - `refactor:` for code refactoring
   - `test:` for test additions/changes
   - `chore:` for maintenance tasks
6. Push to your fork and create a pull request to the `dev` branch

## Code Style

- We use TypeScript for type safety
- Follow the ESLint and Prettier configurations
- Write meaningful commit messages
- Add comments for complex logic
- Keep functions small and focused

## Testing

- Write tests for new features
- Ensure existing tests pass
- Aim for good test coverage

## Pull Request Process

1. Update the README.md with details of changes if applicable
2. Ensure your PR description clearly describes the problem and solution
3. Link any relevant issues
4. Request review from maintainers

## Questions?

Feel free to open an issue for any questions about contributing.