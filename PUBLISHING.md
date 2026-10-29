# Publishing Guide

## Prerequisites

### 1. npm Registry Setup

Before publishing, ensure you have an npm account:
- Sign up at https://www.npmjs.com/signup
- Run `npm login` to authenticate locally

### 2. GitHub Actions Setup

The GitHub Actions workflow will:
- Run tests on Node.js 18, 20, and 22 for every push/PR
- Create a GitHub Release when you push a tag

No additional secrets are required for GitHub Releases (uses built-in GITHUB_TOKEN).

## Manual Publishing

To publish manually to npm:

```bash
# Run tests first
npm test

# Publish to npm
npm publish
```

## Automated Publishing

### GitHub Releases

The GitHub Actions workflow in `.github/workflows/ci.yml` will:

1. **On every push/PR**: Run tests on Node.js 18, 20, and 22
2. **On tag push**: Create a GitHub Release with auto-generated release notes

To create a release:

```bash
# Create and push a tag
npm version patch  # or minor/major
git push --tags

# This will trigger the GitHub Release workflow
```

### npm Publishing

npm publishing is manual (to avoid accidental releases):

```bash
npm publish
```

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

### Package name already taken
- Check npm for availability: `npm view <package-name>`
- Choose a different name or use scoped package: `@username/package-name`

### GitHub Release not created
- Ensure you pushed a tag: `git push --tags`
- Check that the tag follows semantic versioning (e.g., v1.0.0)
- Verify tests passed in the CI workflow
