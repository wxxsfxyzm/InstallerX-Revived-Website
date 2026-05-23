---
title: Quick Start Guide - InstallerX Revived
description: Get started with InstallerX Revived in minutes. Learn basic setup and core features
---

# Getting Started

Welcome to InstallerX Revived! The app is designed to be "out-of-the-box," meaning complex setup is optional. However, understanding the core concepts of **Home**, **Profiles**, and **Settings** will help you unlock its full potential.

## 1. First Run & Permissions

Upon your first installation, you generally do not need to configure complex settings immediately. When you first attempt to install an app, InstallerX will request the necessary permissions:

* **Notifications:** Required to show installation progress, results, and foreground service notifications.
* **All Files Access:** Required to read/delete APK/APKS/XAPK/APKM files from your storage.
* **Authorization (Shizuku/Root):** Required to perform silent or privileged installations.

## 2. Core Concepts: Home, Profiles, and Settings

InstallerX Revived organizes its main features into three tabs:

* **Home:** Shows the default installer status, authorization/privilege status, profile count, and device information. Default installer actions also open from the Home status card.
* **Profiles:** Controls how install or uninstall requests are handled (e.g., installation mode, install flags, user selection, authorizer overrides).
* **Settings:** Controls global app preferences (e.g., UI themes, dialog and notification behavior, blacklist, laboratory).

### Authorization Method
By default, the app uses **Shizuku**. The authorization page on Home detects every authorization method available to InstallerX and lets you choose the global authorizer used across the whole app for privileged operations, such as locking the installer, installing apps, flashing modules, deleting files, or changing system-level settings. This page only checks availability; any required permission is requested when the app actually needs it. The features shown in the app can also change based on your selection.

Available authorizers include:
* **Root:** Can perform every privileged operation with the highest permission level, but performance is usually slightly worse because it has to cold-start `app_process`.
* **Shizuku:** Provides shell or root-level privileges depending on how Shizuku was started. Recommended for most users and usually faster than direct root access.
* **Dhizuku:** Device owner mode backed by DevicePolicyManager APIs. It can handle some operations such as locking the installer and installing apps, but is limited for other privileged tasks.
* **None:** Fully limited by the system. When InstallerX runs as the system installer, it can perform silent app installs; as a normal user app, it uses the system install session and the system package manager asks the user to confirm.

::: tip Profile Override
The authorizer selected on Home controls how InstallerX itself performs privileged operations. The authorizer inside a profile is used for app installation, follows the Home authorizer by default, and can be overridden per profile (e.g., global is Shizuku, but a specific profile uses Root).
:::

## 3. Profiles & Scopes (Per-Source Configuration)

The most powerful feature of InstallerX is the ability to apply different installation rules based on the **Source App** (the app requesting the installation).

* **Default Profile:** The first profile in the list. It applies globally to any app that doesn't have a specific rule.
* **Creating a New Profile:** Click the **+ New** button to create a custom profile.
* **Setting the Scope:** To link a profile to specific apps (e.g., you want *Coolapk* to install silently, but *File Manager* to show a dialog):
    1.  Create or edit a Profile.
    2.  Tap on **Scope**.
    3.  Select the apps you want to apply this profile to.
    4.  Save. When these apps request an installation, this profile will automatically apply.

## 4. Installation Modes

You can choose how the installation is presented in the Profile settings:

* **Dialog:** Shows a popup dialog with package details. You must click "Install" to proceed.
* **Dialog (Auto):** Shows the dialog but automatically install the given package.
* **Notification:** Installs in the background with a standard progress bar. You must click "Install" to proceed。
* **Notification (Auto):** Same as above, but minimizes interaction without user actions.

### Android 16 Live Activities
For devices running **Android 16+**, InstallerX Revived supports the new **Live Activity** notification API for real-time progress updates on the lock screen and status bar.

**Supported Systems:**
* ColorOS 16+
* OneUI 8.0+
* AOSP 16 QPR1+
* Pixel 16 QPR1+
* HyperOS 3.0.300+

## 5. Locking the Installer

Due to strict restrictions and heavy modifications in Chinese custom ROMs, the built-in "Lock as Default Installer" feature may fail on some devices.

* **Compatibility Warning:** This feature may fail on **some HyperOS versions**, and **all ColorOS and OriginOS versions**.
* **Solution:** In these cases, please use the LSPosed module **[InxLocker](https://github.com/Chimioo/InxLocker)** to lock the installer for the best experience.

## 6. Tips & Device-Specific Notes

::: info Xiaomi / HyperOS Users
**1. Installer Masquerading:**
Installing system apps on HyperOS requires the "Installer" itself to be recognized as a system app. By default, InstallerX Revived automatically sets the **Installer Package Name** to `com.android.shell` to ensure compatibility. You do not need to change this unless you are an advanced user.

**2. Auto Lock:**
If you find that the installer lock reverts to the system default after an installation, tap the default installer status card on the home page and enable **"Auto Lock Installer"** on the **Default Installer** page.
:::

::: tip Global / Non-CN Users
If you are outside of mainland China and experience slow installation speeds (often due to Play Protect scanning), you can try enabling **"Disable System Install Verification"** in Settings.
:::
