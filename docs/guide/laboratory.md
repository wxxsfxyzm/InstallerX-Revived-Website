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

InstallerX can also show an InstallerX ASCII art banner when module installation starts. The option to always use root for privileged tasks while running as a system installer has moved to **Settings -> Installer Settings -> Authorizer Tweaks**.

## HTTP Safety

Online builds can install APKs from shared download links and check for updates. The HTTP security policy in Laboratory controls whether cleartext links are allowed:

* **HTTPS only:** safest default.
* **Local cleartext:** allow cleartext HTTP only for local or localhost use.
* **Allow all:** least safe; only use when you trust the source and network.

::: warning
Allowing all cleartext HTTP lowers security. Use it only when you fully trust the source and the current network.
:::

Network-related settings only appear in online builds. Offline builds do not request network permission and will report an error for online-only features.

## GitHub Update Channel

Online builds can choose the GitHub update channel:

* **Official:** access GitHub directly.
* **7ED proxy:** use the built-in proxy channel.
* **Custom GHProxy:** enter your own proxy URL.

If the custom proxy URL is empty, InstallerX falls back to the official channel.

## Practical Advice

Keep Laboratory settings close to defaults for daily use. Turn on one feature at a time, test with a safe APK or trusted module, and turn it off again if your ROM starts routing installs through the stock installer, install sessions fail before confirmation, or network downloads are blocked unexpectedly.
