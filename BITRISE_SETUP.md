# Bitrise Setup Instructions

## Adding this Project to Bitrise (llm-experiments workspace)

### Option 1: Via Bitrise Web UI

1. **Log in to Bitrise** at https://app.bitrise.io
2. **Switch to the llm-experiments workspace**
3. **Click "Add new app"**
4. **Connect your repository:**
   - Choose your Git provider
   - Select this repository
   - Choose the branch to track (typically `master` or `main`)
5. **Skip the project scanner** (we have a custom bitrise.yml)
6. **Configure access:**
   - Add SSH key for repository access
   - Set up team access as needed
7. **Bitrise will detect** the `bitrise.yml` file in the repository root

### Option 2: Via Bitrise CLI

```bash
# Install Bitrise CLI if not already installed
brew install bitrise

# Validate the configuration
bitrise validate

# Run workflows locally to test
bitrise run test
bitrise run primary
```

## Workflow Overview

### `primary` workflow (runs on push to any branch)
- Clones repository
- Pulls cached dependencies
- Installs missing Android SDK tools
- Updates version code/name
- Runs Android Lint
- Runs unit tests (JUnit)
- Runs instrumented tests on virtual device (Pixel 2, API 30)
- Builds debug APK
- Deploys artifacts to Bitrise
- Pushes cache

### `test` workflow (runs on pull requests)
- Lighter workflow for faster feedback
- Runs linting and unit tests only
- Skips instrumented tests and APK building

## Environment Variables Needed

The following may need to be set in Bitrise:
- `PROJECT_LOCATION`: Path to the project (default: `.`)
- Any signing credentials if building release variants

## Next Steps

1. Push this configuration to your repository
2. Add the app to Bitrise using the steps above
3. Configure any required secrets/environment variables
4. Trigger a build to verify everything works

## Troubleshooting

- If builds fail due to Android SDK tools, check the `compileSdkVersion` matches available SDKs
- For instrumented tests, ensure virtual device configuration matches your needs
- Check Gradle wrapper permissions are correct in the repository
