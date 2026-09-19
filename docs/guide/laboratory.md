---
title: Laboratory - Experimental Features in InstallerX Revived
description: Explore experimental features and advanced options in InstallerX Laboratory
---

# Laboratory

Laboratory only contains options that are still experimental or high risk in the current app. These features may change, be removed, or behave differently across ROMs. Enable only the parts you actually need.

## Module Flashing

Enable **Module Flashing** to let InstallerX install supported Magisk, KernelSU, or APatch modules from ZIP files.

After enabling it, InstallerX can open a ZIP as either an app container or a module when both are detected. Module installation is the main area where InstallerX may use module-specific command execution instead of pure PackageInstaller APIs.

::: danger
Always verify the module before flashing. A bad module can bootloop or break system components.
:::

## Root Implementation and ASCII Art

When module flashing is enabled, select the root implementation that matches your device environment:

* **Magisk**
* **KernelSU**
* **APatch**

InstallerX can detect installed Magisk, KernelSU, and APatch modules and compare versions when installing a new module.

InstallerX can also show an InstallerX ASCII art banner when module installation starts. The option to always use root for privileged tasks while running as a system installer has moved to **Settings -> Installer Settings -> Authorizer Tweaks**.

## HTTP Safety

See [Network settings](./app-settings.md#http-safety) for HTTP policy controls.

## GitHub Update Channel

Choose a channel in [Network settings](./app-settings.md#github-update-channel).

## Practical Advice

Keep Laboratory settings close to defaults for daily use. Turn on one feature at a time, test with a safe APK or trusted module, and turn it off again if your ROM starts routing installs through the stock installer, install sessions fail before confirmation, or network downloads are blocked unexpectedly.

## Smart Authorization

Disabled by default. When the profile authorizer is unavailable, try enabled fallback authorizers in order. Select and drag candidates to reorder them, keeping at least one enabled. None is offered only where system session installation is supported.
