---
title: Recent Updates - InstallerX Revived
description: Major improvements in InstallerX Revived from 26.05 to 26.09
---

# Recent Updates

Explore the major improvements in InstallerX Revived from 26.05 to 26.09. See the linked guides for details on using each feature.

::: tip Simplified installation
The online and offline packages are now unified into a single APK.
Release filenames still include `online` for compatibility with in-app updates.
:::

- **Network controls:** use **Settings → Network settings → Allow Internet access** to control update checks and network downloads.
- **Network source modes:** Full download is the compatibility-first default; Smart and Low storage can stream a single APK when the server supports the required HTTP Range and strong ETag behavior. Streaming has limited signature analysis.
- **Signature controls:** configure app signature analysis, optionally analyze split packages, view signature details, and set per-profile blocks for mismatched or unknown signatures. On supported system SDKs, InstallerX can also detect Android 17 v3.2 and ML-DSA signatures.
- **Profiles and authorization:** unknown scopes (designed for certain OPPO and vivo system behaviors), Smart authorization fallback order, and optional auto-approval for session installs are available with explicit permission and safety limits.
- **Settings and history:** You can turn operation history on or off, choose whether to keep or delete existing records when turning it off, and control history indicators separately. When enabled, history keeps up to 100 recent records. You can also back up and restore your profiles, scopes, app settings, and history. A restore replaces the current settings after validation.
- **System integration:** automatic installer locking was removed due to a fix to the installer-lock implementation.
- **Installed module detection:** InstallerX can detect installed Magisk, KernelSU, and APatch modules and compare their versions while installing a new one.

See [Installation](./installation.md), [App Settings](./app-settings.md), [Profiles](./profiles.md), [Laboratory](./laboratory.md), and [System Integration](./system-integration.md).
