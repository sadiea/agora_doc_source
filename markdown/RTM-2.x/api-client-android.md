`RtmClient` ~03f2af90-60ca-11ed-8dae-bf25bf08a626~
## 方法

### create
#### 接口描述

```java
  public static synchronized RtmClient create() throws Exception {
    if (!RtmClientImpl.initializeNativeLibs()) {
      return null;
    }
    if (mInstance == null) {
      mInstance = new RtmClientImpl();
    }
    return mInstance;
  }
```

创建一个 `RtmClient` 单实例。

> 注意：
> - 在调用 `RtmClient` 类的方法之前，你需要调用该方法创建 `RtmClient` 单实例。
> - 在调用 [`release`](#release) 销毁 `RtmClient` 单实例之前，多次调用 `create` 获取的是同一个 `RtmClient` 单实例。Agora 不推荐多次调用 `create`。

#### 返回值
- 一个 `RtmClient` 对象：调用成功。
- `null`：调用失败。

### initialize
#### 接口描述

```java
public abstract int initialize(RtmConfig config);
```

初始化 `RtmClient` 单实例。

> 注意：初始化实例必须在你调用 [`create`](#create) 之后，使用 RTM 其他功能之前完成，以建立正确的账号级别的凭据（例如 APP ID）。


| 参数  | 描述                    |
| --------- | ------------------------------ |
| `config` | 该 `RtmClient` 实例的配置信息，详见 [`RtmConfig`](#rtmconfig)。 |

#### 基本用法

#### 初始化 RTM 实例
<mark>待补充</mark>

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### release
#### 接口描述

```java
  public static synchronized void release() {
    if (mInstance == null) {
      return;
    }
    mInstance.releaseClient();
    mInstance = null;
  }
```

销毁一个 `RtmClient` 单实例以释放资源。

成功销毁后，如需调用 `RtmClient` 类的 API，你需要调用 [`create`](#create) 重新创建 `RtmClient` 单实例。

#### 基本用法
<mark>待补充</mark>

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。


### createStreamChannel
#### 接口描述

```java
public abstract StreamChannel createStreamChannel(@NonNull String channelName);
```

创建一个 `StreamChannel` 实例。

~8704dfd0-60c9-11ed-8dae-bf25bf08a626~

创建 `StreamChannel` 实例后你可以调用其他频道相关方法，详见 [StreamChannel 类](api-channel-android)。

| 参数  | 描述                    |
| --------- | ------------------------------ |
| `channelName` | 频道名称。~23141e20-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法
<mark>待补充</mark>

#### 返回值
- 一个 `StreamChannel` 实例：调用成功。
- `null`：调用失败。




## 回调

### IRtmEventHandler
通过添加事件监听处理程序以获方法调用结果以及事件通知，包括连接状态，消息到达，Presence 状态等事件通知以及监控方法回调结果。如需要在 App 中接收消息和事件通知，在调用这些函数前必须先添加事件监听处理程序。


### onJoinResult
#### 接口描述

```java
@CalledByNative public void onJoinResult(String channelName, String userId, int errorCode);
```

[join](api-channel-android#join) 调用的异步回调，在加入频道时触发。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`     | 加入频道用户的 ID。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onLeaveResult
#### 接口描述

```java
@CalledByNative public void onLeaveResult(String channelName, String userId, int errorCode);
```

[`leave`](api-channel-android#leave) 调用的异步回调，在离开频道时触发。

| 参数    | 描述                              |
| -------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onJoinTopicResult
#### 接口描述

```java
  @CalledByNative
  public void onJoinTopicResult(
      String channelName, String userId, String topicName, String meta, int errorCode);
```

[`joinTopic`](api-channel-android#jointopic) 调用的异步回调，在加入 Topic 时触发。

| 参数    | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`     | 加入 Topic 用户的 User ID。  |
| `topicName`   | Topic 名称。       |
| `meta`        | 创建 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onLeaveTopicResult
#### 接口描述

```java
  @CalledByNative
  public void onLeaveTopicResult(
      String channelName, String userId, String topicName, String meta, int errorCode);
```

[`leaveTopic`](api-channel-android#leavetopic) 调用的异步回调，在离开 Topic 时触发。

| 参数   | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开 Topic 用户的 User ID。  |
| `topicName`   | Topic 名称。       |
| `meta`        | 离开 Topic 的元数据。  |
| `errorCode`   | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。       |

### onTopicSubscribed
#### 接口描述

```java
  @CalledByNative
  public void onTopicSubscribed(String channelName, String userId, String topicName,
      UserList successUsers, UserList failedUsers, int errorCode);
```

[`subscribeTopic`](api-channel-android#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。

| 参数      | 描述                                |
| -------------- | ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 订阅 Topic 用户的 User ID。  |
| `topicName`   | 事件所在 Topic 名称。 |
| `successUsers` | 本次订阅成功的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `failedUsers`  | 本次订阅失败的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。订阅失败的原因可能是超过了订阅数量限制。                       |
| `errorCode`    | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。

### onTopicUnsubscribed
#### 接口描述

```java
  @CalledByNative
  public void onTopicUnsubscribed(String channelName, String userId, String topicName,
      UserList successUsers, UserList failedUsers, int errorCode);
```

[`unsubscribeTopic`](api-channel-android#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 取消订阅 Topic 用户的 User ID。  |
| `topicName`   | 事件所在 Topic 名称。 |
| `successUsers` | 本次取消订阅成功的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `failedUsers`  | 本次取消订阅失败的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。取消订阅失败的原因可能是之前未订阅过该用户。                       |
| `errorCode`    | 频道错误码，详见 [`StreamChannelErrorCode`](#streamchannelerrorcode)。


## 事件通知

### onConnectionStateChange
#### 接口描述

```java
@CalledByNative public void onConnectionStateChange(String channelName, int state, int reason);
```

SDK 连接状态发生改变时会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `channelName` | 事件所在频道名称。 |
| `state`  | SDK 连接状态，详见 [`RtmConnectionState`](#rtmconnectionstate)。  |
| `reason` | SDK 连接状态改变的原因：<li>`CONNECTION_CHANGED_CONNECTING (0)`：建立网络连接中 。</li><li>`CONNECTION_CHANGED_JOIN_SUCCESS (1)`：成功加入频道 。</li><li>`CONNECTION_CHANGED_INTERRUPTED (2)`：网络连接中断 。</li><li>`CONNECTION_CHANGED_BANNED_BY_SERVER (3)`：网络连接被服务器禁止 。</li><li>`CONNECTION_CHANGED_JOIN_FAILED (4)`：加入频道失败 。</li><li>`CONNECTION_CHANGED_LEAVE_CHANNEL (5)`：离开频道 。</li><li>`CONNECTION_CHANGED_INVALID_APP_ID (6)`：不是有效的 APP ID。请更换有效的 APP ID 重新加入频道 。</li><li>`CONNECTION_CHANGED_INVALID_CHANNEL_NAME (7)`：不是有效的频道名。请更换有效的频道名重新加入频道。</li><li>`CONNECTION_CHANGED_INVALID_TOKEN(8)`：生成的 Token 无效。</li><li>`CONNECTION_CHANGED_TOKEN_EXPIRED (9)`：当前使用的 Token 过期，不再有效，需要重新在你的服务端申请生成 Token 。</li><li>`CONNECTION_CHANGED_REJECTED_BY_SERVER (10)`：此用户被服务器禁止 。</li><li>`CONNECTION_CHANGED_SETTING_PROXY_SERVER (11)`：由于设置了代理服务器，SDK 尝试重连 。</li><li>`CONNECTION_CHANGED_RENEW_TOKEN (12)`：更新 Token 引起网络连接状态改变 。</li><li>`CONNECTION_CHANGED_CLIENT_IP_ADDRESS_CHANGED (13)`：客户端 IP 地址变更，可能是由于网络类型，或网络运营商的 IP 或端口发生改变引起 。</li><li>`CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT (14)`：SDK 和服务器连接保活超时，进入自动重连状态 。</li><li>`CONNECTION_CHANGED_REJOIN_SUCCESS (15)`：重新加入频道成功。</li><li>`CONNECTION_CHANGED_LOST (16)`：SDK 和服务器失去连接 。</li><li>`CONNECTION_CHANGED_ECHO_TEST (17)`：连接状态变化由回声测试引起。</li>  |

### onPresenceEvent
#### 接口描述

```java
@CalledByNative public void onPresenceEvent(PresenceEvent event);
```

当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。

你可以根据实际场景监听需要的 Presence 类型：

- 频道相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）：
    - `RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL(0)`：远端用户加入频道。
    - `RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL(1)`：远端用户离开频道。
    - `RTM_PRESENCE_TYPE_REMOTE_CONNECTION_TIMEOUT(2)`：远端用户连接超时。

- Topic 相关类型，你可以在该类型中获取频道类型（`channelType`）、频道名（`channelName`）、用户 ID（`userId`）、Topic 信息（`topicInfos`）：
    - `RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3)`：远端用户加入 Topic。
    - `RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4)`：远端用户离开 Topic。
    - `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5)`：本地用户加入频道，收到该频道中所有 Topic 的信息。

| 参数   | 描述      |
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`PresenceEvent`](#presenceevent)。  |


### onMessageEvent
#### 接口描述

```java
@CalledByNative public void onMessageEvent(MessageEvent event);
```

在不同频道类型中，SDK 发送该事件通知的情况不同。具体如下：
- 在 Message Channel 中，当你在频道中订阅的用户发布消息时，SDK 会发送该事件通知。
- 在 Stream Channel 中，当你在 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。

| 参数   | 描述      |
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`MessageEvent`](#messageevent)。  |


## Class

### RtmConnectionState

```java
public static class RtmConnectionState {

    public static final int RTM_CONNECTION_STATE_DISCONNECTED = 1;

    public static final int RTM_CONNECTION_STATE_CONNECTING = 2;

    public static final int RTM_CONNECTION_STATE_CONNECTED = 3;

    public static final int RTM_CONNECTION_STATE_RECONNECTING = 4;

    public static final int RTM_CONNECTION_STATE_FAILED = 5;
}
```

SDK 连接状态。

| 参数  | 描述                                                                                                |
| ----------------------------------- | -------------------------------------------------- |
| `RTM_CONNECTION_STATE_DISCONNECTED` | 1: SDK 已和服务器断开连接。                                                                      |
| `RTM_CONNECTION_STATE_CONNECTING`   | 2: SDK 正在连接服务器。                                                                    |
| `RTM_CONNECTION_STATE_CONNECTED`    | 3: SDK 已连上服务器。                                                                           |
| `RTM_CONNECTION_STATE_RECONNECTING` | 4: SDK 和服务器断开连接，正在重新连接服务器。 |
| `RTM_CONNECTION_STATE_FAILED`       | 5: SDK 无法连接服务器。


### RtmConfig

```java
public class RtmConfig {

  public String mAppId;

  public String mUserId;

  public IRtmEventHandler mEventHandler;
}
```

`RtmClient` 实例的配置信息，用于存储配置信息，这些信息将影响后续 RTM 客户端的行为。

| 参数   | 描述           |
| ------------ | ----------------------------------- |
| `mAppId`        | 从声网控制台上获取的 APP ID。                                                        |
| `mUserId`       | 用户 ID，用户或设备设置唯一的标识符。你需要维护 `mUserId` 和用户之间的映射关系，并在整个服务周期内不能改变该映射关系。如果不设置该参数，将无法连接到 RTM 服务。                               |
| `mEventHandler` | 事件监听函数句柄，用以监听消息通知，Presence 通知，状态变更通知等事件通知。详见 [`IRtmEventHandler`](#irtmeventhandler)。                               |


#### 基本用法
<mark>待补充</mark>

### Logconfig

```java
public static class LogConfig {

  public String filePath;

  public int fileSizeInKB;

  public int level = RtmConstants.LogLevel.getValue(RtmConstants.LogLevel.LOG_LEVEL_INFO);

}
```

开启日志功能，并设置日志保存路径、日志大小及日志记录等级等，为后续的问题调查收集必要的运行数据。

| 参数    | 描述                             |
| ------------ | -------------------------- |
| `filePath`     | （选填）日志保存路径及日志文件名。请确保对配置的文件路径具备读写权限。默认值为 <code>/storage/emulated/0/Android/data/{packagename}/files/agorasdk.log</code>。    |
| `fileSizeInKB` | （选填）日志文件大小，单位为 KB，取值范围为 [128,1024]，默认值为 1024。如果你将该参数设为小于 128 的值，则使用 128；如果你将该参数设为大于 1024 的值，则使用 1024。                                |
| `level`        |（选填）日志错误等级，默认值为 `LogLevel.LOG_LEVEL_INFO`。 |

#### 基本用法
<mark>待补充</mark>

### MessageEvent

```java
public class MessageEvent {

  public RtmChannelType channelType;

  public String channelName;

  public String topicName;

  public byte[] message;

  public String publisher;

}
```

消息回调事件。

| 参数   | 描述      |
| ------------ | --------- |
| `channelType`  | 频道类型，详见 [`RtmChannelType`](#rtmchanneltype)。  |
| `channelName`   | 频道名称。  |
| `topicName`  | Topic 名称。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>RTM_CHANNEL_TYPE_STREAM(1)</code> 时生效。</div>  |
| `message`   | 消息负载。  |
| `publisher`   | 发布消息用户的 User ID。  |

### PresenceEvent

```java
public class PresenceEvent {

  public RtmChannelType channelType;

  public RtmPresenceType presenceType;

  public String channelName;

  public TopicInfo[] topicInfos;

  public String userId;

}
```

Presence 回调事件。

| 参数    | 描述      |
| ------------ | --------- |
| `presenceType`     | Presence 类型，详见 [`RtmPresenceType`](#rtmpresencetype)。  |
| `channelType`     | 频道类型，详见 [`RtmChannelType`](#rtmchanneltype)。  |
| `channelName`     | 频道名称。  |
| `topicInfos`    | Topic 信息，详见 [`TopicInfo`](#topicinfo)。<div class="alert info">该参数仅在 <code>channelType</code> 为 <code>RTM_CHANNEL_TYPE_STREAM(1)</code> 且 <code>type</code> 为 <code>RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3)</code>、<code>RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4)</code> 或 <code>RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5)</code> 时生效。  |
| `userId`    | 触发 Presence 事件用户的 User ID。  |

### TopicInfo

```java
public class TopicInfo {

  public String topicName;

  public ArrayList<String> publisherUserIds;

  public ArrayList<String> publisherMetas;
}
```

Topic 信息。

| 参数 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topicName`   |Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `publisherUserIds`   | 向该 Topic 发布消息的用户 ID 列表。 |
| `publisherMetas`   | 向该 Topic 发布消息的用户的元数据列表。 |

## Enum

### StreamChannelErrorCode

频道错误码。

```java
public static final int STREAM_CHANNEL_ERROR_OK = 0;

public static final int STREAM_CHANNEL_ERROR_EXCEED_LIMITATION = 1;

public static final int STREAM_CHANNEL_ERROR_USER_NOT_EXIST = 2;
```

| 枚举值    | 描述      | 
| ------------ | --------- |
| `STREAM_CHANNEL_ERROR_OK`     | 0: 操作成功。  | 
| `STREAM_CHANNEL_ERROR_EXCEED_LIMITATION`     | 1: 订阅用户数量超出限制。  | 
| `STREAM_CHANNEL_ERROR_JOIN_FAILURE`     | 2: 所订阅用户不存在。  | 

### RtmChannelType

```java
public enum RtmChannelType {

    RTM_CHANNEL_TYPE_MESSAGE(0),

    RTM_CHANNEL_TYPE_STREAM(1);
}
```

RTM 频道类型。

| 枚举值    | 描述      |
| ------------ | --------- |
| `RTM_CHANNEL_TYPE_MESSAGE`     | 0: Message Channel。  |
| `RTM_CHANNEL_TYPE_STREAM`     | 1: Stream Channel。  |

### RtmPresenceType

```java
public enum RtmPresenceType {

    RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL(0),

    RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL(1),

    RTM_PRESENCE_TYPE_REMOTE_CONNECTION_TIMEOUT(2),

    RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC(3),

    RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC(4),

    RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL(5);
}
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







