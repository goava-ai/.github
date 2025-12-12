Please read `/profile/README.md` for more details.

## Pre-commit Hooks

This repository includes pre-commit hooks that mirror the CI workflow checks. This ensures code quality checks run locally before code is pushed, reducing CI failures and improving developer experience.

### Quick Setup

1. **Install pre-commit**:
   ```bash
   pip install pre-commit
   ```

2. **Install the git hooks**:
   ```bash
   pre-commit install
   ```

3. **Run manually** (optional, to test all files):
   ```bash
   pre-commit run --all-files
   ```

### What's Included

The pre-commit configuration (`.pre-commit-config.yaml`) includes hooks that mirror the CI workflows:

- **detect-secrets**: Scans for potential secrets using the same baseline as CI
- **ESLint**: Lints JavaScript/TypeScript files with auto-fix
- **Prettier**: Formats code automatically
- **Pylint**: Analyzes Python code quality
- **General file checks**: Trailing whitespace, end-of-file fixes, YAML/JSON validation, etc.

### Workflow Templates

This repository contains reusable GitHub Actions workflow templates in `workflow-templates/`:

- `detect-secrets.yml` - Secret detection
- `eslint.yml` - ESLint and Prettier checks
- `npm-test.yml` - Node.js test suite
- `pylint.yml` - Python linting

See the individual `.md` files in `workflow-templates/` for detailed setup instructions for each workflow.
