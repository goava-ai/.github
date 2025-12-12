# GitHub Actions Workflow Templates

This repository contains reusable GitHub Actions workflow templates for consistent CI/CD across projects. These templates ensure code quality, security, and testing standards are enforced automatically.

## 📋 Table of Contents

- [Overview](#overview)
- [Available Workflows](#available-workflows)
- [Quick Start](#quick-start)
- [Workflow Details](#workflow-details)
- [Usage Guidelines](#usage-guidelines)

## Overview

The workflow templates in this repository provide standardized CI/CD checks that can be easily integrated into any project. Each workflow includes:

- **Workflow file** (`.yml`) - The GitHub Actions workflow definition
- **Documentation** (`.md`) - Detailed setup and configuration instructions

All workflows are designed to be:
- ✅ Easy to integrate
- ✅ Well-documented
- ✅ Configurable
- ✅ Consistent across projects

## Available Workflows

### 1. 🔒 Detect Secrets

**File:** `workflow-templates/detect-secrets.yml`

Scans repositories for potential secrets (API keys, passwords, tokens, etc.) using `detect-secrets` to prevent accidental commits of sensitive information.

**Key Features:**
- Uses `detect-secrets[gibberish]==1.5.0`
- Requires `.secrets.baseline` file
- Runs on pushes and pull requests to `main`
- Python 3.11 environment

**Triggers:** `push`, `pull_request` (to `main` branch)

**See:** [detect-secrets.md](workflow-templates/detect-secrets.md) for detailed documentation

---

### 2. 🧹 ESLint & Prettier

**File:** `workflow-templates/eslint.yml`

Runs ESLint and Prettier to ensure code quality and consistent formatting for JavaScript/TypeScript projects.

**Key Features:**
- ESLint with auto-fix
- Prettier code formatting
- Uses Yarn as package manager
- Node.js 22
- Supports manual workflow dispatch

**Triggers:** `push`, `pull_request` (to `main` branch), `workflow_dispatch`

**Commands:**
- `yarn run fix .` - ESLint with auto-fix
- `yarn prettier --write .` - Prettier formatting

**See:** [eslint.md](workflow-templates/eslint.md) for detailed documentation

---

### 3. 🧪 NPM Test Suite

**File:** `workflow-templates/npm-test.yml`

Comprehensive test suite for Node.js projects including unit tests, coverage, and E2E tests.

**Key Features:**
- Build verification
- Unit tests
- Test coverage collection
- E2E tests
- Codecov integration (optional)
- Node.js 22.x with npm caching

**Triggers:** `push`, `pull_request` (to `main` branch)

**Required npm Scripts:**
- `npm run build` - Build the project
- `npm test` - Run unit tests
- `npm run test:cov` - Run tests with coverage
- `npm run test:e2e` - Run E2E tests

**See:** [npm-test.md](workflow-templates/npm-test.md) for detailed documentation

---

### 4. 🐍 Pylint

**File:** `workflow-templates/pylint.yml`

Analyzes Python code quality and enforces coding standards using Pylint.

**Key Features:**
- Multi-version Python testing (3.8, 3.9, 3.10)
- Scans all Python files tracked by git
- Configurable via `.pylintrc`, `pyproject.toml`, or `setup.cfg`
- Matrix strategy for version compatibility

**Triggers:** `push` (all branches)

**Command:**
- `pylint $(git ls-files '*.py')` - Lint all Python files

**See:** [pylint.md](workflow-templates/pylint.md) for detailed documentation

---

## Quick Start

### Using a Workflow Template

1. **Copy the workflow file** to your repository:
   ```bash
   cp .github/workflow-templates/[workflow-name].yml .github/workflows/[workflow-name].yml
   ```

2. **Read the documentation** for prerequisites and setup:
   ```bash
   cat .github/workflow-templates/[workflow-name].md
   ```

3. **Configure prerequisites** (e.g., create `.secrets.baseline`, set up ESLint config, etc.)

4. **Customize the workflow** (optional):
   - Adjust trigger branches
   - Modify versions (Node.js, Python, etc.)
   - Update paths or commands

5. **Commit and push** - The workflow will run automatically based on its triggers

### Example: Adding ESLint Workflow

```bash
# Copy the workflow
cp .github/workflow-templates/eslint.yml .github/workflows/eslint.yml

# Ensure your project has ESLint and Prettier configured
# (see eslint.md for details)

# Commit and push
git add .github/workflows/eslint.yml
git commit -m "Add ESLint workflow"
git push
```

## Workflow Details

### Workflow Comparison

| Workflow | Language | Package Manager | Trigger Events | Key Tools |
|----------|----------|----------------|----------------|-----------|
| **detect-secrets** | Python | pip | push, PR | detect-secrets 1.5.0 |
| **eslint** | JavaScript/TypeScript | Yarn | push, PR, manual | ESLint, Prettier |
| **npm-test** | JavaScript/TypeScript | npm | push, PR | npm, Codecov |
| **pylint** | Python | pip | push | Pylint |

### Common Prerequisites

Most workflows require:

1. **Repository access** - Workflows run in the repository context
2. **Configuration files** - Each workflow may need specific config files
3. **Dependencies** - Project dependencies must be installable
4. **Secrets** (optional) - Some workflows may need GitHub secrets (e.g., `CODECOV_TOKEN`)

### Trigger Configuration

Workflows can be customized to trigger on different events:

```yaml
on:
  push:
    branches: [main, develop]  # Customize branches
  pull_request:
    branches: [main]
  workflow_dispatch:  # Allow manual triggers
```

## Usage Guidelines

### Best Practices

1. **Start with one workflow** - Don't add all workflows at once. Start with the most critical for your project.

2. **Read the documentation** - Each workflow has detailed `.md` documentation with setup instructions.

3. **Test locally first** - Run the tools locally before adding the workflow to ensure they work.

4. **Customize appropriately** - Adjust versions, branches, and paths to match your project needs.

5. **Monitor workflow runs** - Check the Actions tab to ensure workflows run successfully.

### Workflow Selection Guide

**Choose `detect-secrets` if:**
- Your project handles sensitive data
- You want to prevent secret leaks
- You need security scanning

**Choose `eslint` if:**
- You have JavaScript/TypeScript code
- You want consistent code style
- You need automatic formatting

**Choose `npm-test` if:**
- You have a Node.js project
- You want automated testing
- You need coverage reporting

**Choose `pylint` if:**
- You have Python code
- You want code quality checks
- You need style enforcement

### Combining Workflows

You can use multiple workflows together:

```bash
# Example: Full-stack project with Python backend and JS frontend
cp .github/workflow-templates/detect-secrets.yml .github/workflows/
cp .github/workflow-templates/pylint.yml .github/workflows/
cp .github/workflow-templates/eslint.yml .github/workflows/
cp .github/workflow-templates/npm-test.yml .github/workflows/
```

## Documentation

Each workflow template includes comprehensive documentation:

- **[detect-secrets.md](workflow-templates/detect-secrets.md)** - Secret detection setup
- **[eslint.md](workflow-templates/eslint.md)** - ESLint & Prettier configuration
- **[npm-test.md](workflow-templates/npm-test.md)** - Test suite setup
- **[pylint.md](workflow-templates/pylint.md)** - Python linting configuration

## Troubleshooting

### Common Issues

**Workflow not running:**
- Check that the workflow file is in `.github/workflows/`
- Verify trigger conditions (branches, events)
- Ensure the workflow file has valid YAML syntax

**Workflow failing:**
- Check the Actions tab for error messages
- Verify all prerequisites are met
- Review the workflow's documentation for troubleshooting tips

**Configuration issues:**
- Ensure required config files exist (`.secrets.baseline`, `.eslintrc`, etc.)
- Verify package.json scripts match workflow expectations
- Check that dependencies are properly installed

## Contributing

When adding or modifying workflow templates:

1. Update both the `.yml` and `.md` files
2. Test the workflow in a sample repository
3. Document any prerequisites clearly
4. Include troubleshooting tips
5. Keep versions up to date

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Profile README](profile/README.md) - Organization profile information

---

**Note:** These workflow templates are designed to be copied into individual repositories. They are not meant to be used directly from this repository.
