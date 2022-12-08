本文介绍如何集成并使用 RTM SDK 快速构建一个简单的实时消息 app。

开始前，你需要阅读[核心概念](./cn/rtm-2.x/feature-description?platform=flutter)了解更多 RTM 产品中的概念。

## 技术原理

本节展示 app 中实现发送和接收的基本工作流程：

![](https://web-cdn.agora.io/docs-files/1670315064326)

 1. 创建并加入 Stream Channel：调用 `createStreamChannel` 创建一个 `StreamChannel` 类型实例，再调用 `join` 加入频道。
 2. 加入 Topic：调用 `joinTopic` 加入任意一个 Topic。
 3. 在 Topic 中发送和订阅消息：调用 `publishTopicMessage` 发布消息，调用 `subscribeTopic` 订阅远端用户的消息。
 4. 接收远端用户的消息：通过 `onMessageEvent` 事件通知接收远端用户的消息。

## 前提条件

你需要运行 `flutter doctor` 命令检查开发环境和运行环境是否满足要求。

### 开发环境要求

如果你的目标平台为 iOS，你的开发环境需要满足以下需求：

- Flutter 2.0 或更高版本
- Dart 2.14.0 或更高版本
- macOS 操作系统
- 最新版本的 Xcode

如果你的目标平台为 Android，你的开发环境需要满足以下需求：

- Flutter 2.0 或更高版本
- Dart 2.14.0 或更高版本
- macOS 或 Windows 操作系统
- 最新版本的 Android Studio

更多环境要求细节，详见 [Install Flutter](https://docs.flutter.dev/get-started/install)。

### 运行环境要求

- 如果你的目标平台为 iOS，你需要有一台 iOS 的设备。
- 如果你的目标平台为 Android，你需要有一台 Android 设备或模拟器。

### 其他要求

一个有效的 Agora [开发者账号](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up)。

## 获取 App ID

### 1. 创建 Agora 项目

~4c028930-19e2-11eb-b0e2-eb6c69fefbc6~

<a name="appid"></a>

### 2. 获取 Agora 项目的 App ID

~bbd6ec60-19e2-11eb-b0e2-eb6c69fefbc6~

## 创建 Flutter 项目

本文使用 Visual Studio Code 创建 Flutter 项目。你需要在 Visual Studio Code 中安装 Flutter plugin。关于详细设置可以参考 [Set up an editor](https://flutter.dev/docs/get-started/editor?tab=vscode)。

1. 打开 Visual Studio Code，在 **View > Command** 菜单选择 **Flutter: New Project** 命令，按回车键，然后输入项目名称，按回车键。在弹出的窗口中选择项目的创建位置。
2. 选择完成后，Visual Studio Code 会自动生成一个简单的示例项目。

## 添加依赖项

在 `pubspec.yaml` 文件中添加 `agora_rtc_engine` 依赖项，集成 Agora Flutter SDK。

```yaml
agora_rtc_engine:
  git:
    url: https://github.com/AgoraIO/Agora-Flutter-SDK.git
    ref: v6.0.0-sp.4010
```

## 基本流程

打开 `main.dart`，删除  `void main() => runApp(MyApp());` 语句下方的全部代码。并按照步骤增加以下代码：

<div class="alert note">如果你的第一行语句不是 <code>void main() => runApp(MyApp());</code>，你需要将其替换为 <code>void main() => runApp(MyApp());</code> 并删除下方的全部代码。</div>

### 1. 导入 package

导入以下 package：

```dart
import 'package:flutter/material.dart';
import 'dart:async';
import 'package:flutter/foundation.dart';
import 'package:agora_rtc_engine/rtc_engine.dart';
import 'package:permission_handler/permission_handler.dart';
```

### 2. 定义 App ID

输入你获得的 App ID。

```dart
const APP_ID = '<Your App ID>';
```

### 3. 定义 Flutter 应用类

定义 MyApp 应用类：

```dart
// 应用类
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

### 4. 定义应用状态类

参考如下步骤在 `class _MyAppState extends State<MyApp> {}` 中定义应用状态类：

<div class="alert note">你也可以点击<a href="#info">相关信息</a >查看 API 调用时序图和示例项目。</div>

(1) 构建 UI：你可以结合实际业务场景构建发送和接收消息的 UI。本节提供一个简单的 UI 示例。
(2) 创建 RTM 客户端实例：在调用 RTM 的任何 API 之前，你需要先调用 `createAgoraRtmClient` 创建一个 `RtmClient` 实例。
(3) 初始化 `RtmClient` 实例并监听事件：调用 `initialize` 初始化 `RtmClient`，并通过 `RtmConfig` 添加事件监听。
(4) 创建 Stream Channel：在使用 `streamChannel` 类中的任何 API 之前，你需要先调用 `createStreamChannel` 创建 Stream Channel。
(5) 加入 Stream Channel：调用 `join` 加入频道。
(6) 加入 Topic：调用 `joinTopic` 加入 Topic。成功加入 Topic 后，SDK 会自动将你注册为该 Topic 的消息发布者，你可以在该 Topic 中发送消息。
(7) 在 Topic 中发送消息：调用 `publishTopicMessage` 可以在 Topic 中发送消息。成功发送后，SDK 会把该消息分发给该 Topic 的所有订阅者。
(8) 订阅 Topic 中的发布者：调用 `subscribeTopic` 订阅一个 Topic 中的一位或多位消息发布者，在一个 Topic 中最多只能订阅 64 位消息发布者。你也可以通过 `getSubscribedUserList` 查询当前已订阅的消息发布者列表。
(9) 离开 Topic：如果你不需要在该 Topic 中发送消息，调用 `leaveTopic` 离开该 Topic。离开某个 Topic 不会影响你订阅其他 Topic 中的消息。
(10) 离开 Stream Channel：如果你不再需要发送或接收该频道中的消息，调用 `leave` 离开该频道。
(11) 销毁 Stream Channel：如果你不再需要该频道，调用 `release` 销毁对应的 `StreamChannel` 实例以释放资源。
(12) 销毁 RTM 客户端实例：如果你不再需要使用 RTM 客户端，调用 `release` 销毁对应的 `RtmClient` 实例以释放资源。

```dart

// 在 <RTC_APP_ID> 和 <RTM_APP_ID> 中填入 Agora 项目的 App ID
const String rtcAppId = '<RTC_APP_ID>';
const String rtmAppId = '<RTM_APP_ID>';
// 在 <RTM_TOKEN> 中填入你在服务端生成的 Token
const String rtmToken = '<RTM_TOKEN>';
// 在 <RTM_TOPIC> 中填入 Topic 名称
const String topic = '<RTM_TOPIC>';
// 在 <RTM_USER_ID> 中填入用户 ID
const String rtmUserId = '<RTM_USER_ID>';

class _MyAppState extends State<MyApp> {
  late final RtcEngine _engine;
  late final RtmClient _rtmClient;
  late final StreamChannel _streamChannel;

  late TextEditingController _messageController;

  @override
  void initState() {
    super.initState();

    _messageController = TextEditingController();

    _init();
  }

  @override
  void dispose() {
    _dispose();
    super.dispose();
  }

  Future<void> _dispose() async {
    _messageController.dispose();
    // 销毁 RtmClient
    await _rtmClient.release();
    // 销毁 RtcEngine
    await _engine.release();
  }

  Future<void> _init() async {
    // 创建 RtcEngine
    _engine = createAgoraRtcEngine();
    // 初始化 RtcEngine
    await _engine.initialize(const RtcEngineContext(
      appId: rtcAppId,
    ));

    await _initRtmClient();
  }

  Future<void> _joinTopic() async {
    // 加入 Topic
    await _streamChannel.joinTopic(
        topic: topic, options: const JoinTopicOptions());
    // 订阅 Topic
    await _streamChannel.subscribeTopic(
        topic: topic, options: const TopicOptions());
  }

  Future<void> _initRtmClient() async {
    // 创建 RtmClient
    _rtmClient = createAgoraRtmClient();

    final rtmConfig = RtmConfig(
      appId: rtmAppId,
      userId: rtmUserId,
      eventHandler: RtmEventHandler(
        // 监听消息事件通知
        onMessageEvent: (MessageEvent event) {
          debugPrint('[onMessageEvent] event: ${event.toJson()}');
        },
        // 监听状态事件通知
        onPresenceEvent: (PresenceEvent event) {
          debugPrint('[onPresenceEvent] event: ${event.toJson()}');
        },
        // 监听加入频道回调
        onJoinResult: (String channelName, String userId,
            StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onJoinResult] channelName: $channelName, userId: $userId, errorCode: $errorCode');
          _joinTopic();
        },
        // 监听离开频道回调
        onLeaveResult: (String channelName, String userId,
            StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onLeaveResult] channelName: $channelName, userId: $userId, errorCode: $errorCode');
        },
        // 监听加入 Topic 回调
        onJoinTopicResult: (String channelName, String userId, String topic,
            String meta, StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onJoinTopicResult] channelName: $channelName, userId: $userId, topic: $topic, meta: $meta, errorCode: $errorCode');
        },
        // 监听离开 Topic 回调
        onLeaveTopicResult: (String channelName, String userId, String topic,
            String meta, StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onLeaveTopicResult] channelName: $channelName, userId: $userId, topic: $topic, meta: $meta, errorCode: $errorCode');
        },
        // 监听订阅 Topic 或消息发布者回调
        onTopicSubscribed: (String channelName,
            String userId,
            String topic,
            UserList succeedUsers,
            UserList failedUsers,
            StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onTopicSubscribed] channelName: $channelName, userId: $userId, topic: $topic, succeedUsers: ${succeedUsers.toJson()}, failedUsers: ${failedUsers.toJson()}, errorCode: $errorCode');
        },
        // 监听取消订阅 Topic 或消息发布者回调
        onTopicUnsubscribed: (String channelName,
            String userId,
            String topic,
            UserList succeedUsers,
            UserList failedUsers,
            StreamChannelErrorCode errorCode) {
          debugPrint(
              '[onTopicUnsubscribed] channelName: $channelName, userId: $userId, topic: $topic, succeedUsers: ${succeedUsers.toJson()}, failedUsers: ${failedUsers.toJson()}, errorCode: $errorCode');
        },
        // 监听 SDK 连接状态事件通知
        onConnectionStateChange: (String channelName, RtmConnectionState state,
            RtmConnectionChangeReason reason) {
          debugPrint(
              '[onConnectionStateChange] channelName: $channelName, state: $state, reason: $reason');
        },
      ),
    );
    // 初始化 RtmClient
    await _rtmClient.initialize(rtmConfig);

    // 创建 Stream Channel，此处频道名以 My Channel 为例
    _streamChannel = await _rtmClient.createStreamChannel('My Channel');
    // 加入 Stream Channel
    await _streamChannel.join(const JoinChannelOptions(token: rtmToken));
  }

