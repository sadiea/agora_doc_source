RtmClient ~03f2af90-60ca-11ed-8dae-bf25bf08a626~

## 方法

方法调用失败时会抛出 `AgoraRtcException` 错误，请根据返回的错误码对照 [RTM 错误码](error-codes)查看错误原因。
### createAgoraRtmClient
#### 接口描述

```dart
RtmClient createAgoraRtmClient() {
  return RtmClientImpl.create();
}
```
创建一个 `RtmClient` 单实例。

> 注意：
> - 在调用 `RtmClient` 类的方法之前，你需要调用该方法创建 `RtmClient` 单实例。
> - 在调用 [`release`](#release) 销毁 `RtmClient` 单实例之前，多次调用 `createAgoraRtmClient` 获取的是同一个 `RtmClient` 单实例。Agora 不推荐多次调用 `createAgoraRtmClient`。

#### 返回值
- 一个 `RtmClient` 对象：调用成功。

### initialize
#### 接口描述

```dart
Future<void> initialize(RtmConfig config);
```
初始化 `RtmClient` 单实例。

> 注意：初始化实例必须在你调用 [`createAgoraRtmClient`](#createagorartmclient) 之后，使用 RTM 其他功能之前完成，以建立正确的账号级别的凭据（例如 APP ID）。

| 参数 |描述                    |
| --------- |  ------------------------------ |
| `config` | 该 `RtmClient` 实例的配置信息，详见 [`RtmConfig`](#rtmconfig)。 |

#### 基本用法

#### 初始化 RTM 实例
<mark>待补充</mark>


### release
#### 接口描述

```dart
Future<void> release();
```

销毁 `RtmClient` 单实例以释放资源。

成功销毁后，如需调用 `RtmClient` 类的 API，你需要调用 [`createAgoraRtmClient`](#createagorartmclient) 重新创建 `RtmClient` 单实例。

#### 基本用法
<mark>待补充</mark>


### createStreamChannel
#### 接口描述

```dart
Future<StreamChannel> createStreamChannel(String channelName);
```

创建一个 `StreamChannel` 类型实例。

~8704dfd0-60c9-11ed-8dae-bf25bf08a626~

创建 `StreamChannel` 实例后你可以调用其他频道相关方法，详见 [StreamChannel 类](api-channel-flutter)。

| 参数     | 描述                  |
| ------------- | ---------------------------- |
| `channelName` | 频道名称。~23141e20-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法

##### 创建一个 `StreamChannel` 类型实例


#### 返回值
- 一个 `StreamChannel` 类型实例：调用成功。

## 回调

### RtmEventHandler 类

通过添加事件监听处理程序以获方法调用结果以及事件通知，包括连接状态，消息到达，Presence 状态等事件通知以及监控方法回调结果。如需要在 App 中接收消息和事件通知，在调用这些函数前必须先添加事件监听处理程序。

#### 基本用法

##### 添加事件监听程序
<mark>待补充</mark>


### onJoinResult
#### 接口描述

```dart
final void Function(
        String channelName, String userId, StreamChannelErrorCode errorCode)?
    onJoinResult;
```

[`join`](api-channel-flutter#join) 调用的异步回调，在加入频道时触发。

| 参数      | 描述                              |
| ------------- |  ---------------------------------------- |
| `channelName` |  事件所在频道名称。 |
| `userId`      | 加入频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onLeaveResult
#### 接口描述

```dart
final void Function(
        String channelName, String userId, StreamChannelErrorCode errorCode)?
    onLeaveResult;
```

[`leave`](api-channel-flutter#leave) 调用的异步回调，在离开频道时触发。

| 参数      | 描述                              |
| ------------- |  ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onJoinTopicResult
#### 接口描述

```dart
final void Function(String channelName, String userId, String topic,
    String meta, StreamChannelErrorCode errorCode)? onJoinTopicResult;
```

[`joinTopic`](api-channel-flutter#jointopic) 调用的异步回调，在加入 Topic 时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 加入 Topic 用户的 User ID。  |
| `topic`       | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onLeaveTopicResult
#### 接口描述

```dart
final void Function(String channelName, String userId, String topic,
    String meta, StreamChannelErrorCode errorCode)? onLeaveTopicResult;
```

[`leaveTopic`](api-channel-flutter#leavetopic) 调用的异步回调，在离开 Topic 时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开 Topic 用户的 User ID。  |
| `topic`       | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onTopicSubscribed
#### 接口描述

```dart
final void Function(
    String channelName,
    String userId,
    String topic,
    UserList succeedUsers,
    UserList failedUsers,
    StreamChannelErrorCode errorCode)? onTopicSubscribed;
```

[`subscribeTopic`](api-channel-flutter#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中发布消息的用户时触发。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 订阅 Topic 用户的 User ID。  |
| `topic`        | 事件所在 Topic 名称。 |
| `succeedUsers` | 本次订阅成功的消息发布者列表，详见 [`UserList`](api-channel-flutter#userlist)。                       |
| `failedUsers`  | 本次订阅订失败的消息发布者列表，详见 [`UserList`](api-channel-flutter#userlist)。 订阅失败的原因可能是超过了订阅数量限制。                      |
| `errorCode`    | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。 |

### onTopicUnsubscribed
#### 接口描述

```dart
final void Function(
    String channelName,
    String userId,
    String topic,
    UserList succeedUsers,
    UserList failedUsers,
    StreamChannelErrorCode errorCode)? onTopicUnsubscribed;
```

[`unsubscribeTopic`](api-channel-flutter#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中发布消息的用户时触发。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 取消订阅 Topic 用户的 User ID。  |
| `topic`        | 事件所在 Topic 名称。 |
| `succeedUsers` | 本次取消订阅成功的消息发布者列表，详见 [`UserList`](api-channel-flutter#userlist)。  |
| `failedUsers`  | 本次取消订阅失败的消息发布者列表，详见 [`UserList`](api-channel-flutter#userlist)。取消订阅失败的原因可能是之前未订阅过该用户。|
| `errorCode`    |频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。 |

## 事件通知

### onConnectionStateChange
#### 接口描述

```dart
final void Function(String channelName, RtmConnectionState state,
    RtmConnectionChangeReason reason)? onConnectionStateChange;
}
```

SDK 连接状态发生改变时会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `channelName` | 事件所在频道名称。 |
| `state`  | SDK 连接状态，详见 [`RtmConnectionState`](#rtmconnectionstate)。  |
| `reason`   | SDK 连接状态改变原因，详见 [`RtmConnectionChangeReason`](#rtmconnectionchangereason)。  |

#### 基本用法
##### 处理断连
（待补充）

### onPresenceEvent
#### 接口描述

```dart
final void Function(PresenceEvent event)? onPresenceEvent;
```

当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。

你可以根据实际场景监听需要的 Presence 类型：

- 频道相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）：
	- `rtmPresenceTypeRemoteJoinChannel(0)`：远端用户加入频道。
	- `rtmPresenceTypeRemoteLeaveChannel(1)`：远端用户离开频道。
	- `rtmPresenceTypeRemoteConnectionTimeout(2)`：远端用户连接超时。

- Topic 相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）、Topic 信息（`topicInfos`）、Topic 信息数量（`topicInfoNumber`）：
	- `rtmPresenceTypeRemoteJoinTopic(3)`：远端用户加入 Topic。
	- `rtmPresenceTypeRemoteLeaveTopic(4)`：远端用户离开 Topic。
	- `rtmPresenceTypeSelfJoinChannel(5)`：本地用户加入频道，收到该频道中所有 Topic 的信息。

| 参数   |  描述      |
| ------------ |  --------- |
| `event`  | 消息事件类型，详见 [`PresenceEvent`](#presenceeevent)。  |

### onMessageEvent
#### 接口描述

```dart
final void Function(MessageEvent event)? onMessageEvent;
```

在不同频道类型中，SDK 发送该事件通知的情况不同。具体如下：
- 在 Message Channel 中，当你在频道中订阅的用户发布消息时，SDK 会发送该事件通知。
- 在 Stream Channel 中，当你在 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。

| 参数   | 描述      |
| ------------ |--------- |
| `event`  | 消息事件类型，详见 [`MessageEvent`](#messageevent)。  |


## Class

### RtmConfig

```dart
class RtmConfig {

  @JsonKey(name: 'appId')
  final String? appId;

  @JsonKey(name: 'userId')
  final String? userId;

  @JsonKey(name: 'useStringUserId')
  final bool? useStringUserId;

  @JsonKey(name: 'eventHandler', ignore: true)
  final RtmEventHandler? eventHandler;

  @JsonKey(name: 'logConfig')
  final LogConfig? logConfig;

  factory RtmConfig.fromJson(Map<String, dynamic> json) =>
      _$RtmConfigFromJson(json);

  Map<String, dynamic> toJson() => _$RtmConfigToJson(this);
}
```

RTM 客户端配置信息。

| 参数   | 描述           |
| ------------ | ----------------------------------- |
| `appId`        | 从声网控制台上获取的 APP ID。                                                        |
| `userId`       | 用户 ID，用户或设备设置唯一的标识符。你需要维护 `userId` 和用户之间的映射关系，并在整个服务周期内不能改变该映射关系。如果不设置该参数，将无法连接到 RTM 服务。                               |
| `useStringUserId`   | 是否使用 String 型 User ID：<ul><li>`true`：使用 String 型 User ID，即 `userId` 填 String。</li><li>`false`：不使用 String 型 User ID，则 `userId` 填 Int。</li></ul>  |
| `eventHandler` | 事件监听函数句柄，用以监听消息通知，Presence 通知，状态变更通知等事件通知。详见 [`RtmEventHandler`](#rtmeventhandler)。                               |
| `logConfig`    | （选填）日志存储功能。Agora 建议你在调试和定位问题时候开启该功能，日志将会保存在你设置的位置，Agora 技术人员将根据日志详情帮助你分析定位问题。应用正式上线后建议取消设置该功能。详见 [`LogConfig`](#logconfig)。   |

#### 基本用法
<mark>待补充</mark>


### MessageEvent

```dart
class MessageEvent {

  @JsonKey(name: 'channelType')
  final RtmChannelType? channelType;

  @JsonKey(name: 'channelName')
  final String? channelName;

  @JsonKey(name: 'channelTopic')
  final String? channelTopic;

  @JsonKey(name: 'message', ignore: true)
  final Uint8List? message;

  @JsonKey(name: 'messageLength')
  final int? messageLength;

  @JsonKey(name: 'publisher')
  final String? publisher;

  factory MessageEvent.fromJson(Map<String, dynamic> json) =>
      _$MessageEventFromJson(json);

  Map<String, dynamic> toJson() => _$MessageEventToJson(this);
}
```

消息回调事件。

| 参数   | 描述      |
| ------------ |  --------- |
| `channelType`  | 频道类型，详见 [`RtmChannelType`](#rtmchanneltype)。  |
| `channelName`   | 频道名称。  |
| `channelTopic`  | Topic 名称。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>rtmChannelTypeStream(1)</code> 时生效。</div>  |
| `message`   | 消息负载。  |
| `messageLength`  | 消息负载长度。  |
| `publisher`   | 发布消息用户的 User ID。  |

### PresenceEvent

```dart
class PresenceEvent {

  @JsonKey(name: 'channelType')
  final RtmChannelType? channelType;

  @JsonKey(name: 'type')
  final RtmPresenceType? type;

  @JsonKey(name: 'channelName')
  final String? channelName;

  @JsonKey(name: 'topicInfos')
  final List<TopicInfo>? topicInfos;

  @JsonKey(name: 'topicInfoNumber')
  final int? topicInfoNumber;

  @JsonKey(name: 'userId')
  final String? userId;

  factory PresenceEvent.fromJson(Map<String, dynamic> json) =>
      _$PresenceEventFromJson(json);

  Map<String, dynamic> toJson() => _$PresenceEventToJson(this);
}
```

Presence 回调事件。

| 参数    | 描述      |
| ------------ | --------- |
| `type`     | Presence 类型，详见 [`RtmPresenceType`](#rtmpresencetype)。  |
| `channelType`     | 频道类型，详见 [`RtmChannelType`](#rtmchanneltype)。  |
| `channelName`     | 频道名称。  |
| `topicInfos`    | Topic 信息，详见 [TopicInfo](#topicinfo)。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>rtmChannelTypeStream(1)</code> 且 <code>type</code> 为 <code>rtmPresenceTypeRemoteJoinTopic(3)</code>、<code>rtmPresenceTypeRemoteLeaveTopic(4)</code> 或 <code>rtmPresenceTypeSelfJoinChannel(5)</code> 时生效。</div>   |
| `topicInfoNumber`    | Topic 信息数量。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>rtmChannelTypeStream(1)</code> 且 <code>type</code> 为 <code>rtmPresenceTypeRemoteJoinTopic(3)</code>、<code>rtmPresenceTypeRemoteLeaveTopic(4)</code> 或 <code>rtmPresenceTypeSelfJoinChannel(5)</code> 时生效。</div>   |
| `userId`    | 触发 Presence 事件用户的 User ID。  |

### TopicInfo

```dart
class TopicInfo {

  @JsonKey(name: 'topic')
  final String? topic;

  @JsonKey(name: 'numOfPublisher')
  final int? numOfPublisher;

  @JsonKey(name: 'publisherUserIds')
  final List<String>? publisherUserIds;

  @JsonKey(name: 'publisherMetas')
  final List<String>? publisherMetas;

  factory TopicInfo.fromJson(Map<String, dynamic> json) =>
      _$TopicInfoFromJson(json);

  Map<String, dynamic> toJson() => _$TopicInfoToJson(this);
}
```

| 参数 | 描述                                                    |
| --------- | ------------------------------------------------------------ |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `numOfPublisher`   | 向该 Topic 发布消息的用户数量。 |
| `publisherUserIds`   | 向该 Topic 发布消息的用户 ID 列表。 |
| `publisherMetas`   | 发布消息的用户的元数据。 |

## Enum

### RtmChannelType

```dart
enum RtmChannelType {
  @JsonValue(0)
  rtmChannelTypeMessage,

  @JsonValue(1)
  rtmChannelTypeStream,
}
```

RTM 频道类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `rtmChannelTypeMessage`     | 0: Message Channel。  |
| `rtmChannelTypeStream`     | 1: Stream Channel。  |


### RtmConnectionState

```dart
enum RtmConnectionState {
  @JsonValue(1)
  rtmConnectionStateDisconnected,

  @JsonValue(2)
  rtmConnectionStateConnecting,

  @JsonValue(3)
  rtmConnectionStateConnected,

  @JsonValue(4)
  rtmConnectionStateReconnecting,

  @JsonValue(5)
  rtmConnectionStateFailed,
}
```

SDK 连接状态。

| 枚举值  | 描述    |
| ------------------------- | -------------------------------------------------- |
| `rtmConnectionStateDisconnected` | 1: SDK 已和服务器断开连接。                                                                      |
| `rtmConnectionStateConnecting`   | 2: SDK 正在连接服务器。                                                                    |
| `rtmConnectionStateConnected`    | 3: SDK 已连上服务器。                                                                           |
| `rtmConnectionStateReconnecting` | 4: SDK 和服务器断开连接，正在重新连接服务器。 |
| `rtmConnectionStateFailed`       | 5: SDK 无法连接服务器。                                                                      |

### RtmConnectionChangeReason

```dart
enum RtmConnectionChangeReason {
  @JsonValue(0)
  rtmConnectionChangedConnecting,

  @JsonValue(1)
  rtmConnectionChangedJoinSuccess,

  @JsonValue(2)
  rtmConnectionChangedInterrupted,

  @JsonValue(3)
  rtmConnectionChangedBannedByServer,

  @JsonValue(4)
  rtmConnectionChangedJoinFailed,

  @JsonValue(5)
  rtmConnectionChangedLeaveChannel,

  @JsonValue(6)
  rtmConnectionChangedInvalidAppId,

  @JsonValue(7)
  rtmConnectionChangedInvalidChannelName,

  @JsonValue(8)
  rtmConnectionChangedInvalidToken,

  @JsonValue(9)
  rtmConnectionChangedTokenExpired,

  @JsonValue(10)
  rtmConnectionChangedRejectedByServer,

  @JsonValue(11)
  rtmConnectionChangedSettingProxyServer,

  @JsonValue(12)
  rtmConnectionChangedRenewToken,

  @JsonValue(13)
  rtmConnectionChangedClientIpAddressChanged,

  @JsonValue(14)
  rtmConnectionChangedKeepAliveTimeout,

  @JsonValue(15)
  rtmConnectionChangedRejoinSuccess

  @JsonValue(16)
  rtmConnectionChangedLost,

  @JsonValue(17)
  rtmConnectionChangedEchoTest,

  @JsonValue(18)
  rtmConnectionChangedClientIpAddressChangedByUser,

  @JsonValue(19)
  rtmConnectionChangedSameUidLogin,

  @JsonValue(20)
  rtmConnectionChangedTooManyBroadcasters,
}
```

SDK 连接状态改变原因。

| 枚举值                             | 描述                          |
| ------------------------------------ | ------------------------------------ |
|  `rtmConnectionChangedConnecting`   | 0: 建立网络连接中。 |
|  `rtmConnectionChangedJoinSuccess` | 1: 成功加入频道。  |
|  `rtmConnectionChangedInterrupted`   | 2: 网络连接中断。  |
|  `rtmConnectionChangedBannedByServer`  | 3: 网络连接被服务器禁止。   |
|  `rtmConnectionChangedJoinFailed`  |  4: SDK 连续 20 分钟无法加入频道，停止重连频道。  |
|  `rtmConnectionChangedLeaveChannel`  |  5: 离开频道。                            |
|  `rtmConnectionChangedInvalidAppId` |  6: 不是有效的 APP ID，无法加入频道。    |
|  `rtmConnectionChangedInvalidChannelName` |  7: 不是有效的频道名，无法加入频道。     |
|  `rtmConnectionChangedInvalidToken`  |   8: Token 无效，无法加入频道。   |
|  `rtmConnectionChangedTokenExpired` |  9: Token 过期，无法加入频道。     |
|  `rtmConnectionChangedRejectedByServer`  |  10: 被服务器禁止连接。    |
|  `rtmConnectionChangedSettingProxyServer` |  11: 由于设置了代理服务器，SDK 尝试重连。   |
|  `rtmConnectionChangedRenewToken`  |   12: 更新 Token 引起网络连接状态改变。    |
|  `rtmConnectionChangedClientIpAddressChanged` |  13: 由于网络类型，或网络运营商的 IP 或端口发生改变，客户端 IP 地址变更，SDK 尝试重连。     |
|  `rtmConnectionChangedKeepAliveTimeout`  | 14: SDK 和服务器连接保活超时，进入自动重连状态。   |
|  `rtmConnectionChangedRejoinSuccess`  |  15: SDK 重连成功。    |
|  `rtmConnectionChangedLost`  |    16: SDK 丢失与服务器的连接。    |
|  `rtmConnectionChangedEchoTest`  |   17: 通话回声测试引起连接状态改变。     |
|  `rtmConnectionChangedClientIpAddressChangedByUser`|  18: 用户变更客户端 IP 地址，SDK 尝试重连。  |
|  `rtmConnectionChangedSameUidLogin`  |  19: 使用相同的 UID 从不同的设备加入同一频道。   |
|  `rtmConnectionChangedTooManyBroadcasters`  |  20: 频道内主播人数已达上限。   |

### StreamChannelErrorCode

```dart
enum StreamChannelErrorCode {
  @JsonValue(0)
  streamChannelErrorOk,

  @JsonValue(1)
  streamChannelErrorExceedLimitation,

  @JsonValue(2)
  streamChannelErrorUserNotExist,
}
```

频道事件错误码。

| 枚举值    | 描述      |
| ------------ | --------- |
| `streamChannelErrorOk`     | 0: 操作成功。  |
| `streamChannelErrorExceedLimitation`     | 1: 订阅用户数量超出限制。  |
| `streamChannelErrorUserNotExist`     | 2: 所订阅用户不存在。  |

### RtmPresenceType

```dart
enum RtmPresenceType {
  @JsonValue(0)
  rtmPresenceTypeRemoteJoinChannel,

  @JsonValue(1)
  rtmPresenceTypeRemoteLeaveChannel,

  @JsonValue(2)
  rtmPresenceTypeRemoteConnectionTimeout,

  @JsonValue(3)
  rtmPresenceTypeRemoteJoinTopic,

  @JsonValue(4)
  rtmPresenceTypeRemoteLeaveTopic,

  @JsonValue(5)
  rtmPresenceTypeSelfJoinChannel,
}
```

Presence 类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `rtmPresenceTypeRemoteJoinChannel`     | 0: 远端用户加入频道。  |
| `rtmPresenceTypeRemoteLeaveChannel`     | 1: 远端用户离开频道。  |
| `rtmPresenceTypeRemoteConnectionTimeout`     | 2: 远端用户连接超时。  |
| `rtmPresenceTypeRemoteJoinTopic`     | 3: 远端用户加入 Topic。  |
| `rtmPresenceTypeRemoteLeaveTopic`     | 4: 远端用户离开 Topic。  |
| `rtmPresenceTypeSelfJoinChannel`     | 5: 本地用户加入频道，收到该频道中所有 Topic 的信息。  |

