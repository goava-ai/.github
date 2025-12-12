# Pylint Workflow

## Overview
This GitHub Actions workflow runs Pylint to analyze Python code quality and enforce coding standards. It tests your code against multiple Python versions (3.8, 3.9, and 3.10) to ensure compatibility.

## Prerequisites

### 1. Python Project
- Python files (`.py`) in your repository
- The workflow will automatically scan all Python files tracked by git

### 2. Pylint Configuration (Optional but Recommended)
You can configure Pylint using one of the following:
- `.pylintrc` file in the repository root
- `pyproject.toml` with a `[tool.pylint]` section
- `setup.cfg` with a `[pylint]` section
- Command-line arguments (not recommended for CI)

### 3. Requirements File (Optional)
If your project has dependencies, consider including:
- `requirements.txt` or
- `pyproject.toml` with dependencies

## Integration Steps

1. **Copy the workflow file** to your repository:
   ```
   .github/workflows/pylint.yml
   ```

2. **Create a Pylint configuration file** (recommended):
   
   **Option A: Generate default config:**
   ```bash
   pylint --generate-rcfile > .pylintrc
   ```
   
   **Option B: Create minimal `.pylintrc`:**
   ```ini
   [MASTER]
   ignore=venv,__pycache__,migrations
   
   [MESSAGES CONTROL]
   disable=C0111  # missing-docstring
   ```

3. **Customize the workflow** (optional):
   - Adjust Python versions in the matrix (currently: 3.8, 3.9, 3.10)
   - Modify the file pattern if needed (currently: `*.py`)
   - Add installation of project dependencies if required:
     ```yaml
     - name: Install dependencies
       run: |
         pip install -r requirements.txt
     ```

4. **Test locally** (optional):
   ```bash
   pip install pylint
   pylint $(git ls-files '*.py')
   ```

## How It Works

- The workflow runs on every push to any branch
- It uses a matrix strategy to test against Python 3.8, 3.9, and 3.10
- It scans all Python files tracked by git: `$(git ls-files '*.py')`
- The workflow will fail if Pylint finds issues above the configured threshold

## Customization Options

### Python Versions
Modify the matrix to test different Python versions:
```yaml
python-version: ["3.9", "3.10", "3.11", "3.12"]
```

### Specific Files/Directories
To lint only specific paths:
```yaml
pylint src/ tests/
```

### Pylint Options
Add command-line options:
```yaml
pylint --fail-under=8.0 $(git ls-files '*.py')
```

### Install Dependencies
If your project has dependencies:
```yaml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install -r requirements.txt
    pip install pylint
```

## Pylint Configuration Tips

### Common `.pylintrc` settings:
```ini
[MASTER]
# Ignore directories
ignore=venv,__pycache__,migrations,.venv

[MESSAGES CONTROL]
# Disable specific checks
disable=missing-docstring,too-few-public-methods

[FORMAT]
# Line length
max-line-length=120

[BASIC]
# Good variable names
good-names=i,j,k,ex,Run,_,id,db
```

## Troubleshooting

- **"No module named X" errors**: Install project dependencies before running Pylint
- **Too many warnings**: Adjust your `.pylintrc` to disable checks you don't need
- **Python version issues**: Ensure the Python versions in the matrix are supported by GitHub Actions
- **Slow execution**: Consider limiting the files being linted or using `--jobs=0` for parallel processing

