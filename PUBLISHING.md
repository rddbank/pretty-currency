# Publishing Guide

## Prerequisites

### 1. npm Registry Setup

Before publishing, ensure you have an npm account:
- Sign up at https://www.npmjs.com/signup
- Run `npm login` to authenticate locally

### 2. GitHub Secrets Setup

For automatic publishing via GitHub Actions, you need to add an NPM_TOKEN:

1. Go to your GitHub repository settings
2. Navigate to: Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `NPM_TOKEN`
5. Value: Your npm authentication token (get it from https://www.npmjs.com/settings/tokens)

**Important:** Create an "Automation" token (not "Read-only") for CI/CD publishing.

## Manual Publishing

To publish manually to npm:

```bash
# Run tests first
npm test

# Publish to npm
npm publish
```

## Automated Publishing

The GitHub Actions workflow in `.github/workflows/ci.yml` will:

1. **On every push/PR**: Run tests on Node.js 18, 20, and 22
2. **On push to main**: Publish to npm (if tests pass)

## Version Management

To publish a new version:

```bash
# Update version (follows semantic versioning)
npm version patch  # 0.0.1 -> 0.0.2
npm version minor  # 0.0.1 -> 0.1.0
npm version major  # 0.0.1 -> 1.0.0

# This will also create a git tag
# Push the tag to trigger CI/CD publishing
git push --tags
```

## Pre-release Publishing

For alpha/beta releases:

```bash
npm version prerelease --preid alpha  # 0.0.1 -> 0.0.2-alpha.0
npm publish --tag alpha
```

## Checklist Before Publishing

- [ ] All tests pass (`npm test`)
- [ ] README.md is updated with usage examples
- [ ] package.json has correct version
- [ ] package.json has correct keywords and description
- [ ] License is properly set
- [ ] .npmignore excludes test files and development configs
- [ ] GitHub Actions workflow is configured
- [ ] NPM_TOKEN secret is set in GitHub (for automated publishing)

## Post-Publishing

After publishing:

1. Verify the package on npm: https://www.npmjs.com/package/pretty-currency
2. Install and test: `npm install pretty-currency`
3. Update GitHub release notes with changelog
4. Announce the release (if applicable)

## Troubleshooting

### "You do not have permission to publish this package"
- Ensure you're logged in to npm with the correct account
- Verify you're listed as a maintainer on npm

### "403 Forbidden" in GitHub Actions
- Check that NPM_TOKEN secret is set correctly
- Ensure the token has "Automation" permissions, not "Read-only"

### Package name already taken
- Check npm for availability: `npm view <package-name>`
- Choose a different name or use scoped package: `@username/package-name`
