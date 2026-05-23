---
title: Profiles & Scopes - InstallerX Revived
description: Learn how to use Profiles and Scopes in InstallerX Revived for advanced installation control
---

# Profiles & Scopes

Profiles decide how InstallerX handles an install request. You can use the global default profile directly, or scope additional profiles to one or more source apps so different installers, file managers, browsers, or app stores get different behavior.

## Scope

The first profile is the default profile. It applies whenever no scoped profile matches the app that started the install request. Uninstall operations also always use the authorizer and related uninstall settings from the default profile.

Create additional profiles when you want per-source rules. For example, you can let an app store install in the background while keeping file manager installs in a confirmation dialog.

## Authorizer

The authorizer is the privilege backend used to install apps and perform privileged operations.

* **Follow global:** Use the authorizer selected for the app on the home page.
* **Root:** Use the highest permission level for matching installs and follow-up privileged tasks, with slightly worse performance than Shizuku in most cases.
* **Shizuku:** Provides shell or root-level privileges depending on how Shizuku was started, and is usually faster than direct root access.
* **Dhizuku:** Limited by DevicePolicyManager APIs. Useful on some devices, but it cannot set installer package names, package sources, target users, or DexOpt.
* **None:** Fully limited by the system. In system installer mode it can install silently; as a normal user app it uses a system install session and relies on user confirmation.
* **Custom:** Run a custom command for advanced environments.

::: tip System package manager mode
When InstallerX is installed as a system package manager, many profile options are handled by the system and may not behave like Shizuku or Root mode.
:::

## Install Mode

Profiles can choose how an install is presented:

* **Dialog:** Show package details and wait for you to press Install.
* **Dialog Auto:** Show the dialog, then start installation automatically.
* **Notification:** Run in the background with a progress notification and manual confirmation.
* **Notification Auto:** Run from notification with less interaction.
* **Ignore:** Block install requests matched by this profile.

Profiles can configure Toast behavior. It can be disabled, always enabled, or limited to cases where the dialog is not visible.

## Installer Settings

These settings change metadata and follow-up behavior for matching installs.

* **Install reason:** Tell Android why the package is being installed. The User reason may make launchers create a home-screen icon for new apps.
* **Package source:** Mark the package as coming from a store, local file, downloaded file, other source, or unspecified source. Android may apply extra restrictions to local or downloaded files.
* **Install requester:** Optionally override the originating package. The package must already exist on the device.
* **Installer package:** Use InstallerX itself, the initiating app, or a custom package name as the declared installer.
* **Target user:** Install for a specific Android user instead of the current user. This is not supported by Dhizuku.
* **Manual DexOpt:** Trigger dex2oat after installation. Dhizuku is not supported.
* **Auto delete:** Delete install files after a successful install. ZIP containers can also be deleted when the ZIP deletion option is enabled.
* **Display details:** Show target/min SDK, package size, and module information. File path and install initiator visibility are controlled under **Settings -> Installer Settings -> Dialog Settings**.

::: info Xiaomi and HyperOS
Some system app updates require the declared installer to be a system app. InstallerX adds a HyperOS-friendly default, but advanced users can change it to another valid system package such as a file manager or app store.
:::

## Install Options

Profiles can set install flags that are sent to Android PackageInstaller. Common options include allowing test packages, installing for all users, allowing downgrade, granting runtime permissions, bypassing low target SDK restrictions, and requesting update ownership.

Not every flag works on every Android version or ROM. Some options require Root, Shizuku, system package manager mode, or a ROM that has not blocked the underlying PackageInstaller flag.

## Split and Batch Packages

InstallerX supports APK, APKS, APKM, XAPK, ZIP archives that contain APKs, and multiple APKs shared at once.

By default, InstallerX tries to select the best split set for the device. Enable **Select all splits by default** when you want every split selected first, or **Select all APKs by default during batch install** when multi-APK sessions should start with all APKs checked.
