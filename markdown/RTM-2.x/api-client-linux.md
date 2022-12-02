IRtmClient ~03f2af90-60ca-11ed-8dae-bf25bf08a626~

## 方法

### createAgoraRtmClient
#### 接口描述

```cpp
AGORA_API IRtmClient* AGORA_CALL createAgoraRtmClient();
```
创建一个 `IRtmClient` 单实例。

> 注意：
> - 在调用 `IRtmClient` 类的方法之前，你需要调用该方法创建 `IRtmClient` 单实例。
> - 在调用 [`release`](#release) 销毁 `IRtmClient` 单实例之前，多次调用 `createAgoraRtmClient` 获取的是同一个 `IRtmClient` 单实例。Agora 不推荐多次调用 `createAgoraRtmClient`。

#### 返回值
- 一个 `IRtmClient` 对象：调用成功。
- `null`：调用失败。

### initialize
#### 接口描述

```cpp
virtual int initialize(const RtmConfig& config) = 0;
```
初始化 `IRtmClient` 单实例。

> 注意：初始化实例必须在你调用 [`createAgoraRtmClient`](#createagorartmclient) 之后，使用 RTM 其他功能之前完成，以建立正确的账号级别的凭据（例如 APP ID）。

| 参数 |描述                    |
| --------- |  ------------------------------ |
| `config` | 该 `IRtmClient` 实例的配置信息，详见 [`RtmConfig`](#rtmconfig)。 |

#### 基本用法

#### 初始化 RTM 实例
<mark>待补充</mark>

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### release
#### 接口描述

```cpp
virtual int release() = 0;
```

销毁一个 `IRtmClient` 单实例以释放资源。

成功销毁后，如需调用 `IRtmClient` 类的 API，你需要调用 [`createAgoraRtmClient`](#createagorartmclient) 重新创建 `IRtmClient` 单实例。

#### 基本用法
<mark>待补充</mark>

#### 返回值
- `0`：调用成功。
- <`0`：调用失败。

### createStreamChannel
#### 接口描述

```cpp
virtual IStreamChannel* createStreamChannel(const char* channelName) = 0;
```

创建一个 `IStreamChannel` 类型实例。

~8704dfd0-60c9-11ed-8dae-bf25bf08a626~

创建 `IStreamChannel` 实例后你可以调用其他频道相关方法，详见 [IStreamChannel 类](api-channel-linux)。

| 参数     | 描述                  |
| ------------- | ---------------------------- |
| `channelName` | 频道名称。~23141e20-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法

##### 创建一个 `IStreamChannel` 类型实例


#### 返回值
- 一个 `IStreamChannel` 类型实例：调用成功。
- `null`：调用失败。

## 回调

### IRtmEventHandler 类

通过添加事件监听处理程序以获方法调用结果以及事件通知，包括连接状态，消息到达，Presence 状态等事件通知以及监控方法回调结果。如需要在 App 中接收消息和事件通知，在调用这些函数前必须先添加事件监听处理程序。

#### 基本用法

##### 添加事件监听程序
<mark>待补充</mark>

### onJoinResult
#### 接口描述

```cpp
virtual void onJoinResult(const char* channelName, const char* userId, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`join`](api-channel-linux#join) 调用的异步回调，在加入频道时触发。

| 参数      | 描述                              |
| ------------- |  ---------------------------------------- |
| `channelName` |  事件所在频道名称。 |
| `userId`      | 加入频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。       |

### onLeaveResult
#### 接口描述

```cpp
virtual void onLeaveResult(const char* channelName, const char* userId, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`leave`](api-channel-linux#leave) 调用的异步回调，在离开频道时触发。

| 参数      | 描述                              |
| ------------- |  ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。       |

### onJoinTopicResult
#### 接口描述

```cpp
virtual void onJoinTopicResult(const char* channelName, const char* userId, const char* topic, const char* meta, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`joinTopic`](api-channel-linux#jointopic) 调用的异步回调，在加入 Topic 时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 加入 Topic 用户的 User ID。  |
| `topic`       | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。       |

### onLeaveTopicResult
#### 接口描述

```cpp
virtual void onLeaveTopicResult(const char* channelName, const char* userId, const char* topic, const char* meta, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`leaveTopic`](api-channel-linux#leavetopic) 调用的异步回调，在离开 Topic 时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开 Topic 用户的 User ID。  |
| `topic`       | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。       |

### onTopicSubscribed
#### 接口描述

```cpp
virtual void onTopicSubscribed(const char* channelName, const char* userId, const char* topic, UserList succeedUsers, UserList failedUsers, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`subscribeTopic`](api-channel-linux#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 订阅 Topic 用户的 User ID。  |
| `topic`        | 事件所在 Topic 名称。 |
| `succeedUsers` | 本次订阅成功的消息发布者列表，详见 [`UserList`](api-channel-linux#userlist)。                       |
| `failedUsers`  | 本次订阅失败的消息发布者列表，详见 [`UserList`](api-channel-linux#userlist)。订阅失败的原因可能是超过了订阅数量限制。                       |
| `errorCode`    | 频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。

### onTopicUnsubscribed
#### 接口描述

```cpp
virtual void onTopicUnsubscribed(const char* channelName, const char* userId, const char* topic, UserList succeedUsers, UserList failedUsers, STREAM_CHANNEL_ERROR_CODE errorCode) {}
```

[`unsubscribeTopic`](api-channel-linux#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 取消订阅 Topic 用户的 User ID。  |
| `topic`        | 事件所在 Topic 名称。 |
| `succeedUsers` | 本次取消订阅成功的消息发布者列表，详见 [`UserList`](api-channel-linux#userlist)。                       |
| `failedUsers`  | 本次取消订阅失败的消息发布者列表，详见 [`UserList`](api-channel-linux#userlist)。取消订阅失败的原因可能是之前未订阅过该用户。|
| `errorCode`    |频道错误码，详见 [`STREAM_CHANNEL_ERROR_CODE`](api-channel-linux#stream_channel_error_code)。

## 事件通知

### onConnectionStateChange
#### 接口描述

```cpp
virtual void onConnectionStateChange(const char* channelName, RTM_CONNECTION_STATE state, RTM_CONNECTION_CHANGE_REASON reason) {}
```

SDK 连接状态发生改变时会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `channelName` | 事件所在频道名称。 |
| `state`  | SDK 连接状态，详见 [`RTM_CONNECTION_STATE`](#rtm_connection_state)。  |
| `reason`   | SDK 连接状态改变原因，详见 [`RTM_CONNECTION_CHANGE_REASON`](#rtm_connection_change_reason)。  |

#### 基本用法

##### 处理断连
<mark>待补充</mark>

### onPresenceEvent
#### 接口描述

```cpp
virtual void onPresenceEvent(PresenceEvent& event) {}
```

当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。

你可以根据实际场景监听需要的 Presence 类型：

- 频道相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）：
    - `RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL(0)`：远端用户加入频道。
    - `RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL(1)`：远端用户离开频道。
    - `RTM_PRESENCE_TYPE_REMOTE_CONNECTION_TIMEOUT(2)`：远端用户连接超时。

- Topic 相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）、Topic 信息（`topicInfos`）、Topic 信息数量（`topicInfoNumber`）：
    - `RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3)`：远端用户加入 Topic。
    - `RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4)`：远端用户离开 Topic。
    - `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5)`：本地用户加入频道，收到该频道中所有 Topic 的信息。

| 参数   |  描述      |
| ------------ |  --------- |
| `event`  | 消息事件类型，详见 [`PresenceEvent`](#presenceeevent)。  |


### onMessageEvent
#### 接口描述

```cpp
virtual void onMessageEvent(MessageEvent& event) {}
```

在不同频道类型中，SDK 发送该事件通知的情况不同。具体如下：
- 在 Message Channel 中，当你在频道中订阅的用户发布消息时，SDK 会发送该事件通知。
- 在 Stream Channel 中，当你在 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。

| 参数   | 描述      |
| ------------ |--------- |
| `event`  | 消息事件类型，详见 [`MessageEvent`](#messageevent)。  |

## Struct

### RtmConfig

```cpp
struct RtmConfig {

  const char* appId;

  const char* userId;

  IRtmEventHandler* eventHandler;

  commons::LogConfig logConfig;

  RtmConfig() : appId(nullptr),
                userId(nullptr),
                eventHandler(nullptr) {}
}
```

用于存储配置信息，这些信息将影响后续 RTM 客户端的行为。

| 参数   | 描述           |
| ------------ | ----------------------------------- |
| `appId`        | 从声网控制台上获取的 APP ID。                                                        |
| `userId`       | 用户 ID，用户或设备设置唯一的标识符。你需要维护 `userId` 和用户之间的映射关系，并在整个服务周期内不能改变该映射关系。如果不设置该参数，将无法连接到 RTM 服务。                              |
| `eventHandler` | 事件监听函数句柄，用以监听消息通知，Presence 通知，状态变更通知等事件通知。详见 [`IRtmEventHandler`](#irtmeventhandler)。                               |
| `logConfig`    | （选填）日志存储功能。Agora 建议你在调试和定位问题时候开启该功能，日志将会保存在你设置的位置，Agora 技术人员将根据日志详情帮助你分析定位问题。应用正式上线后建议取消设置该功能。详见 [`LogConfig`](#logconfig)。   |

#### 基本用法
<mark>待补充</mark>


### MessageEvent

```cpp
  struct MessageEvent {

    RTM_CHANNEL_TYPE channelType;

    const char* channelName;

    const char* channelTopic;

    const char* message;

    size_t messageLength;

    const char* publisher;

    MessageEvent() : channelType(RTM_CHANNEL_TYPE_STREAM),
                     channelName(nullptr),
                     channelTopic(nullptr),
                     message(nullptr),
                     publisher(nullptr) {}
  };
```

消息回调事件。

| 参数   | 描述      |
| ------------ | --------- |
| `channelType`  | 频道类型，详见 [`RTM_CHANNEL_TYPE`](#rtm_channel_type)。  |
| `channelName`   | 频道名称。  |
| `channelTopic`  | Topic 名称。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>RTM_CHANNEL_TYPE_STREAM(1)</code> 时生效。</div>  |
| `message`   | 消息负载。  |
| `messageLength`   | 消息负载。  |
| `publisher`   | 发布消息用户的 User ID。  |

### PresenceEvent

```cpp
  struct PresenceEvent {

    RTM_CHANNEL_TYPE channelType;

    RTM_PRESENCE_TYPE type;

    const char* channelName;

    TopicInfo* topicInfos;

    size_t topicInfoNumber;

    const char* userId;

    PresenceEvent() : channelType(RTM_CHANNEL_TYPE_STREAM),
                      type(RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL),
                      channelName(nullptr),
                      topicInfos(nullptr),
                      topicInfoNumber(0),
                      userId(nullptr) {}
  };
```

Presence 回调事件。

| 参数    | 描述      |
| ------------ | --------- |
| `type`     | Presence 类型，详见 [`RTM_PRESENCE_TYPE`](#rtm_presence_type)。  |
| `channelType`     | 频道类型，详见 [`RTM_CHANNEL_TYPE`](#rtm_channel_type)。  |
| `channelName`     | 频道名称。  |
| `topicInfos`    | Topic 信息，详见 [`TopicInfo`](#api-topic-linux#topicinfo)。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>RTM_CHANNEL_TYPE_STREAM(1)</code> 且 <code>type</code> 为 <code>RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3)</code>、<code>RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4)</code> 或 <code>RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5)</code> 时生效。</div>  |
| `topicInfoNumber`    | Topic 信息数量。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>RTM_CHANNEL_TYPE_STREAM(1)</code> 且 <code>type</code> 为 <code>RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3)</code>、<code>RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4)</code> 或 <code>RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5)</code> 时生效。</div>  |
| `userId`    | 触发 Presence 事件用户的 User ID。。  |

## Enum

### RTM_CHANNEL_TYPE

```cpp
enum RTM_CHANNEL_TYPE {

  RTM_CHANNEL_TYPE_MESSAGE = 0,

  RTM_CHANNEL_TYPE_STREAM = 1,
};
```

RTM 频道类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `RTM_CHANNEL_TYPE_MESSAGE`     | 0: Message Channel。  |
| `RTM_CHANNEL_TYPE_STREAM`     | 1: Stream Channel。  |


### RTM_CONNECTION_STATE

```cpp
enum RTM_CONNECTION_STATE {

  RTM_CONNECTION_STATE_DISCONNECTED = 1,

  RTM_CONNECTION_STATE_CONNECTING = 2,

  RTM_CONNECTION_STATE_CONNECTED = 3,

  RTM_CONNECTION_STATE_RECONNECTING = 4,

  RTM_CONNECTION_STATE_FAILED = 5,
};
```

SDK 连接状态。

| 枚举值  | 描述    |
| ------------------------- | -------------------------------------------------- |
| `RTM_CONNECTION_STATE_DISCONNECTED` | 1: SDK 已和服务器断开连接。                                                                      |
| `RTM_CONNECTION_STATE_CONNECTING`   | 2: SDK 正在连接服务器。                                                                    |
| `RTM_CONNECTION_STATE_CONNECTED`    | 3: SDK 已连上服务器。                                                                           |
| `RTM_CONNECTION_STATE_RECONNECTING` | 4: SDK 和服务器断开连接，正在重新连接服务器。 |
| `RTM_CONNECTION_STATE_FAILED`       | 5: SDK 无法连接服务器。                                                                      |

### RTM_CONNECTION_CHANGE_REASON

```cpp
enum RTM_CONNECTION_CHANGE_REASON {

  RTM_CONNECTION_CHANGED_CONNECTING = 0,

  RTM_CONNECTION_CHANGED_JOIN_SUCCESS = 1,

  RTM_CONNECTION_CHANGED_INTERRUPTED = 2,

  RTM_CONNECTION_CHANGED_BANNED_BY_SERVER = 3,

  RTM_CONNECTION_CHANGED_JOIN_FAILED = 4,

  RTM_CONNECTION_CHANGED_LEAVE_CHANNEL = 5,

  RTM_CONNECTION_CHANGED_INVALID_APP_ID = 6,

  RTM_CONNECTION_CHANGED_INVALID_CHANNEL_NAME = 7,

  RTM_CONNECTION_CHANGED_INVALID_TOKEN = 8,

  RTM_CONNECTION_CHANGED_TOKEN_EXPIRED = 9,

  RTM_CONNECTION_CHANGED_REJECTED_BY_SERVER = 10,

  RTM_CONNECTION_CHANGED_SETTING_PROXY_SERVER = 11,

  RTM_CONNECTION_CHANGED_RENEW_TOKEN = 12,

  RTM_CONNECTION_CHANGED_CLIENT_IP_ADDRESS_CHANGED = 13,

  RTM_CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT = 14,

  RTM_CONNECTION_CHANGED_REJOIN_SUCCESS = 15,

  RTM_CONNECTION_CHANGED_LOST = 16,

  RTM_CONNECTION_CHANGED_ECHO_TEST = 17,

  RTM_CONNECTION_CHANGED_CLIENT_IP_ADDRESS_CHANGED_BY_USER = 18,

  RTM_CONNECTION_CHANGED_SAME_UID_LOGIN = 19,

  RTM_CONNECTION_CHANGED_TOO_MANY_BROADCASTERS = 20,
};
```

SDK 连接状态改变原因。

| 枚举值                             | 描述                          |
| ------------------------------------ | ------------------------------------ |
|  `RTM_CONNECTION_CHANGE_CONNECTING`   | 0: 建立网络连接中。 |
|  `RTM_CONNECTION_CHANGE_JOIN_SUCCESS` | 1: 成功加入频道。  |
|  `RTM_CONNECTION_CHANGE_INTERRUPTED`   | 2: 网络连接中断。  |
|  `RTM_CONNECTION_CHANGE_BANNED_BY_SERVER`  | 3: 网络连接被服务器禁止。   |
|  `RTM_CONNECTION_CHANGE_JOIN_FAILED`  |  4: SDK 连续 20 分钟无法加入频道，停止重连频道。  |
|  `RTM_CONNECTION_CHANGE_LEAVE_CHANNEL`  |  5: 离开频道。                            |
|  `RTM_CONNECTION_CHANGE_INVALID_APP_ID` |  6: 不是有效的 APP ID，无法加入频道。    |
|  `RTM_CONNECTION_CHANGE_INVALID_CHANNEL_NAME` |  7: 不是有效的频道名，无法加入频道。     |
|  `RTM_CONNECTION_CHANGE_INVALID_TOKEN`  |   8: Token 无效，无法加入频道。   |
|  `RTM_CONNECTION_CHANGE_TOKEN_EXPIRED` |  9: Token 过期，无法加入频道。     |
|  `RTM_CONNECTION_CHANGE_REJECTED_BY_SERVER`  |  10: 被服务器禁止连接。    |
|  `RTM_CONNECTION_CHANGE_SETTING_PROXY_SERVER` |  11: 由于设置了代理服务器，SDK 尝试重连。   |
|  `RTM_CONNECTION_CHANGE_RENEW_TOKEN`  |   12: 更新 Token 引起网络连接状态改变。    |
|  `RTM_CONNECTION_CHANGE_CLIENT_IP_ADDRESS_CHANGED` |  13: 由于网络类型，或网络运营商的 IP 或端口发生改变，客户端 IP 地址变更，SDK 尝试重连。     |
|  `RTM_CONNECTION_CHANGE_KEEP_ALIVE_TIMEOUT`  | 14: SDK 和服务器连接保活超时，进入自动重连状态。   |
|  `RTM_CONNECTION_CHANGE_REJOIN_SUCCESS`  |  15: SDK 重连成功。    |
|  `RTM_CONNECTION_CHANGE_LOST`  |    16: SDK 丢失与服务器的连接。    |
|  `RTM_CONNECTION_CHANGE_ECHO_TEST`  |   17: 通话回声测试引起连接状态改变。     |
|  `RTM_CONNECTION_CHANGE_CLIENT_IP_ADDRESS_CHANGED_BY_USER`|  18: 用户变更客户端 IP 地址，SDK 尝试重连。  |
|  `RTM_CONNECTION_CHANGE_SAME_UID_LOGIN`  |  19: 使用相同的 UID 从不同的设备加入同一频道。   |
|  `RTM_CONNECTION_CHANGE_TOO_MANY_BROADCASTERS`  |  20: 频道内主播人数已达上限。   |

### RTM_PRESENCE_TYPE

```cpp
enum RTM_PRESENCE_TYPE {

  RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL = 0,

  RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL = 1,

  RTM_PRESENCE_TYPE_REMOTE_CONNECTION_TIMEOUT = 2,

  RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC = 3,

  RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC = 4,

  RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL = 5,
};
```

Presence 类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL`     | 0: 远端用户加入频道。  |
| `RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL`     | 1: 远端用户离开频道。  |
| `RTM_PRESENCE_TYPE_REMOTE_CONNECTION_TIMEOUT`     | 2: 远端用户连接超时。  |
| `RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC`     | 3: 远端用户加入 Topic。  |
| `RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC`     | 4: 远端用户离开 Topic。  |
| `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL`     | 5: 本地用户加入频道，收到该频道中所有 Topic 的信息。  |


