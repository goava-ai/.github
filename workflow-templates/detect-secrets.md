# Detect Secrets Workflow

## Overview
This GitHub Actions workflow scans your repository for potential secrets (API keys, passwords, tokens, etc.) using `detect-secrets`. It helps prevent accidental commits of sensitive information.

## Prerequisites

### 1. Secrets Baseline File
You need to create a `.secrets.baseline` file in your repository root. This file stores known false positives and allows the tool to track legitimate secrets that should be ignored.

**To generate the baseline file:**
```bash
# Install detect-secrets
pip install "detect-secrets[gibberish]==1.5.0"

# Generate baseline (run this once)
detect-secrets scan > .secrets.baseline
```

**Important:** After generating the baseline, review it carefully and add any legitimate secrets that should be whitelisted. Commit the `.secrets.baseline` file to your repository.

### 2. Python Environment
- Python 3.11 (as specified in the workflow)
- The workflow will automatically set up Python, but ensure your repository is compatible with Python 3.11

## Integration Steps

1. **Copy the workflow file** to your repository:
   ```
   .github/workflows/detect-secrets.yml
   ```

2. **Generate the baseline file** (if you don't have one):
   ```bash
   pip install "detect-secrets[gibberish]==1.5.0"
   detect-secrets scan > .secrets.baseline
   ```

3. **Review and commit the baseline file**:
   - Open `.secrets.baseline` and verify it doesn't contain actual secrets
   - Add any known false positives to the baseline
   - Commit the file to your repository

4. **Customize the workflow** (optional):
   - Adjust the Python version if needed
   - Modify the trigger branches (currently set to `main`)
   - Update the detect-secrets version if desired

## How It Works

- The workflow runs on pushes and pull requests to the `main` branch
- It scans all files tracked by git using `detect-secrets-hook`
- It compares findings against the `.secrets.baseline` file
- The workflow will fail if new secrets are detected that aren't in the baseline

## Troubleshooting

- **Workflow fails with "baseline not found"**: Make sure `.secrets.baseline` exists in your repository root
- **False positives**: Add legitimate patterns to your baseline file using `detect-secrets audit .secrets.baseline`
- **Python version issues**: Update the `python-version` in the workflow file if needed

