
---
title: 发版说明
description: 
platform: iOS
updatedAt: Mon Jan 28 2019 11:28:00 GMT+0000 (UTC)
---
# 发版说明
## 简介
iOS 互动游戏 SDK，支持使用 Objective-C 和/或 C++ 语言在 iOS 平台上实现游戏语音和视频功能。
点击 [游戏产品概述](https://docs.agora.io/cn/Interactive%20Gaming/product_gaming?platform=All%20Platforms) 了解关键特性。

## 2.2 版
该版本基于 Agora Native SDK 和 Agora C++ SDK 开发，于 2019 年 1 月 28 日发布。新增特性详见下文。

### 新增功能

#### 1. 设置仅语音模式

新增接口 `setVoiceOnlyMode`，支持设置仅语音模式。设置后，SDK 仅传输语音流，而不传输其他流，例如，敲键盘的声音等。该功能可以有效优化语音质量。

#### 2. 设置远端音效位置

新增接口 `setRemoteVoicePosition` ，支持设置远端音效出现的位置和音效音量。

#### 3. 关闭/重新开启本地语音功能

应用程序在加入频道时，语音功能是默认打开的。为满足用户只接收而不发送音频流的需求，该版本新增 `enableLocalAudio` 接口，方便应用程序在进入频道后关闭或重新开启本地语音功能。关闭本地语音功能后，应用程序会收到 `didMicrophoneEnabled` 回调，并停止采集本地音频流。该方法不影响接收和播放远端音频流。


