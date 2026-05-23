---
title: 异常处理 - InstallerX Revived
description: 排查 InstallerX Revived 安装、卸载、授权和模块刷写错误
---

# 常见异常处理

本页说明 InstallerX 常见错误及建议处理方式。反馈问题时，请提供设备型号、Android 与 ROM 版本、InstallerX 版本、授权器类型、操作步骤，以及 logcat 或 LogFox 日志。

AOSP 参考：安装失败码主要来自 [`PackageManager.java`][aosp-pm] 中的 `INSTALL_FAILED_*` 常量；通过 `PackageInstaller` 会话返回的现代状态可参考 [`PackageInstaller.java`][aosp-pi]；卸载失败码主要来自 [`PackageManager.java`][aosp-pm] 中的 `DELETE_FAILED_*` 常量。

## 解析与分析错误

* **No Content Provider** 或 **Permission Denial：** 隐藏应用列表、HMA 或类似隐私模块阻止了 Provider 访问。请将 InstallerX 加入白名单。
* **压缩包损坏：** ZIP、APKS、APKM 或 XAPK 无法读取。请重新下载。
* **所有文件均不受支持：** 选择的文件里没有可安装目标。
* **无效 APK** 或 **证书缺失：** 安装包损坏或不完整，请从可信来源重新下载。
* **缺少必需分包：** 缺少应用必须的 split APK。请使用完整 APKS/APKM/XAPK，或重新选择正确分包组合。

## 安装失败

* **存储空间不足：** 对应 `INSTALL_FAILED_INSUFFICIENT_STORAGE`。清理设备空间后重试。
* **设备上已存在相同包名：** 对应 `INSTALL_FAILED_ALREADY_EXISTS` 或 `INSTALL_FAILED_DUPLICATE_PACKAGE`。当前设备已有同包名应用。请走更新流程，或先卸载旧应用。
* **更新失败 / SharedUID 签名不一致：** 对应 `INSTALL_FAILED_UPDATE_INCOMPATIBLE` / `INSTALL_FAILED_SHARED_USER_INCOMPATIBLE`。签名或共享 UID 要求不匹配。可能需要先卸载，但会有数据丢失风险。
* **应用降级失败：** 对应 `INSTALL_FAILED_VERSION_DOWNGRADE`。在授权器和系统版本支持时开启“允许降级安装”。部分 Android 14/15 OEM 系统可通过智能建议尝试降级安装路径。
* **测试包：** 对应 `INSTALL_FAILED_TEST_ONLY`。开启“允许测试包”，或使用智能建议临时重试。
* **CPU 架构不受支持：** 对应 `INSTALL_FAILED_CPU_ABI_INCOMPATIBLE` / `INSTALL_FAILED_NO_MATCHING_ABIS`。下载匹配当前 CPU 的安装包。部分 arm64 或 x86_64 设备只有在 ROM 提供运行时转译时，才能运行 32 位或异构架构包。
* **targetSDK 过低：** 对应 `INSTALL_FAILED_DEPRECATED_SDK_VERSION`。信任应用时可开启“绕过低目标 SDK 限制”。
* **系统限制 USB 安装：** 对应 `INSTALL_FAILED_USER_RESTRICTED`。前往开发者选项启用相关 USB 安装权限后重试。
* **缺少未知来源安装权限：** InstallerX 内部错误。无特权模式下，请授予 InstallerX 安装未知应用权限。
* **黑名单 / 配置阻止：** InstallerX 内部策略错误，不是 AOSP 失败码。修改黑名单或配置文件。智能建议可为当前会话临时允许一次。

## OEM 特定错误

* **HyperOS 安装系统 app 需要声明有效安装者：** 使用 Shizuku 或 Root，并声明一个有效系统安装者包名。
* **vivo 安装系统 app 需要声明有效安装者：** OriginOS / Vivo 可能要求声明应用商店等有效安装来源。
* **系统禁止跨区安装：** ROM 根据构建类型或地区阻止该包。
* **请登录设备原有的谷歌账号：** 设备处于 FRP 限制状态，需要完成账号验证。

## 卸载失败

* **设备政策 / 所有者禁用：** 对应 `DELETE_FAILED_DEVICE_POLICY_MANAGER` / `DELETE_FAILED_OWNER_BLOCKED`。设备所有者、工作资料或策略管理器禁止卸载。
* **用户受限：** 对应 `DELETE_FAILED_USER_RESTRICTED`。当前用户没有卸载该包的权限。
* **HyperOS 不允许在此处卸载系统应用：** HyperOS 阻止了当前系统应用卸载路径。
* **包管理服务内部错误：** 对应 `DELETE_FAILED_INTERNAL_ERROR`。重启后重试，或换授权器并附带日志反馈。

## 授权器错误

* **Shizuku 不可用：** 启动 Shizuku 或 Sui，并授予 InstallerX 权限。
* **Dhizuku 不可用：** 更新并重启 Dhizuku。Dhizuku 权限较有限，不支持多项高级安装能力。
* **Root 不可用：** 在 Magisk、KernelSU、APatch 或 Root 管理器中授予权限。
* **授权器启动失败：** 特权辅助进程启动失败。重启 InstallerX 或授权服务后重试。

## 模块、网络与 ZIP 错误

* **模块安装失败：** 检查模块 ZIP 和 Root 实现方式，并查看退出码和模块安装输出。
* **不兼容的授权器：** 模块刷写需要受支持的特权路径。
* **当前配置不允许明文 HTTP：** HTTP 安全策略阻止了明文链接。请使用 HTTPS，或在实验室中调整 HTTP 策略。
* **仅允许本地网络使用明文 HTTP：** 当前策略只允许本地或局域网明文 HTTP。
* **无法打开 ZIP 文件：** 压缩包不可读或格式异常，请重新下载或用其他工具验证。

## 智能建议

开启智能建议后，InstallerX 可以为测试包、降级、低 target SDK、HyperOS 安装者声明、黑名单和配置阻止等常见失败提供一次性重试方案。这些修改只对当前安装会话生效，除非你之后保存到配置文件。

[aosp-pm]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageManager.java
[aosp-pi]: https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/content/pm/PackageInstaller.java
