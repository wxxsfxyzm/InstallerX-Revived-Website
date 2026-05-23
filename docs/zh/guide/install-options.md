---
title: 安装选项与参数 - InstallerX Revived
description: 了解 InstallerX Revived 可配置的 Android 安装标志位
---

# 安装选项与参数

安装选项是传递给 Android PackageInstaller 的高级标志位。它们可以保存在配置文件中，也可以在安装对话框的扩展菜单里临时修改。对应的 AOSP 常量主要定义在 [`PackageManager.java`][aosp-pm]，会通过 `PackageInstaller.SessionParams` 进入系统安装会话。

::: warning
安装标志位只是向系统发出请求，并不保证一定生效。Android 版本、ROM 策略、授权器，以及 InstallerX 是否为系统包管理器，都会影响最终结果。
:::

## 常用选项

* **允许测试包：** 对应 `INSTALL_ALLOW_TEST`，允许安装带 `testOnly` 标记的 APK。
* **为所有用户安装：** 对应 `INSTALL_ALL_USERS`，为设备上的所有 Android 用户安装应用。此选项会覆盖配置里的目标用户。
* **允许降级安装：** 对应 `INSTALL_ALLOW_DOWNGRADE` / `INSTALL_REQUEST_DOWNGRADE`，允许低版本覆盖高版本。Android 15+ 通常需要 Root 或系统包管理器模式。
* **授予所有请求权限：** 对应 `INSTALL_GRANT_RUNTIME_PERMISSIONS`，安装后授予应用请求的运行时权限。所有文件访问、无障碍、通知监听等特殊权限不会自动授予。
* **绕过低目标 SDK 限制：** 对应 `INSTALL_BYPASS_LOW_TARGET_SDK_BLOCK`，在 Android 阻止旧 target SDK 应用安装时允许继续安装。
* **请求更新所有权：** 对应 `INSTALL_REQUEST_UPDATE_OWNERSHIP`，请求系统将 InstallerX 作为后续更新来源。

## 存储与来源标志

* **内置存储：** 对应 `INSTALL_INTERNAL`，强制安装到内部存储。
* **外置存储：** 对应 `INSTALL_EXTERNAL`，在支持的旧 Android 版本上强制安装到外部存储。
* **来自 ADB 的安装请求：** 对应 `INSTALL_FROM_ADB`，告诉系统该请求来自 ADB。
* **禁用验证：** 对应 `INSTALL_DISABLE_VERIFICATION`，在安装过程中禁用软件包验证。它不是 InstallerX 的签名验证。

这些选项主要用于测试或特定 ROM 工作流，在现代 Android 上可能被忽略。

## 高级 PackageInstaller 标志

* **免安装应用：** 对应 `INSTALL_INSTANT_APP`，将软件包作为 Instant App 安装。
* **不要杀死安装进程：** 对应 `INSTALL_DONT_KILL_APP`，安装拆分功能时避免杀死主应用。
* **完整应用：** 对应 `INSTALL_FULL_APP`，标记正在安装完整应用，常用于 Instant App 升级。
* **激进分配：** 对应 `INSTALL_ALLOCATE_AGGRESSIVE`，告诉系统该包对系统健康或安全重要，腾空间时更积极清理可清理文件。
* **虚拟预加载：** 对应 `INSTALL_VIRTUAL_PRELOAD`，标记为虚拟预装包。
* **APEX：** 对应 `INSTALL_APEX`，标记正在安装 APEX 包。
* **启用回滚：** 对应 `INSTALL_ENABLE_ROLLBACK`，为未来回滚流程保留元数据。
* **阶段安装：** 对应 `INSTALL_STAGED`，作为 staged session 安装。
* **试运行：** 对应 `INSTALL_DRY_RUN`，在支持的 Android 版本上只验证不安装。
* **禁用允许的 APEX 更新检查：** 对应 `INSTALL_DISABLE_ALLOWED_APEX_UPDATE_CHECK`，在支持的 Android 版本上跳过 APEX 更新允许性检查。

只有在理解对应 Android PackageInstaller 行为时才建议使用这些选项。

## 分包与多 APK 默认选择

* **默认选中所有分包：** 不使用 InstallerX 的最佳分包组合，而是默认勾选全部 split APK。
* **默认选择所有 APK：** 单次安装会话包含多个 APK 时，默认勾选全部 APK。

对于 APKS、APKM 和 XAPK，默认的最佳选择通常更安全，因为它会根据当前设备匹配语言、屏幕密度、ABI 和功能分包。

## 签名与兼容性

在启用 Core Patch 或类似系统级 Hook 后，系统可能不再阻止签名不匹配或签名无法验证的安装。这样一来，使用 InstallerX 的应用商店可能会静默替换不同签名的应用，例如把开发版自动更新成正式版。

为避免这种风险，每个配置文件都可以单独设置两项策略门禁：

* **允许签名不匹配：** 允许新 APK 与当前已安装版本签名不同的更新。
* **允许未知签名：** 允许 InstallerX 无法验证签名的安装。

这两个选项默认关闭。关闭时，如果命中对应风险，InstallerX 会在安装到达系统前阻止本次请求，并显示配置阻止类错误；调用方会收到标准安装失败结果。失败对话框中的建议按钮可以临时允许本次安装一次。

::: warning
这些选项主要用于已经使用 Core Patch 等框架能力的特殊环境。没有 Core Patch 时，系统通常已经会拒绝这类安装；开启后则等于允许绕过正常更新安全检查，请只对可信来源配置。
:::

[aosp-pm]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageManager.java
