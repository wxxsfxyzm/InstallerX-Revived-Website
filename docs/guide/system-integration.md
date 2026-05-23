---
title: System Integration - InstallerX Revived
description: Integrate InstallerX Revived with your Android system for seamless app installation
---

# System Integration

InstallerX can work as a normal app, a locked default installer, or a system package manager. The available behavior depends heavily on your ROM, Android version, and authorizer.

## Default Installer

Locking InstallerX as the default package installer lets it receive most APK install intents from file managers, browsers, and other apps. In the current app structure, default installer actions are opened from the status card on the home page.

Tap the default installer status card on the home page to open the **Default Installer** page. This page includes:

* **Auto Lock Installer:** Attempt to set InstallerX as the default installer when an install starts.
* **Lock as Default Installer:** Manually run the lock action.
* **Clear Default Installer Lock:** Clear the lock, preferably with the same authorizer used to lock it.
* **LSPosed Module Enabled:** If a third-party LSPosed module already forces installer routing, enable this to disable in-app status detection.
* **InxLocker link:** The page links to the recommended locker module for ROMs that restrict direct takeover.

Some requests cannot be captured directly, especially when the source app uses an explicit component, a session install flow, or an OEM-controlled installer.

## LSPosed and InxLocker

Some ROMs prevent third-party installers from becoming the real default installer. In those cases, use an LSPosed module to intercept install intents and forward them to InstallerX.

[InxLocker](https://github.com/Chimioo/InxLocker) is the recommended companion module. If you already use an LSPosed locker, enable the LSPosed module switch in InstallerX so the home page activation status reflects that setup instead of relying only on built-in detection.

## System Package Manager Mode

Advanced users can run InstallerX as the system package manager to replace the ROM's stock package installer. This is a high-risk setup intended for users who understand system partitions, module mounts, and package-name matching.

There are three common installation paths:

* **Overwrite the APK after Core Patch:** suitable when Core Patch or a similar framework already allows replacing the system installer. If the stock package manager has a higher `versionCode` than the current InstallerX build, the system may restore the stock app after reboot; handle the version code before relying on this path.
* **Flash a module:** better for long-term use. Stable releases usually provide matching flashable module packages in the Telegram channel, and you can also fork the project and build the module with GitHub Actions.
* **Pack it into `super` or preinstall it in a ROM:** suitable for ROM maintainers or users comfortable with image packaging. Place the correctly named InstallerX package into the system image and flash or ship it with the ROM.

The package name must match the package manager used by your ROM:

* **AOSP build:** `com.android.packageinstaller`
* **Google build:** `com.google.android.packageinstaller`

General guidance:

* **China-region Xiaomi / HyperOS:** enable **Use native installer** in Cemiuiler under **System Framework -> Other**, then flash the AOSP build.
* **China-region OPPO / OnePlus / ColorOS:** the AOSP build usually matches.
* **Global ROMs / Google devices:** the Google build usually matches.

::: warning
This is only a reference for common setups, not a guarantee for every OEM. Before flashing, packaging, or preinstalling it, verify that your system package manager package name, mount path, and privileged permission files match exactly. A mismatch can cause boot issues.
:::

If you use **KernelSU**, prefer the meta-module package and disable KernelSU's default module uninstall feature so module removal behavior does not conflict with replacing the system package manager.

After you have already installed InstallerX through a system-app module once, later updates usually only require installing the APK inside the module. You do not need to flash the module and reboot for every app update.

When InstallerX runs as the system package manager, some custom metadata options, such as custom installer source, may no longer be supported.

## OEM Notes

* **HyperOS and MIUI:** system app updates may require a declared installer that is itself a system app. InstallerX can declare a compatible installer package and provide smart suggestions when this fails.
* **ColorOS and OriginOS:** default installer locking is often restricted. Use InxLocker when direct locking fails.
* **Honor:** when using Shizuku, disable the developer option that monitors ADB installs if installs hang.
* **OPPO and OnePlus:** InstallerX can show OEM-specific APK metadata on supported builds.

## Background and Notifications

Notification installs rely on foreground services. On ROMs with strict background control, set InstallerX battery usage to unrestricted if progress notifications freeze.

InstallerX cleans up background services shortly after an install finishes, so unrestricted background access is mainly a compatibility setting for strict ROMs.

## Live Activity and Mi Island

On supported Android 16+ systems, InstallerX can use native Live Activity notifications for richer install progress. Supported environments include ColorOS 16+, OneUI 8.0+, AOSP 16 QPR1+, Pixel 16 QPR1+, and HyperOS 3.0.300+.

Xiaomi Super Island is a notification style under **Settings -> Installer Settings -> Notification Settings**. Its bypass restriction, XMSF blocking interval, and outer glow options are also configured there, and should only be changed when you understand the device-specific effect.
