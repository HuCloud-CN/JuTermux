# Termux 应用程序

[![Build status](https://github.com/termux/termux-app/workflows/Build/badge.svg)](https://github.com/termux/termux-app/actions)
[![Testing status](https://github.com/termux/termux-app/workflows/Unit%20tests/badge.svg)](https://github.com/termux/termux-app/actions)
[![Join the chat at https://gitter.im/termux/termux](https://badges.gitter.im/termux/termux.svg)](https://gitter.im/termux/termux)
[![Join the Termux discord server](https://img.shields.io/discord/641256914684084234.svg?label=&logo=discord&logoColor=ffffff&color=5865F2)](https://discord.gg/HXpF69X)
[![Termux library releases at Jitpack](https://jitpack.io/v/termux/termux-app.svg)](https://jitpack.io/#termux/termux-app)

[Termux](https://termux.com) 是一个安卓终端应用程序和Linux环境。

请注意，本存储库仅适用于应用程序本身（用户界面和终端仿真）。对于可以在应用程序内部安装的软件包，请参见 [termux/termux-packages](https://github.com/termux/termux-packages)。

关于Termux软件包管理的快速指南可在 [Package Management](https://github.com/termux/termux-packages/wiki/Package-Management) 中找到。它还提供了在运行 `apt` 或 `pkg` 命令时如何修复 **`repository is under maintenance or down`** 错误的信息。

**我们正在寻找Termux安卓应用程序的维护者。**

***

**注意：Termux在Android 12+上可能不稳定。** Android系统会杀死任何超过32个（所有应用程序的总限制）以及任何使用过多CPU的（幻影）进程。您可能会在终端中收到 `[Process completed (signal 9) - press Enter]` 消息，而实际上您自己并没有退出shell进程。请查看相关问题 [#2366](https://github.com/termux/termux-app/issues/2366)、[issue tracker](https://issuetracker.google.com/u/1/issues/205156966)、[phantom cached and empty processes docs](https://github.com/agnostic-apollo/Android-Docs/blob/master/en/docs/apps/processes/phantom-cached-and-empty-processes.md) 和 [this TLDR comment](https://github.com/termux/termux-app/issues/2366#issuecomment-1237468220)，了解如何禁用对幻影进程和过度CPU使用进程的修剪。稍后将添加适当的文档页面。在Android 12L或13中应有一个选项可以禁用此类进程的杀死功能，所以如果您处于Android 11并且没有root权限，特别是请自行承担风险升级。

***

## 内容
- [Termux应用程序和插件](#termux-应用程序和插件)
- [安装](#安装)
- [卸载](#卸载)
- [重要链接](#重要链接)
- [调试](#调试)
- [维护者和贡献者须知](#维护者和贡献者须知)
- [分支](#分支)

## Termux应用程序和插件

核心 [Termux](https://github.com/termux/termux-app) 应用程序配有以下可选插件应用程序：

- [Termux:API](https://github.com/termux/termux-api)
- [Termux:Boot](https://github.com/termux/termux-boot)
- [Termux:Float](https://github.com/termux/termux-float)
- [Termux:Styling](https://github.com/termux/termux-styling)
- [Termux:Tasker](https://github.com/termux/termux-tasker)
- [Termux:Widget](https://github.com/termux/termux-widget)

## 安装

最新版本是 `v0.118.0`。

**注意：强烈建议您尽快更新到 `v0.118.0` 或更高版本，以修复包括[这里](https://termux.github.io/general/2022/02/15/termux-apps-vulnerability-disclosures.html)报告的一个严重的世界可读漏洞。在此再次提醒[用户](https://www.reddit.com/r/termux/comments/pkujfa/important_deprecation_notice_for_google_play)已经从Google Play商店安装了Termux应用程序的用户，这些应用程序的[更新](#google-play-store-deprecated)已不再支持。建议您转移到F-Droid或GitHub发布版。**

Termux可以通过以下列出的各种渠道获取，仅支持 **Android `>= 7`** 版本，并支持所有应用程序和软件包。

对Android `5`和`6`的应用程序和软件包支持已于[2020-01-01](https://www.reddit.com/r/termux/comments/dnzdbs/end_of_android56_support_on_20200101/)在 `v0.83` 版本中停止，但仅针对应用程序的支持（没有软件包更新支持）于[2022-05-24](https://github.com/termux/termux-app/pull/2740)通过[GitHub](#github)源重新添加。详情请查看[这里](https://github.com/termux/termux-app/wiki/Termux-on-android-5-or-6)。

不同来源的APK文件使用不同的签名密钥进行签名。Termux应用程序及其所有插件使用相同的 [`sharedUserId`](https://developer.android.com/guide/topics/manifest/manifest-element) `com.termux`，因此必须从相同来源安装所有的APK文件，才能一起工作。不要试图将它们混合在一起，例如，不要尝试从 `F-Droid` 安装一个应用程序或插件，再从其他来源如 `GitHub` 安装另一个。Android Package Manager 通常也不允许安装不同签名的APK文件，您在安装时会遇到 `App not installed`、`Failed to install due to an unknown error`、`INSTALL_FAILED_UPDATE_INCOMPATIBLE`、`INSTALL_FAILED_SHARED_USER_INCOMPATIBLE`、`signatures do not match previously installed version` 等错误。此限制可以通过root或定制rom绕过。

如果您希望从不同来源安装，则必须**先卸载设备上任何和所有现有的Termux或其插件应用程序APK文件**，然后再从新的来源安装所有新的APK文件。详情请查看[卸载](#卸载)部分。在卸载之前，您可能还需要考虑[备份Termux](https://wiki.termux.com/wiki/Backing_up_Termux)，以便在从不同来源重新安装后恢复。

在以下段落中，*"bootstrap"* 指的是与 `termux-app` 本身一起分发的最小软件包，以启动一个工作shell环境。其zip文件在[这里](https://github.com/termux/termux-packages/releases)构建和发布。

### F-Droid

可以从 [这里](https://f-droid.org/en/packages/com.termux/) 获取 `Termux` 应用程序。

您**不需要**下载 `F-Droid` 应用程序（通过 `Download F-Droid` 链接）来安装Termux。您可以通过点击每个版本部分底部的 `Download APK` 链接直接从网站下载Termux APK。

通常需要几天（甚至一周或更长时间）才能在 `GitHub` 发布更新后在 `F-Droid` 上可用。`F-Droid` 发布的版本是 `F-Droid` 在检测到 [GitHub](https://gitlab.com/fdroid/fdroiddata/-/blob/master/metadata/com.termux.yml) 上有新版本发布后构建和发布的。Termux维护者**没有**对 `F-Droid` 上Termux应用程序构建和发布的控制权。此外，Termux维护者也无法访问 `F-Droid` 发布的APK签名密钥，因此我们无法在 `GitHub` 上发布与 `F-Droid` 版本兼容的APK。

`F-Droid` 应用程序可能不会通知您更新，您需要在应用程序的 `Updates` 选项卡中手动执行下拉刷新操作以检查更新。确保为该应用程序禁用电池优化，详情请查看 https://dontkillmyapp.com/。

仅发布通用APK，它将适用于所有支持的架构。APK和bootstrap安装大小将为 `~180MB`。`F-Droid` 不支持特定架构的APK。

### GitHub

可以通过 [`GitHub Releases`](https://github.com/termux/termux-app/releases) 获取 `Termux` 应用程序（版本 `>= 0.118.0`）或通过 [`GitHub Build Action`](https://github.com/termux/termux-app/actions/workflows/debug_build.yml?query=branch%3Amaster+event%3Apush) 工作流。**对于安卓 `>= 7`，只安装 `apt-android-