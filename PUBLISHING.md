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

## Automated Release Workflow

This project uses **standard-version** for automated version management and changelog generation.

### Conventional Commits

To enable automatic version bumps, commit messages must follow [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>[optional scope]: <description>

[optional body]

[optional footer]
```

**Types:**
- `feat:` New feature (triggers minor version bump)
- `fix:` Bug fix (triggers patch version bump)
- `docs:` Documentation changes
- `perf:` Performance improvements
- `chore:` Maintenance tasks
- `refactor:` Code refactoring
- `test:` Test changes
- `ci:` CI/CD changes
- `style:` Code style changes

**Examples:**
```bash
git commit -m "feat: add support for custom decimal precision"
git commit -m "fix: handle negative numbers correctly"
git commit -m "docs: update README with examples"
```

### Creating a Release

The release process is automated:

```bash
# 1. Make your changes with conventional commits
git add .
git commit -m "feat: add new feature"

# 2. Run the release command
npm run release

# This will:
# - Automatically bump version based on commits
# - Update CHANGELOG.md
# - Update package.json
# - Create a git tag
# - Commit the changes

# 3. Push to GitHub
git push --follow-tags

# This will trigger GitHub Actions to:
# - Run tests
# - Create a GitHub Release with changelog
```

### Manual Version Bumps

If you need to force a specific version type:

```bash
npm run release:patch   # 0.2.0 -> 0.2.1
npm run release:minor   # 0.2.0 -> 0.3.0
npm run release:major   # 0.2.0 -> 1.0.0
```

### First Release

For the first release of this project:

```bash
npm run release:first
git push --follow-tags
```

## Publishing to npm

After creating a release with standard-version:

```bash
# The package is ready to publish
npm publish
```

This will:
- Run tests automatically (via prepublishOnly hook)
- Publish to npm with the new version

## Workflow Summary

1. **Develop**: Make changes with conventional commits
2. **Release**: Run `npm run release` to bump version and update changelog
3. **Push**: `git push --follow-tags` to push commits and tags
4. **GitHub**: CI creates GitHub Release automatically
5. **npm**: Run `npm publish` to publish to npm

## Available Scripts

- `npm run release` - Automatic version bump based on commits
- `npm run release:patch` - Force patch version bump
- `npm run release:minor` - Force minor version bump
- `npm run release:major` - Force major version bump
- `npm run release:first` - First release initialization
- `npm test` - Run tests
- `npm publish` - Publish to npm

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
