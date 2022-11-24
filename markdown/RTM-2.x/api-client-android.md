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

创建一个 `RtmClient` 实例。

#### 返回值
- 一个 `RtmClient` 对象：调用成功。
- 空：调用失败。

### initialize
#### 接口描述

```java
public abstract int initialize(RtmConfig config);
```

初始化 `RtmClient` 实例。

> 注意：初始化实例必须在你调用 [create](#create) 之后，使用 RTM 其他功能之前完成，以建立正确的账号级别的凭据（例如 APP ID）。


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

销毁一个 `RtmClient` 类型实例以释放资源。


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

创建`StreamChannel` 实例后你可以调用其他频道相关方法，详见 [StreamChannel 类](api-channel-android)。

| 参数  | 描述                    |
| --------- | ------------------------------ |
| `channelName` | 频道名称，命名限制详见[频道名称](feature-description#%E9%A2%91%E9%81%93%E5%90%8D%E7%A7%B0)。 |

#### 基本用法
<mark>待补充</mark>

#### 返回值
- 一个 `StreamChannel` 实例：调用成功。
- 空：调用失败。




## 回调

### IRtmEventHandler  
通过添加事件监听处理程序以获得接口调用结果以及事件通知，包括连接状态，消息到达，Presence 状态等事件通知以及监控接口回调结果。如需要在 App 中接收消息和事件通知，在调用这些函数前必须先添加事件监听处理程序。

### onMessageEvent
#### 接口描述

```java
@CalledByNative public void onMessageEvent(MessageEvent event);
```

当你订阅的 Topic 中订阅的用户发布消息时，会触发该回调。

| 参数   | 描述      | 
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`MessageEvent`](#messageevent)。  | 

### onPresenceEvent
#### 接口描述

```java
@CalledByNative public void onPresenceEvent(PresenceEvent event);
```

当频道中有用户的 Presence 状态发生变更时会触发该回调。比如，远端用户加入或离开频道，同一频道内远端用户加入或离开 Topic，本地用户加入频道时收到 Topic 消息。


| 参数   | 描述      | 
| ------------ | --------- |
| `event`  | 消息事件类型，详见 [`PresenceEvent`](#presenceevent)。  | 


### onJoinResult
#### 接口描述

```java
@CalledByNative public void onJoinResult(String channelName, String userId, int errorCode);
```

加入频道时会触发该回调。

| 参数      | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`     | 加入频道用户的 ID。  |
| `errorCode`   | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。       |

### onLeaveResult
#### 接口描述

```java
@CalledByNative public void onLeaveResult(String channelName, String userId, int errorCode);
```

离开频道时触发该回调。

| 参数    | 描述                              |
| -------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开频道用户的 User ID。  |
| `errorCode`   | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。       |

### onJoinTopicResult
#### 接口描述

```java
  @CalledByNative
  public void onJoinTopicResult(
      String channelName, String userId, String topicName, String meta, int errorCode);
```

加入 Topic 时会触发该回调。

| 参数    | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`     | 加入 Topic 用户的 User ID。  |
| `topicName`   | Topic 名称。       |
| `meta`        | 创建 Topic 的辅助信息。  |
| `errorCode`   | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。       |

#### onLeaveTopicResult
##### 接口描述

```java
  @CalledByNative
  public void onLeaveTopicResult(
      String channelName, String userId, String topicName, String meta, int errorCode);
```

离开 Topic 时会触发该回调。

| 参数   | 描述                              |
| ------------- | ---------------------------------------- |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 离开 Topic 用户的 User ID。  |
| `topicName`   | Topic 名称。       |
| `meta`        | 离开 Topic 的辅助信息。  |
| `errorCode`   | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。       |

### onTopicSubscribed
#### 接口描述

```java
  @CalledByNative
  public void onTopicSubscribed(String channelName, String userId, String topicName,
      UserList successUsers, UserList failedUsers, int errorCode);
```

订阅 Topic 或订阅 Topic 中发布消息的用户时会触发该回调。

| 参数      | 描述                                |
| -------------- | ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 订阅 Topic 用户的 User ID。  |
| `topicName`   | 事件所在 Topic 名称。 |
| `successUsers` | 本次操作成功的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `failedUsers`  | 本次操作失败的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `errorCode`    | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。 

### onTopicUnsubscribed
#### 接口描述

```java
  @CalledByNative
  public void onTopicUnsubscribed(String channelName, String userId, String topicName,
      UserList successUsers, UserList failedUsers, int errorCode);
```

取消订阅 Topic 或取消订阅 Topic 中发布消息的用户时会触发该回调。

| 参数       | 描述                                |
| --------------| ------------------------------------------ |
| `channelName` | 事件所在频道名称。 |
| `userId`      | 取消订阅 Topic 用户的 User ID。  |
| `topicName`   | 事件所在 Topic 名称。 |
| `successUsers` | 本次操作成功的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `failedUsers`  | 本次操作失败的消息发布者列表，详见 [`UserList`](api-channel-android#userlist)。                       |
| `errorCode`    | 频道错误码，详见[`STREAM_CHANNEL_ERROR_CODE`](api-channel-android#stream_channel_error_code)。 


### onConnectionStateChange
#### 接口描述

```java
@CalledByNative public void onConnectionStateChange(String channelName, int state, int reason);
```

SDK 连接状态发生改变时会触发该回调。

| 参数   | 描述      | 
| ------------ | --------- |
| `channelName` | 事件所在频道名称。 |
| `state`  | SDK 连接状态，详见 [`RTM_CONNECTION_STATE`](#rtmconnectionstate)。  | 
| `reason` | SDK 连接状态改变的原因，详见 [`RTM_CONNECTION_CHANGE_REASON`](#rtmconnectionchangereason)。  | 

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
```

`RtmClient` 实例的配置信息。

该接口用于存储配置信息，这些信息将影响后续 RTM 客户端的行为。

| 参数   | 描述           |
| ------------ | ----------------------------------- |
| `mAppId`        | 从声网控制台上获取的 APP ID。                                                        |
| `mUserId`       | 用户 ID，用户或设备设置唯一的标识符。你需要维护 `mUserId` 和用户之间的映射关系，并在整个服务周期内不能改变该映射关系。如果不设置该参数，将无法连接到 RTM 服务。                               |
| `mEventHandler` | 事件监听函数句柄，用以监听消息通知，Presence 通知，状态变更通知等事件通知。详见 [`IRtmEventHandler`](#irtmeventhandler)。                               |


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
| `topicName`  | Topic 名称。  | 
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
| `channelType`     | 频道类型，详见 [`RtmChannelType`](#rtmchanneltype)。  | 
| `type`     | Presence 类型，详见 [`RtmPresenceType`](#rtmpresencetype)。  | 
| `channelName`     | 频道名称。  | 
| `topicInfos`    | Topic 信息，详见 [TopicInfo](api-channel-android#topicinfo)。  | 
| `userId`    | 触发 Presence 事件用户的 User ID。  | 

## Enum

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
| `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL`     | 5: 本地用户加入频道。  | 






