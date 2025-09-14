# BHub - Build and Deployment

This document describes the automated build process for the BHub Android application.

## GitHub Actions Workflow

The repository includes a GitHub Actions workflow (`.github/workflows/build-apk.yml`) that automatically builds the Android application and uploads APK artifacts.

### Workflow Triggers

The build workflow runs on:
- Push to `main` or `develop` branches
- Pull requests targeting the `main` branch  
- Manual trigger via GitHub Actions UI

### Build Process

The workflow performs the following steps:

1. **Environment Setup**
   - Uses Ubuntu latest runner
   - Sets up Java 17 (required by Gradle 9.0.0)
   - Caches Gradle dependencies for faster builds

2. **Version Information**
   - Extracts version name and code from `gradle/libs.versions.toml`
   - Generates build timestamp for artifact naming

3. **APK Compilation**
   - Builds debug APK (`assembleDebug`)
   - Builds release APK (`assembleRelease`)
   - Uses debug signing for CI builds (when keystore.properties is not available)

4. **Artifact Upload**
   - Uploads debug APK with version info in filename
   - Uploads release APK with version info in filename
   - Uploads build metadata and logs
   - Artifacts are retained for 30 days (7 days for logs)

### Signing Configuration

The application is configured to handle CI/CD environments gracefully:

- **Production builds**: Uses keystore.properties if available
- **CI builds**: Falls back to debug signing when keystore.properties is missing
- Both debug and release variants are generated with appropriate signing

### Downloading APKs

After a successful build:

1. Go to the GitHub Actions tab in the repository
2. Click on the completed workflow run
3. Scroll down to the "Artifacts" section
4. Download the desired APK (debug or release)

The artifact names include version information and build timestamp for easy identification:
- `BHub-v1.1-debug-YYYYMMDD-HHMMSS`
- `BHub-v1.1-release-YYYYMMDD-HHMMSS`

### Build Summary

Each workflow run provides a summary with:
- Version information
- Build timestamp
- Commit hash
- APK file sizes
- Build status indicators