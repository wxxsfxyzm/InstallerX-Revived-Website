---
title: Exceptions & Error Handling - InstallerX Revived
description: Troubleshoot exceptions and errors in InstallerX Revived installation process
---

# Common Exceptions Handling

This page explains common errors shown by InstallerX and the safest next step. If you report a bug, include your device model, Android and ROM version, InstallerX version, authorizer, operation, and logs from logcat or LogFox.

AOSP reference: install failures mostly map to `INSTALL_FAILED_*` constants in [`PackageManager.java`][aosp-pm]; modern `PackageInstaller` session status values are in [`PackageInstaller.java`][aosp-pi]; uninstall failures mostly map to `DELETE_FAILED_*` constants in [`PackageManager.java`][aosp-pm].

## Resolve and Analysis Errors

* **No Content Provider** or **Permission Denial:** Hide My Applist or a similar privacy module is blocking provider access. Add InstallerX to the whitelist.
* **Corrupted archive:** the ZIP, APKS, APKM, or XAPK cannot be read. Re-download the file.
* **All files unsupported:** none of the selected files contains a supported install target.
* **Invalid APK** or **No certificates:** the package is broken or incomplete. Download from a trusted source again.
* **Missing split:** a required split APK is absent. Use the full APKS/APKM/XAPK bundle or select the correct split set.

## Install Failures

* **Insufficient storage:** maps to `INSTALL_FAILED_INSUFFICIENT_STORAGE`. Free device storage and retry.
* **Duplicate package:** maps to `INSTALL_FAILED_ALREADY_EXISTS` or `INSTALL_FAILED_DUPLICATE_PACKAGE`. An app with the same package name already exists. Use update mode or uninstall the existing app first.
* **Update incompatible** or **Shared user incompatible:** maps to `INSTALL_FAILED_UPDATE_INCOMPATIBLE` / `INSTALL_FAILED_SHARED_USER_INCOMPATIBLE`. Signatures or shared UID requirements do not match. Uninstalling first may be required, but this can delete data.
* **Version downgrade:** maps to `INSTALL_FAILED_VERSION_DOWNGRADE`. Enable Allow Downgrade if your authorizer and Android version support it. On some Android 14/15 OEM builds, Smart Suggestions may offer downgrade retry paths.
* **Test-only package:** maps to `INSTALL_FAILED_TEST_ONLY`. Enable Allow Test, or use the smart suggestion to retry once with the flag.
* **CPU ABI incompatible:** maps to `INSTALL_FAILED_CPU_ABI_INCOMPATIBLE` / `INSTALL_FAILED_NO_MATCHING_ABIS`. Download a package that matches your CPU. Some arm64 or x86_64 devices can run translated 32-bit packages only if the ROM provides a runtime translator.
* **Deprecated SDK version:** maps to `INSTALL_FAILED_DEPRECATED_SDK_VERSION`. Enable Bypass Low Target SDK Block if you trust the app.
* **User restricted:** maps to `INSTALL_FAILED_USER_RESTRICTED`. Enable USB install or related developer options, then retry.
* **Missing install permission:** InstallerX internal error. Grant unknown app install permission when using non-privileged mode.
* **Blacklisted package** or **blocked by profile:** InstallerX internal policy error, not an AOSP failure code. Edit the blacklist or profile. Smart Suggestions can allow a one-time bypass.

## OEM-Specific Errors

* **HyperOS isolation violation:** system app installation requires a valid system installer declaration. Use Shizuku or Root and declare a valid installer package.
* **OriginOS blacklist:** Vivo may require a valid app-store installer declaration for system apps.
* **Rejected by build type:** the ROM blocks this cross-region or build-type package.
* **OEM FRP locked:** finish device account verification before installing.

## Uninstall Failures

* **Device policy** or **owner blocked:** maps to `DELETE_FAILED_DEVICE_POLICY_MANAGER` / `DELETE_FAILED_OWNER_BLOCKED`. A device owner, work profile, or policy manager blocks uninstalling.
* **User restricted:** maps to `DELETE_FAILED_USER_RESTRICTED`. The current user is not allowed to uninstall this package.
* **HyperOS system app:** HyperOS blocks this system app uninstall path.
* **Internal error:** maps to `DELETE_FAILED_INTERNAL_ERROR`. Package manager failed internally. Reboot, retry with another authorizer, or capture logs.

## Authorizer Errors

* **Shizuku not working:** start Shizuku or Sui and grant InstallerX permission.
* **Dhizuku not working:** update and restart Dhizuku. Dhizuku has limited privileges and cannot perform several advanced InstallerX operations.
* **Root not working:** grant root access in Magisk, KernelSU, APatch, or your root manager.
* **App process authorizer failed:** the privileged helper failed to start. Retry after restarting InstallerX or the authorizer service.

## Module, Network, and ZIP Errors

* **Module install failed:** verify the module ZIP and selected root implementation. Check the exit code and module installer output.
* **Incompatible authorizer:** module flashing requires a supported privileged path.
* **HTTP not allowed:** the current HTTP policy blocks cleartext HTTP. Use HTTPS or change the Laboratory HTTP policy.
* **HTTP restricted for localhost:** cleartext HTTP is limited to local addresses.
* **ZIP exception:** the archive is unreadable or malformed. Re-download or extract it with another tool to verify it.

## Smart Suggestions

When enabled, Smart Suggestions can offer safe one-time retries for common failures such as test-only packages, downgrade attempts, low target SDK blocks, HyperOS installer declarations, blacklisted packages, and profile blocks. These changes apply to the current install session unless you later save them in a profile.

[aosp-pm]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageManager.java
[aosp-pi]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageInstaller.java
