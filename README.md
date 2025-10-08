# Kilax App Updates

This repository contains the version configuration and release files for the Kilax mobile application.

## Purpose

This repository serves as the central location for:
- **Version Configuration** - JSON file that controls app updates
- **APK Releases** - Downloadable Android application packages
- **Release Notes** - Documentation of changes in each version

## Configuration File

The `kilax_app_config.json` file controls the app's update behavior:

```json
{
  "latest_version": "1.0.0",
  "latest_build_number": 2,
  "force_update": false,
  "update_message": "A new version is available!",
  "download_url": "https://github.com/Jassim-hub/Kilax-app-updates/releases/...",
  ...
}
```

### Key Configuration Options

#### Version Control
- **`latest_version`** - Semantic version string (e.g., "1.0.0")
- **`latest_build_number`** - Integer build number that must increment with each release
- **`minimum_supported_version`** - Oldest version still supported
- **`minimum_supported_build`** - Oldest build number still supported

#### Update Behavior
- **`force_update`** - Set to `true` to block app usage until update is installed
- **`update_message`** - Custom message shown to users about the update
- **`download_url`** - Direct link to the APK file (GitHub release or other CDN)
- **`update_check_interval_hours`** - How often to check for updates (default: 24)

#### App Configuration
- **`app_variant`** - Configure app features and branding
  - `type` - App type (e.g., "streaming")
  - `app_name` - Display name
  - `primary_color` - Brand color hex code
  - `enable_movies` - Toggle movies feature
  - `enable_series` - Toggle series feature
  - `enable_downloads` - Toggle download feature
  - `enable_premium_features` - Toggle premium features

#### Maintenance Mode
- **`maintenance_mode`** - Control app availability during maintenance
  - `enabled` - Set to `true` to enable maintenance mode
  - `message` - Message displayed to users
  - `scheduled_end_time` - ISO 8601 timestamp for expected completion

#### Feature Flags
- **`feature_flags`** - Enable/disable features remotely
  - `enable_chromecast` - Chromecast support
  - `enable_social_sharing` - Share functionality
  - `enable_reviews` - User reviews
  - `enable_watchlist` - Watchlist feature
  - `enable_continue_watching` - Continue watching feature

## How to Release a New Version

### 1. Build the Release APK

```bash
cd /path/to/kilax
flutter build apk --release
```

### 2. Create a GitHub Release

1. Go to [Releases](https://github.com/Jassim-hub/Kilax-app-updates/releases)
2. Click "Create a new release"
3. Tag version: `v1.0.1` (increment appropriately)
4. Release title: `Kilax v1.0.1`
5. Description: Add release notes
6. Upload the APK: `build/app/outputs/flutter-apk/app-release.apk`
7. Rename to: `kilax-v1.0.1.apk`
8. Publish release

### 3. Update Configuration

Edit `kilax_app_config.json`:

```json
{
  "latest_version": "1.0.1",
  "latest_build_number": 3,  // Increment this
  "force_update": false,      // Set to true if critical
  "update_message": "What's new in this version...",
  "download_url": "https://github.com/Jassim-hub/Kilax-app-updates/releases/download/v1.0.1/kilax-v1.0.1.apk",
  "release_notes": [
    "New feature X",
    "Bug fix Y",
    "Performance improvement Z"
  ],
  ...
}
```

### 4. Commit and Push

```bash
git add kilax_app_config.json
git commit -m "Release v1.0.1"
git push origin main
```

### 5. Verify

Within 24 hours (or immediately on app restart), users will see the update notification.

## Force Update Workflow

When you need to push a critical update that all users must install:

1. Update `kilax_app_config.json`:
```json
{
  "force_update": true,
  "latest_build_number": 4,  // Must be higher than all previous builds
  "update_message": "Critical security update required. Please update now.",
  ...
}
```

2. Commit and push immediately
3. Users will be blocked from using the app until they update

## Shorebird Code Push

For code-only changes that don't require full APK download:

### Create a Release
```bash
shorebird release android
```

### Create a Patch
```bash
shorebird patch android
```

Patches are automatically detected and applied without updating this repository.

## Testing Updates

### Test Force Update
1. Set your current app's build number to a lower value (e.g., 1)
2. Set `force_update: true` and `latest_build_number: 2` in config
3. Launch app - should show force update dialog

### Test Optional Update
1. Set `force_update: false`
2. Set higher build number in config
3. Launch app - should show skippable notification

### Test Shorebird Patch
1. Make code changes
2. Run `shorebird patch android`
3. Launch app - should download and prompt to restart

## Rollback Procedure

If an update causes issues:

### Immediate Rollback
1. Revert `kilax_app_config.json` to previous version:
```bash
git revert HEAD
git push origin main
```

2. Users will stop receiving the problematic update immediately

### APK Rollback
If users already installed the bad APK:
1. Create a hotfix release with incremented build number
2. Update config to point to the hotfix
3. Set `force_update: true` to ensure fast adoption

## Monitoring

Monitor update adoption:
- Check download counts in GitHub Releases
- Monitor app analytics for version distribution
- Watch for crash reports tied to specific versions

## Security Notes

- Never commit sensitive API keys or credentials to this repository
- Always test updates thoroughly before releasing
- Keep download URLs accessible and stable
- Use HTTPS for all download URLs
- Consider CDN for large APK files to reduce GitHub bandwidth

## Support

For issues or questions:
- Check app logs for update errors
- Verify JSON syntax is valid
- Ensure download URLs are accessible
- Test on multiple Android versions

## File Structure

```
Kilax-app-updates/
├── README.md                  # This file
├── kilax_app_config.json     # Version configuration
└── releases/                  # Historical releases (optional)
    ├── v1.0.0/
    ├── v1.0.1/
    └── ...
```

## Version History

| Version | Build | Date | Type | Notes |
|---------|-------|------|------|-------|
| 1.0.0 | 2 | 2025-10-08 | Initial | First production release |

---

**Repository Owner**: [@Jassim-hub](https://github.com/Jassim-hub)  
**Main App Repository**: [nanamoviehub-3](https://github.com/Jassim-hub/nanamoviehub-3)  
**License**: All Rights Reserved
