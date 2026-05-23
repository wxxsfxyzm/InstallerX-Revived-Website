---
title: Install Options & Parameters - InstallerX Revived
description: Customize installation parameters and options for advanced control over app installation
---

# Install Options & Parameters

Install options are advanced flags passed to Android PackageInstaller. They can be saved in a profile or changed temporarily from the install dialog extended menu. The matching AOSP constants are mostly defined in [`PackageManager.java`][aosp-pm] and are applied through `PackageInstaller.SessionParams`.

::: warning
Install flags are requests to the system, not guarantees. Android version, ROM policy, authorizer, and whether InstallerX is a system package manager all affect the final result.
:::

## Common Options

* **Allow Test:** maps to `INSTALL_ALLOW_TEST` and installs APKs marked with `testOnly`.
* **Install For All Users:** maps to `INSTALL_ALL_USERS` and installs the package for every Android user on the device. This can override a profile's target user setting.
* **Allow Downgrade:** maps to `INSTALL_ALLOW_DOWNGRADE` / `INSTALL_REQUEST_DOWNGRADE` and allows installing a lower version over a newer one. Android 15+ usually requires Root or system package manager mode.
* **Grant All Requested Permissions:** maps to `INSTALL_GRANT_RUNTIME_PERMISSIONS` and grants requested runtime permissions after install. Special permissions such as All Files Access, Accessibility, and notification listener access are not granted automatically.
* **Bypass Low Target SDK Block:** maps to `INSTALL_BYPASS_LOW_TARGET_SDK_BLOCK` and allows apps targeting old SDK versions on Android versions that normally block them.
* **Request Update Ownership:** maps to `INSTALL_REQUEST_UPDATE_OWNERSHIP` and requests that Android treats this installer as the future update owner.

## Storage and Source Flags

* **Internal Storage:** maps to `INSTALL_INTERNAL` and forces installation to internal storage.
* **External Storage:** maps to `INSTALL_EXTERNAL` and forces installation to external storage where supported. This is only available on older Android versions.
* **Install request from ADB:** maps to `INSTALL_FROM_ADB` and tells Android the install request came from ADB.
* **Disable Verification:** maps to `INSTALL_DISABLE_VERIFICATION` and disables package verification during install. This is not InstallerX signature verification.

These flags are mostly useful for testing or ROM-specific workflows. They may be ignored by modern Android builds.

## Advanced PackageInstaller Flags

* **Instant App:** maps to `INSTALL_INSTANT_APP` and installs the package as an instant app.
* **Don't Kill App:** maps to `INSTALL_DONT_KILL_APP` and avoids killing the main app when installing split features.
* **Full App:** maps to `INSTALL_FULL_APP` and marks the package as a full app, often used when upgrading an instant app.
* **Allocate Aggressive:** maps to `INSTALL_ALLOCATE_AGGRESSIVE` and tells the system the package is important to system health or security when reclaiming storage.
* **Virtual Preload:** maps to `INSTALL_VIRTUAL_PRELOAD` and marks the package as a virtual preload.
* **APEX:** maps to `INSTALL_APEX` and marks the package as an APEX package.
* **Enable Rollback:** maps to `INSTALL_ENABLE_ROLLBACK` and allows rollback metadata for future rollback flows.
* **Staged:** maps to `INSTALL_STAGED` and installs as a staged session.
* **Dry Run:** maps to `INSTALL_DRY_RUN` and verifies without installing on supported Android versions.
* **Disable Allowed APEX Update Check:** maps to `INSTALL_DISABLE_ALLOWED_APEX_UPDATE_CHECK` and skips allowed APEX update checks on supported Android versions.

Only use these when you understand the Android PackageInstaller behavior behind them.

## Split and Multi-APK Defaults

* **Select all splits by default:** select every split APK instead of InstallerX's best-fit split set.
* **Select all APKs by default during batch install:** select every APK when a single install session contains multiple APK files.

For APKS, APKM, and XAPK files, the default best-fit behavior is usually safer because it matches language, density, ABI, and feature splits to the current device.

## Signature and Compatibility

When Core Patch or similar system-level hooks are enabled, Android may no longer block installs with mismatched or unverifiable signatures. That means an app store using InstallerX could silently replace an app with a differently signed build, such as auto-updating a development build to an official release.

To prevent that, each profile has two policy gates:

* **Allow Signature Mismatch:** allow updates where the new APK's signature differs from the currently installed version.
* **Allow Unknown Signature:** allow installs where InstallerX cannot verify the signature.

Both options are off by default. When disabled and the matching risk is detected, InstallerX blocks the request before it reaches the system, shows a profile-blocked install failure, and reports a standard install failure to the calling app. The failure dialog can show a suggestion chip for a one-time bypass.

::: warning
These options are mainly for special environments using Core Patch or similar framework-level changes. Without Core Patch, the system usually rejects these installs already. Enabling them allows bypassing normal update safety checks, so use them only for trusted sources.
:::

[aosp-pm]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageManager.java
