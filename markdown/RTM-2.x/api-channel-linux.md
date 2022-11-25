~8704dfd0-60c9-11ed-8dae-bf25bf08a626~
## 方法

### join
#### 接口描述
```cpp
virtual int join(const JoinChannelOptions& options) = 0;
```

加入频道。

调用该方法会触发 [`onJoinResult`](api-client-linux#onjoinresult) 事件回调。成功加入频道后，SDK 会触发 [`onPresenceEvent`](api-client-linux#onpresenceevent) 事件回调：
- 加入频道的用户本人会收到 `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL` 事件。
- 频道中的其他用户会收到 `RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL` 事件。

> 注意：
> - 单次调用该方法只能加入一个频道，如需加入多个频道，你需要多次调用该方法。单个客户端可以同时加入最多 100 个频道。
> - 用户需要收到成功加入频道的 `onJoinResult` 回调才能继续进行频道相关的操作。

| 参数       | 描述        |
| --------------- | ------------------ |
| `options`       | （选填）加入频道时的配置选项，详见 [`JoinChannelOptions`](#joinchanneloptions)。         |


#### 基本用法 

##### 加入频道
<mark>待补充</mark>

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### getChannelName
#### 接口描述

```cpp
virtual const char* getChannelName() = 0;
```

获取频道名称。

#### 返回值
频道名称。

### leave
#### 接口描述

```cpp
virtual int leave() = 0;
```

离开频道。

调用该方法会触发 [`onLeaveResult`](api-client-linux#onleaveresult) 事件回调。成功加入频道后，频道中的其他用户会收到 [`onPresenceEvent`](api-client-linux#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL` 事件。

#### 基本用法

##### 离开频道
<mark>待补充</mark>

#### 返回值

- `0 `：调用成功。
- < `0 `：调用失败。                                                             

### release
#### 接口描述

```cpp
virtual int release() = 0;
```

销毁一个 `IStreamChannel` 类型实例。

如果你不再需要某个频道，可以调用该方法销毁对应的 `IStreamChannel` 实例以释放资源。调用该方法销毁 `IStreamChannel` 实例不会销毁此频道，后续可通过调用 [`createStreamChannel`](api-client-linux#createstreamchannel) 和 [`join`](#join) 再次加入该频道。

> 注意：如果不先调用 [`leave`](#leave) 离开频道而直接调用 `release` 销毁频道实例，SDK 会自动调用 `leave` 并触发对应的事件回调。

#### 基本用法

##### 离开频道
<mark>待补充</mark>


#### 返回值

- `0`：调用成功。
- <`0`：调用失败。

### joinTopic
#### 接口描述

```cpp
virtual int joinTopic(const char* topic, const JoinTopicOptions& options) = 0;
```

加入一个 Topic。

只有加入 Topic 才能执行发送 Topic 消息的操作。

调用该方法会触发 [`onJoinTopicResult`](api-client-linux#onjointopicresult) 事件回调。成功加入 Topic 后频道中的其他用户会收到 [`onPresenceEvent`](api-client-linux#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC` 事件通知。

> 注意：
> - 请在加入频道后调用该方法。
> - 同一个客户端最多只能同时加入 8 个 Topic，超出数量会报错。

| 参数 | 描述                                                    |
| --------- | ------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic，详见 [Topic 名称](feature-description#topic-%E5%90%8D%E7%A7%B0)。 |
| `options` | （选填）加入 Topic 时的配置选项，详见 [`JoinTopicOptions`](#jointopicoptions)                                             |


#### 基本用法

##### 加入Topic
<mark>待补充</mark>


#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### leaveTopic
#### 接口描述

```cpp
virtual int leaveTopic(const char* topic) = 0;
```

离开一个 Topic。

当客户端加入的 Topic 达到上限时，需要调用 `leaveTopic` 离开某些不再需要的 Topic 以释放资源。

调用该方法会触发 [`onLeaveTopicResult`](api-client-linux#onleavetopicresult) 事件回调。成功离开 Topic 后频道中的其他用户会收到 [`onPresenceEvent`](api-client-linux#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC` 事件通知。


| 参数 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic，详见 [Topic 名称](feature-description#topic-%E5%90%8D%E7%A7%B0)。 |

#### 基本用法 
<mark>待补充</mark>


#### 返回值
- `0`：调用成功。
- < `0`：调用失败。


### publishTopicMessage
#### 接口描述

```cpp
virtual int publishTopicMessage(const char* topic, const char* message, size_t length) = 0;
```

在指定 Topic 中发送文本消息。消息在传输的过程中默认已经被 SSL/TLS 加密，以保证数据链路层安全。

成功调用该方法后，频道中订阅该 Topic 且订阅该消息发布者的用户会收到 [`onMessageEvent`](api-client-linux#onmessageevent) 事件回调。

> 注意：
> - 调用该方法前需先调用 [`joinTopic`](#jointopic) 加入 Topic。
> - 不支持同时向多个 Topic 发送同一条消息。
> - 以下做法可有效提升消息收发的可靠性：
>   - 在调用 `joinTopic` 时可将 `qos` 字段配置为 `RTM_MESSAGE_QOS_ORDERED` 以开启该 Topic 的消息保序能力。除此之外，
>   - 以串行的方式发送消息。
>   - 消息负载不要超过 1 KB，否则发送会失败。
>   - 单个客户端在单个 Topic 中发送消息的速率上限为 60 QPS，如果发送速率超限，将会有部分消息会被丢弃。在满足要求的情况下，速率越低越好。

| 参数 | 描述                                                    |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称代表同一个 Topic，详见 [Topic 名称](feature-description#topic-%E5%90%8D%E7%A7%B0)。 |
| `message` | 消息负载，长度在 1024 字节以内。  |
| `length` | 消息负载长度。  |


#### 基本用法

##### 向 Topic 中发送消息
<mark>待补充</mark>

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。


### subscribeTopic
#### 接口描述

```cpp
virtual int subscribeTopic(const char* topic, const TopicOptions& options) = 0;
```

订阅 Topic 及 Topic 中的消息发送者。

`subscribeTopic` 为增量方法。例如，第一次调用该方法时，订阅消息发布者列表为 `[UserA,UserB]`  , 第二次调用该方法时，订阅消息发布者列表为 `[UserB,UserC]`，则最后成功订阅的结果是 `[UserA，UserB,UserC]`。你可以通过 [`getSubscribedUserList`](#getsubscribeduserlist) 查询当前已经订阅的消息发布者名单列表。

频道中单个 Topic 的消息发布者的数量没有上限，但对于 Topic 订阅者，目前最多只能同时订阅 50 个 Topic，每个 Topic 中最多只能订阅 64 个消息发布者。

如果用户网络连接出现问题，RTM 2.0 将自动尝试重新连接，但在断连期间的消息会丢失。

调用该方法会触发 [`onTopicSubscribed`](api-client-linux#ontopicsubscribed) 回调。


| 参数    | 描述                                                    |
| ------------| --------------------------------------------------------- |
| `topic`      |  Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。 |
| `options`    | （选填）订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-linux#topicoptions)。如果不填写该字段，SDK 将随机订阅该 Topic 中 64 个消息发布者；如果该 Topic 中消息发布者不超过 64 人，则订阅所有消息发布者。                     |


#### 基本用法 

##### 订阅指定用户
<mark>待补充</mark>

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### unsubscribeTopic
#### 接口描述

```cpp
virtual int unsubscribeTopic(const char* topic, const TopicOptions& options) = 0;
```
取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。

调用该方法会触发 [`onTopicUnsubscribed`](api-client-linux#ontopicunsubscribed) 回调。

| 参数    | 描述                                                    |
| ------------|-------------------------------------------------------------- |
| `topic`      | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。 |
| `options`    | （选填）取消订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-linux#topicoptions)。你可以指定想要取消订阅的消息发布者。<ul><li>如果 <code>options</code> 中配置的用户列表不在已订阅的用户名单中，API 会返回正常调用结果，但订阅用户列表不会有任何变化。</li>
<li>如果该字段为空，将取消订阅该 Topic 及取消订阅该 Topic 中所有消息发布者。</li></ul>         |


#### 基本用法 

##### 取消订阅指定用户
<mark>待补充</mark>


##### 取消订阅 Topic 
<mark>待补充</mark>


#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### getSubscribedUserList
#### 接口描述

```cpp
virtual int getSubscribedUserList(const char* topic, UserList* users) = 0;
```

查询指定 Topic 中已订阅的消息发布者列表。

| 参数 | 描述                                                    |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。 |
| `users`   | `UserList` 对象的引用，用于返回已订阅的用户列表。详见  [`UserList`](#userlist)。                                                  |


#### 基本用法 

##### 查询指定 Topic 已订阅消息发布者名单
<mark>待补充</mark>

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

## Struct

### JoinChannelOptions

```cpp
struct JoinChannelOptions {

  const char* token;

  JoinChannelOptions() : token(NULL) {}
};
```

加入频道时的配置选项。

| 参数   |描述      | 
| ------------ |  --------- |
| `token`        | （选填）用于鉴权的 RTM Token。                  |


### UserList

```cpp
struct UserList {

  const char** users;

  size_t userCount;

  UserList() : users(NULL), userCount(0) {}
};
```
用户列表。

| 参数  | 描述                                   |
| --------------------------- |------------------------------------------------------- |
| `users` | 用户 ID 列表。                                                                      |
| `userCount`   | 用户数量。                                                                    |


### JoinTopicOptions


```cpp
struct JoinTopicOptions {

  RTM_MESSAGE_QOS qos;

  const char* meta;

  size_t metaLength;

  JoinTopicOptions() : qos(RTM_MESSAGE_QOS_UNORDERED), meta(NULL), metaLength(0) {}
};
```

加入 Topic 的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `qos` | 指定后续发送 Topic 消息时的 QoS 保障，详见 [`RTM_MESSAGE_QOS`](#rtm_message_qos) 。默认值为 `RTM_MESSAGE_QOS_ORDERED`： 开启消息保序。  |
| `meta`   | 创建 Topic 的辅助信息。  |
| `metaLength`   |  创建 Topic 的辅助信息长度。  |

### TopicOptions


```cpp
struct TopicOptions {

  const char** users;

  size_t userCount;

  TopicOptions() : users(NULL), userCount(0) {}
};
```

订阅或取消订阅 Topic 时的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `users`     | 该 Topic 中想要订阅的消息发布者列表，消息发布者数量不能超过 64 个。    |
| `userCount` | 订阅的消息发布者数量。     |


### TopicInfo

```cpp
struct TopicInfo {

  const char* topic;

  size_t numOfPublisher;

  const char** publisherUserIds;

  const char** publisherMetas;

  TopicInfo() : topic(NULL),
                numOfPublisher(0),
                publisherUserIds(NULL),
                publisherMetas(NULL) {}
};
```

Topic 信息。

| 参数 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic，详见 [Topic 名称](feature-description#topic-%E5%90%8D%E7%A7%B0)。 |
| `numOfPublisher`   | 向该 Topic 发布消息的用户数量。 |
| `publisherUserIds`   | 向该 Topic 发布消息的用户 ID 列表。 |
| `publisherMetas`   | 发布消息的用户的 Topic 辅助信息。 |

## Enum

### STREAM_CHANNEL_ERROR_CODE

```cpp
enum STREAM_CHANNEL_ERROR_CODE {

  STREAM_CHANNEL_ERROR_OK = 0,

  STREAM_CHANNEL_ERROR_EXCEED_LIMITATION = 1,

  STREAM_CHANNEL_ERROR_USER_NOT_EXIST = 2,
};
```

频道事件错误码。

| 枚举值    | 描述      | 
| ------------ | --------- |
| `STREAM_CHANNEL_ERROR_OK`     | 0: 操作成功。  | 
| `STREAM_CHANNEL_ERROR_EXCEED_LIMITATION`     | 1: 订阅用户数量超出限制。  | 
| `STREAM_CHANNEL_ERROR_JOIN_FAILURE`     | 2: 所订阅用户不存在。  | 


### RTM_MESSAGE_QOS

```cpp
enum RTM_MESSAGE_QOS {

  RTM_MESSAGE_QOS_UNORDERED = 0,

  RTM_MESSAGE_QOS_ORDERED = 1,
};
```

消息保序配置。

| 枚举值 |  描述                                                    |
| --------- | -------------------------------------------------------------- |
| `RTM_MESSAGE_QOS_UNORDERED`   | 0: 不开启消息保序。 |
| `RTM_MESSAGE_QOS_ORDERED`   | 1: 开启消息保序。 |



