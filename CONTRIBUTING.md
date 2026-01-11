# Contributing to Research Space

Thank you for your interest in contributing to Research Space! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Branch Naming Convention](#branch-naming-convention)
- [Commit Message Format](#commit-message-format)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Code Style Guidelines](#code-style-guidelines)
- [Testing](#testing)
- [Reporting Issues](#reporting-issues)

---

## Getting Started

### Prerequisites

- **Node.js** 18+ ([Download](https://nodejs.org/))
- **npm** 9+ (comes with Node.js)
- **Git** ([Download](https://git-scm.com/))

### Development Setup

1. **Fork the repository**

   Click the "Fork" button on [GitHub](https://github.com/PRATS-gits/research-vite-app)

2. **Clone your fork**

   ```bash
   git clone https://github.com/YOUR_USERNAME/research-vite-app.git
   cd research-vite-app
   ```

3. **Install dependencies**

   ```bash
   # Install frontend dependencies
   npm install

   # Install backend dependencies
   cd backend
   npm install
   ```

4. **Configure environment**

   ```bash
   # Backend environment
   cd backend
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. **Setup database**

   ```bash
   cd backend
   npx prisma generate
   npx prisma migrate dev
   ```

6. **Start development servers**

   ```bash
   # Terminal 1 - Frontend (from root)
   npm run dev

   # Terminal 2 - Backend
   cd backend
   npm run dev
   ```

---

## Development Workflow

### Branch Protection

> **Important**: The `main` branch has protection rules enabled. All changes must be submitted via Pull Request.

1. Create a new branch from `main`
2. Make your changes
3. Submit a Pull Request
4. Wait for review and approval
5. Changes are merged after approval

---

## Branch Naming Convention

Use descriptive branch names with the following prefixes:

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New features | `feature/file-preview` |
| `fix/` | Bug fixes | `fix/upload-progress-bar` |
| `docs/` | Documentation updates | `docs/api-reference` |
| `refactor/` | Code refactoring | `refactor/library-store` |
| `test/` | Adding/updating tests | `test/file-controller` |
| `chore/` | Maintenance tasks | `chore/update-deps` |

**Examples:**

```bash
git checkout -b feature/share-link-expiry
git checkout -b fix/folder-deletion-cascade
git checkout -b docs/deployment-guide
```

---

## Commit Message Format

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only changes |
| `style` | Code style changes (formatting, semicolons, etc.) |
| `refactor` | Code refactoring (no feature/fix) |
| `perf` | Performance improvements |
| `test` | Adding or updating tests |
| `build` | Build system or dependencies |
| `ci` | CI/CD configuration |
| `chore` | Other changes (maintenance) |
| `revert` | Revert a previous commit |

### Scope (Optional)

The scope indicates the area of the codebase affected:

- `frontend` - Frontend React application
- `backend` - Backend Express API
- `api` - API endpoints
- `ui` - UI components
- `db` - Database/Prisma changes
- `deps` - Dependencies
- `config` - Configuration files

### Examples

```bash
# Feature
feat(frontend): add file preview modal with zoom controls

# Bug fix
fix(api): resolve presigned URL expiration timing issue

# Documentation
docs: update API documentation with new endpoints

# Refactor
refactor(backend): extract S3 operations into separate service

# Breaking change (use ! after type)
feat(api)!: change file list response structure

BREAKING CHANGE: The files array now includes nested folder information.
```

---

## Pull Request Guidelines

### Before Submitting

1. **Ensure your branch is up to date**

   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Run linting**

   ```bash
   npm run lint
   cd backend && npm run lint
   ```

3. **Run type checking**

   ```bash
   npm run type-check
   cd backend && npm run type-check
   ```

4. **Test your changes locally**

### PR Template

When creating a Pull Request, include:

```markdown
## Description
Brief description of the changes made.

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation update

## Related Issues
Closes #(issue number)

## Checklist
- [ ] My code follows the project's coding standards
- [ ] I have updated the documentation accordingly
- [ ] I have added tests that prove my fix/feature works
- [ ] All new and existing tests pass locally
- [ ] I have run linting and type-checking

## Screenshots (if applicable)
Add screenshots for UI changes.
```

### Review Process

1. At least one maintainer review is required
2. All CI checks must pass
3. Conflicts must be resolved
4. Commits should be squashed if requested

---

## Code Style Guidelines

### General

- Use **TypeScript** for all code
- Enable **strict mode** in TypeScript
- Use **ESLint** and **Prettier** for formatting
- Follow existing patterns in the codebase

### Frontend (React/TypeScript)

- Use functional components with hooks
- Use `const` for component declarations
- Prefix interface names with `I` for props (e.g., `IFileCardProps`)
- Use named exports for components
- Place hooks at the top of components
- Organize imports: React → Third-party → Local

### Backend (Express/TypeScript)

- Use controller-service-model pattern
- Validate all inputs with Joi/Zod
- Return consistent JSON response format
- Handle errors with try-catch and error middleware
- Use async/await (no callbacks)

### Documentation

For detailed coding standards, see:

- [CODING_STANDARDS.md](docs/rules/CODING_STANDARDS.md)
- [PLAN_RULE.md](docs/rules/PLAN_RULE.md)

---

## Testing

### Running Tests

```bash
# Frontend (when implemented)
npm run test

# Backend
cd backend
npm run test
npm run test:coverage
```

### Writing Tests

- Write unit tests for business logic
- Write integration tests for API endpoints
- Use descriptive test names
- Follow AAA pattern (Arrange, Act, Assert)

---

## Reporting Issues

### Bug Reports

Create an issue with:

- Clear, descriptive title
- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment details (OS, Node version, browser)
- Screenshots (if applicable)

### Feature Requests

Create an issue with:

- Clear description of the feature
- Use case / problem it solves
- Proposed implementation (optional)
- Any alternatives considered

### Where to Report

- **GitHub Issues**: [Create Issue](https://github.com/PRATS-gits/research-vite-app/issues)
- **Discussions**: [GitHub Discussions](https://github.com/PRATS-gits/research-vite-app/discussions)

---

## Questions?

If you have questions about contributing:

1. Check existing [documentation](PROJECT_OVERVIEW.md)
2. Search [existing issues](https://github.com/PRATS-gits/research-vite-app/issues)
3. Start a [discussion](https://github.com/PRATS-gits/research-vite-app/discussions)

---

**Thank you for contributing to Research Space!** 🚀