@override
Widget build(BuildContext context) {
  return MaterialApp(
    home: Scaffold(
      body: Column(
        children: [
          TextField(
            controller: _messageController,
          ),
          ElevatedButton(
            onPressed: () async {
              final message =
                  Uint8List.fromList(utf8.encode(_messageController.text));

              await _streamChannel.publishTopicMessage(
                topic: topic,
                message: message,
                length: message.length,
              );
            },
            child: const Text('Send Message'),
          ),
        ],
      ),
    ),
  );
}
}
```

## 运行项目

1. 在项目根目录运行以下命令安装依赖项。

    ```bash
    flutter packages get
    ```

2. 在 Visual Studio Code 右下角状态栏选择需要运行项目的设备。运行以下命令启动示例项目。

    ```bash
    flutter run
    ```

## 测试你的 app

按照以下步骤测试你的 app：

1. 分别在两台设备（设备 A、B）中运行你的 app。确保设备 A 和 B 使用不同的用户 ID，以及相同的 App ID、Token、频道名称、Topic 名称。
2. 在设备 A 中，输入消息并点击 **Send Message** 发送。
3. 设备 B 会收到 `onMessageEvent` 事件通知，表示成功接收消息。

## 开发注意事项

1. 一条 RTM 消息可以是字符串或者二进制数据，你需要在业务层自行区分消息负载格式。为更灵活地实现你的业务，你也可以使用 JSON 等其他方式来构建你的负载格式，此时，你需要确保转交给 RTM 的消息负载已字符串序列化。

2. <code>userId</code> 为不超过 64 位的任意字符串序列，且具有唯一性。不同用户、同一用户的不同终端设备需要通过不同的 <code>userId</code> 进行区分，所以你需要处理终端用户和 <code>userId</code> 的映射关系，确保终端用户的 <code>userId</code> 唯一且可以被复用。此外，项目中的 <code>userId</code> 数量还会影响最大连接数（PCU）的计量，从而影响计费。

3. 成功离开频道后，你在该频道中注册的所有 Topic 发布者的角色以及你在所有 Topic 中的订阅关系都将自动解除。如需恢复之前注册的发布者角色和消息的订阅关系，声网推荐你在调用 <code>leave</code> 之前自行记录相关信息，以便后续重新调用 <code>join</code>、<code>joinTopic</code> 和 <code>SubscribeTopic</code> 进行相关设置。

<a name="info"></a>

## 相关信息

### API 调用时序图

下图展示实现发送和接收消息的 API 调用时序：

![](https://web-cdn.agora.io/docs-files/1670320126847)

### API 文档

在实现发送和接收消息的过程中，你可以参考如下 API 文档：

- [概览](./cn/rtm-2.x/api-overview-flutter?platform=Flutter)
- [RtmClient](./cn/rtm-2.x/api-client-flutter?platform=Flutter)
- [StreamChannel](./cn/rtm-2.x/api-channel-flutter?platform=Flutter)
- [错误码](/cn/rtm-2.x/error-codes?platform=All%20Platforms)

### 示例项目

声网在 Github 上提供一个开源的实时消息示例项目 [rtm_chat](https://github.com/AgoraIO-Extensions/Agora-Flutter-SDK/blob/v6.0.0-sp.4010/example/lib/examples/advanced/rtm_chat/rtc_chat.dart)，你可以前往下载或查看其中的源代码。