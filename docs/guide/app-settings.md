---
title: App Settings - InstallerX Revived
description: Configure app settings in InstallerX Revived for optimal installation experience
---

# App Settings

App settings provide you with a wealth of customization options, allowing you to tailor the app to your usage habits and preferences.

## Theme

Customize the look and feel of the app.

### UI Engine
InstallerX Revived offers two UI engines for you to choose from:
*   **Google Material 3**: A modern Android UI based on Material 3 Expressive.
*   **Miuix**: A HyperOS-like UI style with optional custom colors where supported.

### Dynamic Theming
The app supports dynamic theming and manual theme colors. You can also enable blur effects, a floating bottom bar, and predictive back animations.

You can further customize color behaviors:
*   **Colorful Dialog**: When enabled, the installation dialog will extract colors from the icon of the app being installed, making each installation interface unique.
*   **Colorful Live Notification Progress**: When live notifications are enabled, the progress indicator can also follow the package icon color.

### Icon Preference
You can choose the source for the icons displayed in the installation dialog:
*   **Parse from Package**: Directly parses and displays the original icon from the APK file.
*   **Use System Icon Pack**: If you have a third-party icon pack installed, you can select this option to make the icons in the app list consistent with your system icons. This is tested working on HyperOS and OneUI.

### Hide App Icon
You can hide InstallerX's launcher icon and open settings with the dial code `*#*#46789#*#*`. You can also add InstallerX's Quick Settings tile and open settings directly from the system quick toggles.

If InstallerX was installed through a module, you can also launch settings from the KernelSU action entry. Confirm that you can access settings before hiding the icon. On Android 10+, some systems may still show a placeholder icon that opens the app details page.

---

## Basic Settings

The Settings tab also contains basic system-level toggles:

* **Disable ADB Verify:** Disable install scanning on some systems, such as Play Protect checks triggered during installation. This is unavailable with Dhizuku or no-privilege mode.
* **Ignore Battery Optimizations:** Helps on ROMs with strict background policies when notification installs appear stuck.

## Installer Global Settings

Installer settings collect global behavior for the installation flow. The current groups are:

* **Dialog Settings:** Control the extended menu, smart suggestions, whether dismissing the dialog cancels installation, automatic background install, long-press background install, and tapping the app icon to share the package. In the Miuix UI, this page also controls whether the install file path and install initiator are shown.
* **Notification Settings:** Choose Standard Notification, Live Activity, or Xiaomi Super Island for install progress. Super Island options include bypass restriction, XMSF blocking interval, and outer glow. Notification preferences also include showing the dialog when tapping a notification and auto-clearing success notifications.
* **Authorizer Tweaks:** Fine-tune authorizer-specific behavior. In system installer mode, InstallerX can always use root for privileged tasks such as module flashing or file deletion. In user app mode, it can try installing without user action and adjust the close-session countdown.
* **Biometric Authentication:** Disable, always require, or follow profile settings when device authentication is available.
* **Install Requester:** When enabled, profiles expose OriginatingUid/requester customization.

## Xposed Detection

InstallerX can detect Xposed modules and show related module information. When **Quick Open LSPosed** is enabled, the install result screen can offer a shortcut to open LSPosed after installing a detected module.

## OEM-Specific Information

On supported OPPO / OnePlus devices, InstallerX can show OEM-specific metadata in the install dialog to help explain restrictions or special prompts used by the stock installer.

### Preset Installation Sources
You can preset some frequently used installation sources (app package names). This allows for quick selection when creating a profile for an app or in the installation dialog, simplifying the workflow.

### Blacklist
The blacklist can be used to block the installation of specific applications. You can add rules based on the app's **package name** or **shared UID**. Any application matching a blacklist rule will be intercepted and cannot be installed.

Shared UID blacklist rules can include exempted packages. If Smart Suggestions are enabled, some blacklist hits can be allowed once for the current install session.

### Default Installer
Default installer actions have moved to the home page status card. Tap the default installer status card on the home page to open the **Default Installer** page, where you can lock or clear InstallerX's default installer role and enable **Auto Lock Installer**. If you use an LSPosed module to force installer routing, enable the corresponding switch so the home page shows InstallerX as active.

---

## Uninstaller Settings

Configure options related to app uninstallation.

### Authorizer Note
The uninstaller always uses the authorizer from the default profile.

### Uninstall Options
*   **Keep App Data**: Uninstalls the app but keeps its data and cache files.
*   **Uninstall For All Users**: Removes the app for every Android user on the device.
*   **Delete System App**: Allows uninstalling a system app for the current user where the ROM permits it.
*   **Biometric Authentication**: Require device authentication before uninstalling.

### Non-Root Shortcut
You can manually call InstallerX's uninstaller by entering a target package name. This is useful when the system does not route uninstall intents to InstallerX directly.
