AgoraRtmClientKit ~03f2af90-60ca-11ed-8dae-bf25bf08a626~
## 方法

### initWithConfig
#### 接口描述

```objc
- (instancetype _Nullable) initWithConfig:(AgoraRtmClientConfig * _Nonnull)config
                                 delegate:(id <AgoraRtmClientDelegate> _Nullable)delegate;
```

初始化 `AgoraRtmClientKit` 单实例。

> 注意：初始化实例必须在你使用 RTM 其他功能之前完成，以建立正确的账号级别的凭据（例如 APP ID）。

| 参数 |描述                    |
| --------- | ------------------------------ |
| `config` | 该 `AgoraRtmClientKit` 实例的配置信息，详见 [`AgoraRtmClientConfig`](#agorartmclientconfig)。 |
| `delegate` | 频道事件监听程序，详见 [`AgoraRtmClientDelegate`](#agorartmclientdelegate-协议)。 |

#### 基本用法

#### 初始化 RTM 实例
<mark>待补充</mark>

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### destroy
#### 接口描述

```objc
- (int) destroy;
```

销毁一个 `AgoraRtmClientKit` 单实例以释放资源。

成功销毁后，如需调用 `AgoraRtmClientKit` 类的 API，你需要调用 [`initWithConfig`](#initwithconfig) 重新初始化 `AgoraRtmClientKit` 单实例。

#### 基本用法
<mark>待补充</mark>

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### createStreamChannel
#### 接口描述

```objc
- (AgoraRtmStreamChannel * _Nullable)createStreamChannel:(NSString * _Nonnull)channelName;
```

创建一个 `AgoraRtmStreamChannel` 实例。

~8704dfd0-60c9-11ed-8dae-bf25bf08a626~

创建 `AgoraRtmStreamChannel` 实例后你可以调用其他频道相关方法，详见 [`StreamChannel` 类](api-channel-ios)。

| 参数  | 描述                    |
| --------- | ------------------------------ |
| `channelName` | 频道名称。~23141e20-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法
<mark>待补充</mark>

#### 返回值
- 一个 `AgoraRtmStreamChannel` 实例：调用成功。
- `nil`：调用失败。

## 回调

### AgoraRtmClientDelegate 协议

通过添加事件监听处理程序以获方法调用结果以及事件通知，包括连接状态，消息到达，Presence 状态等事件通知以及监控方法回调结果。如需要在 App 中接收消息和事件通知，在调用这些函数前必须先添加事件监听处理程序。

#### 添加事件监听程序

```objc

```

### joinChannel
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    joinChannel:(NSString * _Nonnull)channelName
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`joinWithOption`](api-channel-ios#joinwithoption) 调用的异步回调，在加入频道时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 加入频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

### leaveChannel
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    leaveChannel:(NSString * _Nonnull)channelName
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`leave`](api-channel-ios#leave) 调用的异步回调，在离开频道时触发。

| 参数    | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName`| 事件所在频道名称。 |
| `userId`     | 离开频道用户的 User ID。  |
| `errorCode`  | 频道错误码，详见 [`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

### joinTopic
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    joinTopic:(NSString * _Nonnull)topic
    inChannel:(NSString * _Nonnull)channelName
    withMeta:(NSData * _Nullable)meta
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`joinTopic`](api-channel-ios#jointopic) 调用的异步回调，在加入 Topic 时触发。

| 参数    | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName`  | 事件所在频道名称。 |
| `userId`      | 加入 Topic 用户的 User ID。  |
| `topic`        | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`    | 频道错误码，详见 [`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

### leaveTopic
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    leaveTopic:(NSString * _Nonnull)topic
    inChannel:(NSString * _Nonnull)channelName
    withMeta:(NSData * _Nullable)meta
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`leaveTopic`](api-channel-ios#leavetopic) 调用的异步回调，在离开 Topic 时触发。

| 参数    | 描述                              |
| -------------  | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开 Topic 用户的 User ID。  |
| `topic`       | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

### subscribe
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    inTopic:(NSString * _Nonnull)topic
    inChannel:(NSString * _Nonnull)channelName
    withSubscribeSuccess:(NSArray<NSString *> * _Nonnull)succeedUsers
    withSubscribeFailed:(NSArray<NSString *> * _Nonnull)failedUsers
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`subscribeTopic`](api-channel-ios#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。

| 参数      | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 订阅 Topic 用户的 User ID。  |
| `topic`       | 事件所在 Topic 名称。|
| `succeedUsers`| 本次订阅成功的消息发布者列表。                       |
| `failedUsers` | 本次订阅失败的消息发布者列表。订阅失败的原因可能是超过了订阅数量限制。                      |
| `errorCode`   | 频道错误码，详见 [`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

### unsubscribe
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onUser:(NSString * _Nonnull)userId
    inTopic:(NSString * _Nonnull)topic
    inChannel:(NSString * _Nonnull)channelName
    withUnsubscribeSuccess:(NSArray<NSString *> * _Nonnull)succeedUsers
    withUnsubscribeFailed:(NSArray<NSString *> * _Nonnull)failedUsers
    result:(AgoraRtmStreamChannelErrorCode)errorCode;
```

[`unsubscribeTopic`](api-channel-ios#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。

| 参数        | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`     | 取消订阅 Topic 用户的 User ID。  |
| `topicName`     | 事件所在 Topic 名称。 |
| `succeedUsers` | 本次取消订阅成功的消息发布者列表。                       |
| `failedUsers`   | 本次取消订阅失败的消息发布者列表。取消订阅失败的原因可能是之前未订阅过该用户。                        |
| `errorCode`    | 频道错误码，详见[`AgoraRtmStreamChannelErrorCode`](#agorartmstreamchannelerrorcode)。       |

## 事件通知

### connectionStateChange
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)kit
    channel:(NSString * _Nonnull)channelName
    connectionStateChanged:(AgoraRtmClientConnectionState)state
    result:(AgoraRtmClientConnectionChangeReason)reason;
```

SDK 连接状态发生改变时会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `channelName` | 事件所在频道名称。 |
| `state`  | SDK 连接状态，详见 [`AgoraRtmClientConnectionState`](#agorartmclientconnectionstate)。  |
| `reason`   | SDK 连接状态改变原因，详见 [`AgoraRtmClientConnectionChangeReason`](#agorartmclientconnectionchangereason)。  |

#### 基本用法
##### 处理断连

```objc

```


### onPresenceEvent
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onPresenceEvent:(AgoraRtmPresenceEvent * _Nonnull)event;
```

当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。

你可以根据实际场景监听需要的 Presence 类型：

- 频道相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）：
    - `AgoraRtmPresenceTypeRemoteJoinChannel(0)`：远端用户加入频道。
    - `AgoraRtmPresenceTypeRemoteLeaveChannel(1)`：远端用户离开频道。
    - `AgoraRtmPresenceTypeRemoteConnectionTimeout(2)`：远端用户连接超时。

- Topic 相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）、Topic 信息（`topicInfos`）：
    - `AgoraRtmPresenceTypeRemoteJoinTopic(3)`：远端用户加入 Topic。
    - `AgoraRtmPresenceTypeRemoteLeaveTopic(4)`：远端用户离开 Topic。
    - `AgoraRtmPresenceTypeSelfJoinChannel(5)`：本地用户加入频道，收到该频道中所有 Topic 的信息。

| 参数   | 描述      |
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`AgoraRtmPresenceEvent`](#agorartmpresenceevent)。  |


### onMessageEvent
#### 接口描述

```objc
- (void)rtmKit:(AgoraRtmClientKit * _Nonnull)rtmKit
    onMessageEvent:(AgoraRtmMessageEvent * _Nonnull)event;
```

在不同频道类型中，SDK 发送该事件通知的情况不同。具体如下：
- 在 Message Channel 中，当你在频道中订阅的用户发布消息时，SDK 会发送该事件通知。
- 在 Stream Channel 中，当你在 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`AgoraRtmMessageEvent`](#agorartmmessageevent)。  |



## Interface

### AgoraRtmClientConfig


```objc
__attribute__((visibility("default"))) @interface AgoraRtmClientConfig: NSObject

@property (nonatomic, copy, nonnull) NSString *appId;

@property (nonatomic, copy, nonnull) NSString *userId;
```

用于存储 `AgoraRtmClientKit` 实例的配置信息，这些信息将影响后续 RTM 客户端的行为。

| 属性  | 描述           |
| ------------ |----------------------------------- |
| `appId`       | 从声网控制台上获取的 APP ID。                                                        |
| `userId`      | 用户 ID，用户或设备设置唯一的标识符。你需要维护 `userId` 和用户之间的映射关系，并在整个服务周期内不能改变该映射关系。如果不设置该属性，将无法连接到 RTM 服务。     |


#### 基本用法

```objc

```


### AgoraRtmMessageEvent

```objc
__attribute__((visibility("default"))) @interface AgoraRtmMessageEvent: NSObject
rty (nonatomic, assign, readonly) AgoraRtmChannelType channelType;

@property (nonatomic, copy, nonnull) NSString *channelName;

@property (nonatomic, copy, nonnull) NSString *channelTopic;

@property (nonatomic, copy, nonnull) NSData *message;

@property (nonatomic, copy, nonnull) NSString *publisher;
```

消息回调事件。

| 属性   | 描述      |
| ------------  | --------- |
| `channelType`  | 频道类型，详见 [`AgoraRtmChannelType`](#agorartmchanneltype)。  |
| `channelName`   | 频道名称。  |
| `channelTopic`  | Topic 名称。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>AgoraRtmChannelTypeStream(1)</code> 时生效。</div>  |
| `message`   | 消息负载。  |
| `publisher`   | 发布消息用户的 User ID。  |

### AgoraRtmPresenceEvent

```objc
__attribute__((visibility("default"))) @interface AgoraRtmPresenceEvent: NSObject

@property (nonatomic, assign, readonly) AgoraRtmChannelType channelType;

@property (nonatomic, assign, readonly) AgoraRtmPresenceType type;

@property (nonatomic, copy, nonnull) NSString *channelName;

@property (nonatomic, copy, nonnull) NSArray<AgoraRtmTopicInfo *> *topicInfos;

@property (nonatomic, copy, nonnull) NSString *userId;
```

Presence 回调事件。

| 属性    | 描述      |
| ------------ | --------- |
| `type`        |Presence 类型，详见 [`AgoraRtmPresenceType`](#agorartmpresencetype)。  |
| `channelType`         |频道类型，详见 [`AgoraRtmChannelType`](#agorartmchanneltype)。  |
| `channelName`      | 频道名称。  |
| `topicInfos`    | Topic 信息，详见 [`AgoraRtmTopicInfo`](#agorartmtopicinfo)。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>AgoraRtmChannelTypeStream(1)</code> 且 <code>type</code> 为 <code>AgoraRtmClientConnectionStateConnected(3)</code>、<code>AgoraRtmClientConnectionStateReconnecting(4)</code> 或 <code>AgoraRtmClientConnectionStateFailed(5)</code> 时生效。</div>  |
| `userId`    | 触发 Presence 事件用户的 User ID。  |

### AgoraRtmTopicInfo

```objc
__attribute__((visibility("default"))) @interface AgoraRtmTopicInfo: NSObject

@property (nonatomic, copy, nonnull) NSString *topic;

@property (nonatomic, copy, nonnull) NSArray<NSString *> *publisherUserIds;

@property (nonatomic, copy, nonnull) NSArray<NSData *> *publisherMetas;
```

Topic 信息。

| 属性 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `publisherUserIds`   | 向该 Topic 发布消息的用户 ID 列表。 |
| `publisherMetas`   | 向该 Topic 发布消息的用户的元数据列表。 |

## Enum


### AgoraRtmStreamChannelErrorCode

```objc
typedef NS_ENUM(NSInteger, AgoraRtmStreamChannelErrorCode) {

  AgoraRtmStreamChannelErrorOk = 0,

  AgoraRtmStreamChannelErroExceedLimitation = 1,

  AgoraRtmStreamChannelErrorUserNotExist = 2,
};
```

频道错误码。

| 枚举值    | 描述      | 
| ------------ | --------- |
| `AgoraRtmStreamChannelErrorOk`     | 0: 操作成功。  | 
| `AgoraRtmStreamChannelErroExceedLimitation`     | 1: 订阅用户超出数量限制。  | 
| `AgoraRtmStreamChannelErrorUserNotExist`     | 2: 订阅或取消订阅的用户不存在。  | 

### AgoraRtmMessageEvent

```objc
typedef NS_ENUM(NSInteger, AgoraRtmChannelType) {

  AgoraRtmChannelTypeMessage = 0,

  AgoraRtmChannelTypeStream = 1,
};
```

RTM 频道类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `AgoraRtmChannelTypeMessage`     | 0: Message Channel。  |
| `AgoraRtmChannelTypeStream`     | 1: Stream Channel。  |


### AgoraRtmClientConnectionState

```objc
typedef NS_ENUM(NSInteger, AgoraRtmClientConnectionState) {

  AgoraRtmClientConnectionStateDisconnected = 1,

  AgoraRtmClientConnectionStateConnecting = 2,

  AgoraRtmClientConnectionStateConnected = 3,

  AgoraRtmClientConnectionStateReconnecting = 4,

  AgoraRtmClientConnectionStateFailed = 5,
};
```

SDK 连接状态。

| 枚举值  | 描述                                                                                                |
| ----------------------------------- | ----------------------------------------------- |
| `AgoraRtmClientConnectionStateDisconnected` | 1: SDK 已和服务器断开连接。                                                                      |
| `AgoraRtmClientConnectionStateConnecting`   | 2: SDK 正在连接服务器。                                                                    |
| `AgoraRtmClientConnectionStateConnected`    | 3: SDK 已连上服务器。                                                                           |
| `AgoraRtmClientConnectionStateReconnecting` | 4: SDK 和服务器断开连接，正在重新连接服务器。 |
| `AgoraRtmClientConnectionStateFailed`       | 5: SDK 无法连接服务器。                                                                      |

### AgoraRtmClientConnectionChangeReason

```objc
typedef NS_ENUM(NSInteger, AgoraRtmClientConnectionChangeReason) {

  AgoraRtmClientConnectionChangedConnecting = 0,

  AgoraRtmClientConnectionChangedJoinSuccess = 1,

  AgoraRtmClientConnectionChangedInterrupted = 2,

  AgoraRtmClientConnectionChangedBannedByServer = 3,

  AgoraRtmClientConnectionChangedJoinFailed = 4,

  AgoraRtmClientConnectionChangedLeaveChannel = 5,

  AgoraRtmClientConnectionChangedInvalidAppId = 6,

  AgoraRtmClientConnectionChangedInvalidChannelName = 7,

  AgoraRtmClientConnectionChangedInvalidToken = 8,

  AgoraRtmClientConnectionChangedTokenExpired = 9,

  AgoraRtmClientConnectionChangedRejectedByServer = 10,

  AgoraRtmClientConnectionChangedSettingProxyServer = 11,

  AgoraRtmClientConnectionChangedRenewToken = 12,

  AgoraRtmClientConnectionChangedClientIpAddressChanged = 13,

  AgoraRtmClientConnectionChangedKeepAliveTimeout = 14,

  AgoraRtmClientConnectionChangedRejoinSuccess = 15,

  AgoraRtmClientConnectionChangedChangedLost = 16,

  AgoraRtmClientConnectionChangedEchoTest = 17,

  AgoraRtmClientConnectionChangedClientIpAddressChangedByUser = 18,

  AgoraRtmClientConnectionChangedSameUidLogin = 19,

  AgoraRtmClientConnectionChangedTooManyBroadcasters = 20,
};
```

SDK 连接状态改变原因。

| 枚举值                             | 描述                          |
| ------------------------------------ | ------------------------------------ |
|  `AgoraRtmClientConnectionChangedConnecting`   | 0: 建立网络连接中。 |
|  `AgoraRtmClientConnectionChangedJoinSuccess` | 1: 成功加入频道。  |
|  `AgoraRtmClientConnectionChangedInterrupted`   | 2: 网络连接中断。  |
|  `AgoraRtmClientConnectionChangedBannedByServer`  | 3: 网络连接被服务器禁止。   |
|  `AgoraRtmClientConnectionChangedJoinFailed`  |  4: SDK 连续 20 分钟无法加入频道，停止重连频道。  |
|  `AgoraRtmClientConnectionChangedLeaveChannel`  |  5: 离开频道。                            |
|  `AgoraRtmClientConnectionChangedInvalidAppId` |  6: 不是有效的 APP ID，无法加入频道。    |
|  `AgoraRtmClientConnectionChangedInvalidChannelName` |  7: 不是有效的频道名，无法加入频道。     |
|  `AgoraRtmClientConnectionChangedInvalidToken`  |   8: Token 无效，无法加入频道。   |
|  `AgoraRtmClientConnectionChangedTokenExpired` |  9: Token 过期，无法加入频道。     |
|  `AgoraRtmClientConnectionChangedRejectedByServer`  |  10: 被服务器禁止连接。    |
|  `AgoraRtmClientConnectionChangedSettingProxyServer` |  11: 由于设置了代理服务器，SDK 尝试重连。   |
|  `AgoraRtmClientConnectionChangedRenewToken`  |   12: 更新 Token 引起网络连接状态改变。    |
|  `AgoraRtmClientConnectionChangedClientIpAddressChanged` |  13: 由于网络类型，或网络运营商的 IP 或端口发生改变，客户端 IP 地址变更，SDK 尝试重连。     |
|  `AgoraRtmClientConnectionChangedKeepAliveTimeout`  | 14: SDK 和服务器连接保活超时，进入自动重连状态。   |
|  `AgoraRtmClientConnectionChangedRejoinSuccess`  |  15: SDK 重连成功。    |
|  `AgoraRtmClientConnectionChangedChangedLost`  |    16: SDK 丢失与服务器的连接。    |
|  `AgoraRtmClientConnectionChangedEchoTest`  |   17: 通话回声测试引起连接状态改变。     |
|  `AgoraRtmClientConnectionChangedClientIpAddressChangedByUser`|  18: 用户变更客户端 IP 地址，SDK 尝试重连。  |
|  `AgoraRtmClientConnectionChangedSameUidLogin`  |  19: 使用相同的 UID 从不同的设备加入同一频道。   |
|  `AgoraRtmClientConnectionChangedTooManyBroadcasters`  |  20: 频道内主播人数已达上限。   |

### AgoraRtmPresenceType

```objc
typedef NS_ENUM(NSInteger, AgoraRtmPresenceType) {

  AgoraRtmPresenceTypeRemoteJoinChannel = 0,

  AgoraRtmPresenceTypeRemoteLeaveChannel = 1,

  AgoraRtmPresenceTypeRemoteConnectionTimeout = 2,

  AgoraRtmPresenceTypeRemoteJoinTopic = 3,

  AgoraRtmPresenceTypeRemoteLeaveTopic = 4,

  AgoraRtmPresenceTypeSelfJoinChannel = 5,
};
```

Presence 类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `AgoraRtmPresenceTypeRemoteJoinChannel`     | 0: 远端用户加入频道。  |
| `AgoraRtmPresenceTypeRemoteLeaveChannel`     | 1: 远端用户离开频道。  |
| `AgoraRtmPresenceTypeRemoteConnectionTimeout`     | 2: 远端用户连接超时。  |
| `AgoraRtmPresenceTypeRemoteJoinTopic`     | 3: 远端用户加入 Topic。  |
| `AgoraRtmPresenceTypeRemoteLeaveTopic`     | 4: 远端用户离开 Topic。  |
| `AgoraRtmPresenceTypeSelfJoinChannel`     | 5: 本地用户加入频道，收到该频道中所有 Topic 的信息。  |

