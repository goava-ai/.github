# NPM Test Workflow

## Overview
This GitHub Actions workflow runs a comprehensive test suite for Node.js projects. It builds the project, runs unit tests with coverage, executes end-to-end (E2E) tests, and uploads coverage reports to Codecov.

## Prerequisites

### 1. Node.js Project Setup
- Node.js 22.x (as specified in the workflow)
- A `package.json` file in your repository root
- `package-lock.json` file (required for `npm ci`)

### 2. Required npm Scripts
Your `package.json` must include the following scripts:
```json
{
  "scripts": {
    "build": "your-build-command",
    "test": "your-test-command",
    "test:cov": "your-coverage-command",
    "test:e2e": "your-e2e-test-command"
  }
}
```

### 3. Test Framework
You need a testing framework installed. Common options:
- **Jest** (recommended for coverage)
- **Mocha** with **nyc** or **c8** for coverage
- **Vitest**
- **Ava**

### 4. Coverage Tool
For the `test:cov` script, you need a coverage tool:
- **Jest** (built-in coverage)
- **nyc** (for Mocha/Jest)
- **c8** (for Node.js native test runner)
- **Vitest** (built-in coverage)

### 5. Codecov Integration (Optional but Recommended)
To upload coverage reports:
- A Codecov account (free for open source projects)
- A Codecov token stored as a GitHub secret named `CODECOV_TOKEN`

## Integration Steps

1. **Copy the workflow file** to your repository:
   ```
   .github/workflows/npm-test.yml
   ```

2. **Set up your package.json scripts**:
   
   **Example with Jest:**
   ```json
   {
     "scripts": {
       "build": "tsc",
       "test": "jest",
       "test:cov": "jest --coverage",
       "test:e2e": "jest --config jest.e2e.config.js"
     },
     "devDependencies": {
       "jest": "^29.0.0",
       "@types/jest": "^29.0.0"
     }
   }
   ```
   
   **Example with Mocha and nyc:**
   ```json
   {
     "scripts": {
       "build": "tsc",
       "test": "mocha",
       "test:cov": "nyc --reporter=lcov --reporter=text mocha",
       "test:e2e": "mocha test/e2e/**/*.test.js"
     },
     "devDependencies": {
       "mocha": "^10.0.0",
       "nyc": "^15.0.0"
     }
   }
   ```

3. **Configure coverage output**:
   Ensure your coverage tool generates `lcov.info` format in the `./coverage/` directory:
   
   **Jest configuration (jest.config.js):**
   ```javascript
   module.exports = {
     coverageDirectory: 'coverage',
     collectCoverageFrom: ['src/**/*.{js,ts}'],
     coverageReporters: ['lcov', 'text', 'html']
   };
   ```
   
   **nyc configuration (.nycrc.json):**
   ```json
   {
     "reporter": ["lcov", "text", "html"],
     "report-dir": "coverage"
   }
   ```

4. **Set up Codecov** (optional):
   
   a. Sign up at [codecov.io](https://codecov.io)
   
   b. Add your repository to Codecov
   
   c. Get your repository token from Codecov
   
   d. Add it as a GitHub secret:
      - Go to your repository → Settings → Secrets and variables → Actions
      - Click "New repository secret"
      - Name: `CODECOV_TOKEN`
      - Value: Your Codecov token
   
   e. If you don't want to use Codecov, you can remove or comment out the "Upload coverage reports" step

5. **Customize the workflow** (optional):
   - Adjust Node.js version in the matrix
   - Modify trigger branches
   - Add or remove test steps as needed
   - Configure different test commands

## How It Works

- **Triggers**: Runs on pushes and pull requests to the `main` branch
- **Node.js Setup**: Uses Node.js 22.x with npm caching enabled
- **Dependencies**: Installs dependencies using `npm ci` (clean install)
- **Build**: Runs the build script to compile/prepare the project
- **Unit Tests**: Executes unit tests
- **Coverage**: Runs tests with coverage collection
- **E2E Tests**: Runs end-to-end tests
- **Coverage Upload**: Uploads coverage reports to Codecov (if token is configured)

## Customization Options

### Node.js Versions
Test against multiple Node.js versions:
```yaml
matrix:
  node-version: [18.x, 20.x, 22.x]
```

### Operating Systems
Test on multiple platforms:
```yaml
strategy:
  matrix:
    node-version: [22.x]
    os: [ubuntu-latest, windows-latest, macos-latest]
runs-on: ${{ matrix.os }}
```

### Conditional Steps
Skip steps if scripts don't exist:
```yaml
- name: Build project
  run: npm run build
  continue-on-error: true  # Don't fail if build script doesn't exist
```

### Different Package Manager
To use yarn or pnpm instead:
```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: ${{ matrix.node-version }}
    cache: 'yarn'  # or 'pnpm'

- name: Install dependencies
  run: yarn install --frozen-lockfile  # or pnpm install --frozen-lockfile
```

### Coverage Paths
If your coverage file is in a different location:
```yaml
- name: Upload coverage reports
  uses: codecov/codecov-action@v4
  with:
    files: ./coverage/coverage.lcov  # Adjust path
```

## Common Test Setup Examples

### Jest Setup
```json
{
  "scripts": {
    "test": "jest",
    "test:cov": "jest --coverage --coverageReporters=lcov",
    "test:e2e": "jest --config jest.e2e.config.js"
  }
}
```

### Vitest Setup
```json
{
  "scripts": {
    "test": "vitest run",
    "test:cov": "vitest run --coverage",
    "test:e2e": "vitest run --config vitest.e2e.config.ts"
  }
}
```

### Mocha + nyc Setup
```json
{
  "scripts": {
    "test": "mocha 'test/**/*.test.js'",
    "test:cov": "nyc --reporter=lcov --reporter=text mocha 'test/**/*.test.js'",
    "test:e2e": "mocha 'test/e2e/**/*.test.js'"
  }
}
```

## Troubleshooting

- **"npm ci" fails**: Ensure `package-lock.json` exists and is up to date. Run `npm install` locally and commit the lock file.
- **"Script not found" errors**: Make sure all required scripts (`build`, `test`, `test:cov`, `test:e2e`) are defined in `package.json`.
- **Coverage file not found**: Verify your coverage tool generates `./coverage/lcov.info`. Check your test framework configuration.
- **Codecov upload fails**: 
  - Ensure `CODECOV_TOKEN` secret is set in repository settings
  - Check that the coverage file path matches (`./coverage/lcov.info`)
  - The workflow won't fail if Codecov upload fails (`fail_ci_if_error: false`)
- **Tests timeout**: Consider adding timeout configuration to your test framework or increasing GitHub Actions timeout.
- **Build step fails**: If your project doesn't need a build step, you can remove it or make it optional with `continue-on-error: true`.

## Best Practices

1. **Always commit package-lock.json**: Required for `npm ci` to work reliably
2. **Use npm ci**: Faster and more reliable than `npm install` in CI
3. **Separate unit and E2E tests**: Keep them in separate scripts for better organization
4. **Set coverage thresholds**: Configure minimum coverage requirements in your test framework
5. **Cache dependencies**: The workflow already uses npm caching, but you can add explicit caching for faster runs
6. **Run tests in parallel**: Configure your test framework to run tests in parallel when possible

