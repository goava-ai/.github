# ESLint & Prettier Workflow

## Overview
This GitHub Actions workflow runs ESLint and Prettier to ensure code quality and consistent formatting for JavaScript/TypeScript projects. It automatically fixes linting issues and formats code.

## Prerequisites

### 1. Node.js Project Setup
- Node.js 22 (as specified in the workflow)
- A `package.json` file in your repository root

### 2. Package Manager: Yarn
The workflow uses `yarn` as the package manager. Make sure you have:
- `yarn` installed locally (for development)
- A `yarn.lock` file committed to your repository (recommended)

### 3. ESLint Configuration
You need one of the following ESLint configuration files:
- `.eslintrc.js`
- `.eslintrc.json`
- `.eslintrc.yml` / `.eslintrc.yaml`
- `eslint.config.js` (ESLint 9+ flat config)
- Or ESLint configuration in `package.json`

### 4. Prettier Configuration (Optional but Recommended)
You may want one of the following Prettier configuration files:
- `.prettierrc`
- `.prettierrc.json`
- `.prettierrc.js`
- `.prettierrc.yml` / `.prettierrc.yaml`
- Or Prettier configuration in `package.json`

### 5. Required Dependencies
Your `package.json` should include:
```json
{
  "devDependencies": {
    "eslint": "^8.0.0",
    "prettier": "^2.0.0"
  },
  "scripts": {
    "fix": "eslint --fix",
    "prettier": "prettier"
  }
}
```

## Integration Steps

1. **Copy the workflow file** to your repository:
   ```
   .github/workflows/eslint.yml
   ```

2. **Install dependencies** (if not already present):
   ```bash
   yarn add -D eslint prettier
   ```

3. **Set up ESLint configuration**:
   ```bash
   # Option 1: Use a popular preset
   yarn add -D eslint-config-standard eslint-plugin-import eslint-plugin-node eslint-plugin-promise
   
   # Option 2: Initialize ESLint interactively
   yarn eslint --init
   ```

4. **Set up Prettier configuration** (optional):
   Create `.prettierrc`:
   ```json
   {
     "semi": true,
     "singleQuote": true,
     "tabWidth": 2
   }
   ```

5. **Add scripts to package.json**:
   ```json
   {
     "scripts": {
       "fix": "eslint --fix",
       "prettier": "prettier"
     }
   }
   ```

6. **Customize the workflow** (optional):
   - Adjust Node.js version if needed
   - Change trigger branches (currently set to `main`)
   - Modify the paths being linted (currently set to `.`)

## How It Works

- The workflow runs on pushes, pull requests to `main`, and manual triggers (`workflow_dispatch`)
- It installs dependencies using `yarn install`
- Runs ESLint with auto-fix: `yarn run fix .`
- Runs Prettier to format code: `yarn prettier --write .`
- The workflow will fail if there are unfixable linting errors

## Customization Options

- **Change package manager**: Replace `yarn` with `npm` or `pnpm` throughout the workflow
- **Lint specific directories**: Change `.` to specific paths like `src/` or `lib/`
- **Node.js version**: Update `node-version` in the workflow file
- **Add caching**: Consider adding `actions/cache` to speed up dependency installation

## Troubleshooting

- **"yarn: command not found"**: The workflow uses yarn, ensure your project uses yarn (or modify the workflow to use npm/pnpm)
- **ESLint errors**: Review your ESLint configuration and ensure all required plugins are installed
- **Prettier conflicts**: Ensure ESLint and Prettier are configured to work together (consider using `eslint-config-prettier`)

