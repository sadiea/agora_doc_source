~8704dfd0-60c9-11ed-8dae-bf25bf08a626~
## 方法

### join
#### 接口描述
```dart
Future<void> join(JoinChannelOptions options);
```

加入频道。

调用该方法会触发 [`onJoinResult`](api-client-flutter#onjoinresult) 事件回调。成功加入频道后，SDK 会触发 [`onPresenceEvent`](api-client-flutter#onpresenceevent) 事件回调：
- 加入频道的用户本人会收到 `rtmPresenceTypeSelfJoinChannel` 事件。
- 频道中的其他用户会收到 `rtmPresenceTypeRemoteJoinChannel` 事件。

> 注意：
> - 单次调用该方法只能加入一个频道，如需加入多个频道，你需要多次调用该方法。单个客户端可以同时加入最多 100 个频道。
> - 用户需要收到成功加入频道的 `onJoinResult` 回调才能继续进行频道相关的操作。

| 参数       | 描述        |
| --------------- | ------------------ |
| `options`       | （选填）加入频道时的配置选项，详见 [`JoinChannelOptions`](#joinchanneloptions)。         |


#### 基本用法 

##### 加入频道
<mark>待补充</mark>


### getChannelName
#### 接口描述

```dart
Future<String> getChannelName();
```

获取频道名称。

#### 返回值
- 频道名称：调用成功。

### leave
#### 接口描述

```dart
Future<void> leave();
```

离开频道。

调用该方法会触发 [`onLeaveResult`](api-client-flutter#onleaveresult) 事件回调。成功加入频道后，频道中的其他用户会收到 [`onPresenceEvent`](api-client-flutter#onpresenceevent) 中的 `rtmPresenceTypeRemoteLeaveChannel` 事件。

#### 基本用法

##### 离开频道
<mark>待补充</mark>
                                                          

### release
#### 接口描述

```dart
Future<void> release();
```

销毁一个 `StreamChannel` 类型实例。

如果你不再需要某个频道，可以调用该方法销毁对应的 `StreamChannel` 实例以释放资源。调用该方法销毁 `StreamChannel` 实例不会销毁此频道，后续可通过再次调用 [`createStreamChannel`](api-client-flutter#createstreamchannel) 和 [`join`](#join) 重新加入该频道。

> 注意：如果不先调用 [`leave`](#leave) 离开频道而直接调用 `release` 销毁频道实例，SDK 会自动调用 `leave` 并触发对应的事件回调。

#### 基本用法

##### 离开频道
<mark>待补充</mark>


### joinTopic
#### 接口描述

```dart
Future<void> joinTopic(
    {required String topic, required JoinTopicOptions options});
```

加入一个 Topic。

只有加入 Topic 才能执行发送 Topic 消息的操作。

调用该方法会触发 [`onJoinTopicResult`](api-client-flutter#onjointopicresult) 事件回调。成功加入 Topic 后频道中的其他用户会收到 [`onPresenceEvent`](api-client-flutter#onpresenceevent) 中的 `rtmPresenceTypeRemoteJoinTopic` 事件通知。

> 注意：
> - 请在加入频道后调用该方法。
> - 同一个客户端最多只能同时加入 8 个 Topic，超出数量会报错。

| 参数 | 描述                                                    |
| --------- | ------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options` | （选填）加入 Topic 时的配置选项，详见 [`JoinTopicOptions`](#jointopicoptions)                                             |


#### 基本用法

##### 加入Topic
<mark>待补充</mark>


### leaveTopic
#### 接口描述

```dart
Future<void> leaveTopic(String topic);
```

离开一个 Topic。

当客户端加入的 Topic 达到上限时，需要调用 `leaveTopic` 离开某些不再需要的 Topic 以释放资源。

调用该方法会触发 [`onLeaveTopicResult`](api-client-flutter#onleavetopicresult) 事件回调。成功离开 Topic 后频道中的其他用户会收到 [`onPresenceEvent`](api-client-flutter#onpresenceevent) 中的 `rtmPresenceTypeRemoteLeaveTopic` 事件通知。


| 参数 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法 
<mark>待补充</mark>


### publishTopicMessage
#### 接口描述

```dart
Future<void> publishTopicMessage(
    {required String topic, required Uint8List message, required int length});
```

在指定 Topic 中发送文本消息。消息在传输的过程中默认已经被 SSL/TLS 加密，以保证数据链路层安全。

成功调用该方法后，频道中订阅该 Topic 且订阅该消息发布者的用户会收到 [`onMessageEvent`](api-client-flutter#onmessageevent) 事件回调。

> 注意：
> - 调用该方法前需先调用 [`joinTopic`](#jointopic) 加入 Topic。
> - 不支持同时向多个 Topic 发送同一条消息。
> - 以下做法可有效提升消息收发的可靠性：
>   - 在调用 `joinTopic` 时可将 `qos` 字段配置为 `rtmMessageQosOrdered` 以开启该 Topic 的消息保序能力。除此之外，
>   - 以串行的方式发送消息。
>   - 消息负载不要超过 1 KB，否则发送会失败。
>   - 单个客户端在单个 Topic 中发送消息的速率上限为 60 QPS，如果发送速率超限，将会有部分消息会被丢弃。在满足要求的情况下，速率越低越好。

| 参数 | 描述                                                    |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称代表同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `message` | 消息负载，长度在 1024 字节以内。  |
| `length` | 消息负载长度。  |


#### 基本用法

##### 向 Topic 中发送消息
<mark>待补充</mark>


### subscribeTopic
#### 接口描述

```dart
Future<void> subscribeTopic(
    {required String topic, required TopicOptions options});
```

订阅 Topic 及 Topic 中的消息发送者。

`subscribeTopic` 为增量方法。例如，第一次调用该方法时，订阅消息发布者列表为 `[UserA,UserB]`, 第二次调用该方法时，订阅消息发布者列表为 `[UserB,UserC]`，则最后成功订阅的结果是 `[UserA，UserB,UserC]`。你可以通过 [`getSubscribedUserList`](#getsubscribeduserlist) 查询当前已经订阅的消息发布者名单列表。

频道中单个 Topic 的消息发布者的数量没有上限，但对于 Topic 订阅者，目前最多只能同时订阅 50 个 Topic，每个 Topic 中最多只能订阅 64 个消息发布者。

如果用户网络连接出现问题，RTM 2 将自动尝试重新连接，但在断连期间的消息会丢失。

调用该方法会触发 [`onTopicSubscribed`](api-client-flutter#ontopicsubscribed) 回调。


| 参数    | 描述                                                    |
| ------------| --------------------------------------------------------- |
| `topic`      |  Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options`    | （选填）订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-flutter#topicoptions)。如果不填写该字段，SDK 将随机订阅该 Topic 中 64 个消息发布者。                     |


#### 基本用法 

##### 订阅指定用户
<mark>待补充</mark>


### unsubscribeTopic
#### 接口描述

```dart
Future<void> unsubscribeTopic(
    {required String topic, required TopicOptions options});
```
取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。

调用该方法会触发 [`onTopicUnsubscribed`](api-client-flutter#ontopicunsubscribed) 回调。

| 参数    | 描述                                                    |
| ------------|-------------------------------------------------------------- |
| `topic`      | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options`    | （选填）取消订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-flutter#topicoptions)。你可以指定想要取消订阅的消息发布者。<ul><li>如果 <code>options</code> 中配置的用户列表不在已订阅的用户名单中，API 会返回正常调用结果，但订阅用户列表不会有任何变化。</li><li>如果该字段为空，将取消订阅该 Topic 及取消订阅该 Topic 中所有消息发布者。</li></ul>         |


#### 基本用法 

##### 取消订阅指定用户
<mark>待补充</mark>


##### 取消订阅 Topic 
<mark>待补充</mark>


### getSubscribedUserList
#### 接口描述

```dart
Future<UserList> getSubscribedUserList(String topic);
```

查询指定 Topic 中已订阅的消息发布者列表。

| 参数 | 描述                                                    |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |


#### 基本用法 

##### 查询指定 Topic 已订阅消息发布者名单
<mark>待补充</mark>

#### 返回值
- 已订阅的用户列表：调用成功。

## Class

### JoinChannelOptions

```dart
class JoinChannelOptions {

  @JsonKey(name: 'token')
  final String? token;

  factory JoinChannelOptions.fromJson(Map<String, dynamic> json) =>
      _$JoinChannelOptionsFromJson(json);

  Map<String, dynamic> toJson() => _$JoinChannelOptionsToJson(this);
}
```

加入频道时的配置选项。

| 参数   |描述      | 
| ------------ |  --------- |
| `token`        | （选填）用于鉴权的 RTM Token。                  |

### JoinTopicOptions


```dart
class JoinTopicOptions {

  @JsonKey(name: 'qos')
  final RtmMessageQos? qos;

  @JsonKey(name: 'meta', ignore: true)
  final Uint8List? meta;

  @JsonKey(name: 'metaLength')
  final int? metaLength;

  factory JoinTopicOptions.fromJson(Map<String, dynamic> json) =>
      _$JoinTopicOptionsFromJson(json);

  Map<String, dynamic> toJson() => _$JoinTopicOptionsToJson(this);
}
```

加入 Topic 的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `qos` | 指定后续发送 Topic 消息时的 QoS 保障，详见 [`RtmMessageQos`](#rtmmessageqos) 。默认值为 `rtmMessageQosOrdered`： 开启消息保序。  |
| `meta`   | （选填）Topic 的元数据。  |
| `metaLength`   |  （选填）Topic 的元数据的长度。  |

### TopicOptions


```dart
class TopicOptions {

  @JsonKey(name: 'users')
  final List<String>? users;

  @JsonKey(name: 'userCount')
  final int? userCount;

  factory TopicOptions.fromJson(Map<String, dynamic> json) =>
      _$TopicOptionsFromJson(json);
c
  Map<String, dynamic> toJson() => _$TopicOptionsToJson(this);
}
```

订阅或取消订阅 Topic 时的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `users`     | （选填）该 Topic 中想要订阅的消息发布者列表，消息发布者数量不能超过 64 个。    |
| `userCount` | （选填）订阅的消息发布者数量。     |


### UserList

```dart
class UserList {

  @JsonKey(name: 'users')
  final List<String>? users;

  @JsonKey(name: 'userCount')
  final int? userCount;

  factory UserList.fromJson(Map<String, dynamic> json) =>
      _$UserListFromJson(json);

  Map<String, dynamic> toJson() => _$UserListToJson(this);
}
```
用户列表。

| 参数  | 描述                                   |
| --------------------------- |------------------------------------------------------- |
| `users` | 用户 ID 列表。                                                                      |
| `userCount`   | 用户数量。                                                                    |

## Enum

### RtmMessageQos

```dart
enum RtmMessageQos {
  @JsonValue(0)
  rtmMessageQosUnordered,

  @JsonValue(1)
  rtmMessageQosOrdered,
}
```

消息保序配置。

| 枚举值 |  描述                                                    |
| --------- | -------------------------------------------------------------- |
| `rtmMessageQosUnordered`   | 0: 不开启消息保序。 |
| `rtmMessageQosOrdered`   | 1: 开启消息保序。 |



