~8704dfd0-60c9-11ed-8dae-bf25bf08a626~

## 方法

### Join
#### 接口描述

```csharp
public abstract int Join(JoinChannelOptions options);
```

加入频道。

调用该方法会触发 [`OnJoinResult`](api-client-unity#onjoinresult) 事件回调。成功加入频道后，SDK 会触发 [`OnPresenceEvent`](api-client-unity#onpresenceevent) 事件回调：
- 加入频道的用户本人会收到 `RTM_PRESENCE_TYPE_SELF_JOIN_CHANNEL` 事件。
- 频道中的其他用户会收到 `RTM_PRESENCE_TYPE_REMOTE_JOIN_CHANNEL` 事件。

> 注意：
> - 单次调用该方法只能加入一个频道，如需加入多个频道，你需要多次调用该方法。单个客户端可以同时加入最多 100 个频道。
> - 用户需要收到成功加入频道的 `OnJoinResult` 回调才能继续进行频道相关的操作。


| 参数       | 描述        |
| --------------- | ------------------ |
| `options`       | （选填）加入频道时的配置选项，详见 [`JoinChannelOptions`](#joinchanneloptions)。         |


#### 基本用法

##### 加入频道
```csharp
loc_stChannel = rtmClient.CreateStreamChannel("Location");
// 创建一个 JoinChannelOptions 示例。
JoinChannelOptions joinChannelOptions =new JoinChannelOptions() ;
// 订阅他人的 Presence 事件。
joinChannelOptions.withPresence = true;
// 订阅频道属性。
joinChannelOptions.withMetaData = true;
// 加入频道名为 Location 的频道。
loc_stChannel.Join(joinChannelOptions);
```

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### GetChannelName
#### 接口描述

```csharp
public abstract string GetChannelName();
```

获取频道名称。


#### 返回值
频道名称。

### Leave
#### 接口描述

```csharp
public abstract int Leave();
```

离开频道。

调用该方法会触发 [`OnLeaveResult`](api-client-unity#onleaveresult) 事件回调。成功加入频道后，频道中的其他用户会收到 [`OnPresenceEvent`](api-client-unity#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_LEAVE_CHANNEL` 事件。

#### 基本用法

##### 离开频道
```csharp
loc_stChannel = rtmClient.CreateStreamChannel("Location");
// 创建一个 JoinChannelOptions 示例。
JoinChannelOptions joinChannelOptions =new JoinChannelOptions() ;
// 订阅他人的 Presence 事件。
joinChannelOptions.withPresence = true;
// 订阅频道属性。
joinChannelOptions.withMetaData = true;
// 加入频道名为 Location 的频道。
loc_stChannel.Join(joinChannelOptions);
// 在这里加入你的实现代码。
// 离开 Location 频道。
loc_stChannel.Leave();
```

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。                                                             

### Release
#### 接口描述

```csharp
public abstract int Release();
```
销毁一个 `IStreamChannel` 类型实例。

如果你不再需要某个频道，可以调用该方法销毁对应的 `IStreamChannel` 实例以释放资源。调用该方法销毁 `IStreamChannel` 实例不会销毁此频道，后续可通过再次调用 [`CreateStreamChannel`](api-client-unity#createstreamchannel) 和 [`Join`](#join) 重新加入该频道。

> 注意：如果不先调用 [`Leave`](#leave) 离开频道而直接调用 `Release` 销毁频道实例，SDK 会自动调用 `Leave` 并触发对应的事件回调。

#### 基本用法

##### 离开频道

```csharp
loc_stChannel = rtmClient.CreateStreamChannel("Location");
// 创建一个 JoinChannelOptions 示例。
JoinChannelOptions joinChannelOptions =new JoinChannelOptions() ;
// 订阅他人的 Presence 事件。
joinChannelOptions.withPresence = true;
// 订阅频道属性。
joinChannelOptions.withMetaData = true;
// 加入频道名为 Location 的频道。
loc_stChannel.Join(joinChannelOptions);
// 在这里加入你的实现代码。
// 离开 Location 频道。
loc_stChannel.Leave();
// 销毁 Location 频道对应的 `IStreamChannel` 实例。
loc_stChannel.Release();
```

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### JoinTopic
#### 接口描述

```csharp
public abstract int JoinTopic(string topic, JoinTopicOptions options);
```

加入一个 Topic。

只有加入 Topic 才能执行发送 Topic 消息的操作。

调用该方法会触发 [`OnJoinTopicResult`](api-client-unity#onjointopicresult) 事件回调。成功加入 Topic 后频道中的其他用户会收到 [`onPresenceEvent`](api-client-unity#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_JOIN_TOPIC` 事件通知。

> 注意：
> - 请在加入频道后调用该方法。
> - 同一个客户端最多只能同时加入 8 个 Topic，超出数量会报错。


| 参数 |描述                                                    |
| --------- | ---------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options` | （选填）加入 Topic 时的配置选项，详见 [`JoinTopicOptions`](#jointopicoptions)                                             |


#### 基本用法

##### 加入Topic

```csharp
// 创建一个 JoinTopicOptions 实例。
JoinTopicOptions joinTopicOptions = new JoinTopicOptions() ;
// QoS 保障设置为消息保序。
joinTopicOptions.qos = RTM_MESSAGE_QOS.RTM_MESSAGE_QOS_ORDERED ;
// 加入名为 gesture 的 Topic。
loc_stChannel.JoinTopic( "gesture",joinTopicOptions) ;
```


#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### LeaveTopic
#### 接口描述

```csharp
public abstract int LeaveTopic(string topic);
```

离开一个 Topic。

当客户端加入的 Topic 达到上限时，需要调用 `LeaveTopic` 离开某些不再需要的 Topic 以释放资源。

调用该方法会触发 [`OnLeaveTopicResult`](api-client-unity#onleavetopicresult) 事件回调。成功离开 Topic 后频道中的其他用户会收到 [`OnPresenceEvent`](api-client-unity#onpresenceevent) 中的 `RTM_PRESENCE_TYPE_REMOTE_LEAVE_TOPIC` 事件通知。


| 参数 |描述                                                    |
| ---------  | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |

#### 基本用法

```csharp
// 离开名为 gesture 的 Topic。
loc_stChannel.LeaveTopic( "gesture") ;
```

#### 返回值
- `0 `：调用成功。
- < `0 `：调用失败。

### PublishTopicMessage[1/2]
#### 接口描述

```csharp
public abstract int PublishTopicMessage(string topic, string message);
```

在指定 Topic 中发送文本消息。消息在传输的过程中默认已经被 SSL/TLS 加密，以保证数据链路层安全。

成功调用该方法后，频道中订阅该 Topic 且订阅该消息发布者的用户会收到 [`OnMessageEvent`](api-client-unity#onmessageevent) 事件回调。

> 注意：
> - 调用该方法前需先调用 [`JoinTopic`](#jointopic) 加入 Topic。
> - 不支持同时向多个 Topic 发送同一条消息。
> - 以下做法可有效提升消息收发的可靠性：
>   - 在调用 `JoinTopic` 时可将 `qos` 字段配置为 `RTM_MESSAGE_QOS_ORDERED` 以开启该 Topic 的消息保序能力。除此之外，
>   - 以串行的方式发送消息。
>   - 消息负载不要超过 1 KB，否则发送会失败。
>   - 单个客户端在单个 Topic 中发送消息的速率上限为 60 QPS，如果发送速率超限，将会有部分消息会被丢弃。在满足要求的情况下，速率越低越好。


| 参数 | 描述                                                   |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称代表同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `message` |  消息负载，长度在 1024 字节以内。  |


#### 基本用法

##### 向 Topic 中发送消息
```csharp
// 创建一个 JoinTopicOptions 实例。
JoinTopicOptions joinTopicOptions = new JoinTopicOptions() ;
// 配置 QoS 保障。
joinTopicOptions.qos = RTM_MESSAGE_QOS.RTM_MESSAGE_QOS_ORDERED ;
// 加入名为 gesture 的 Topic。
loc_stChannel.JoinTopic( "gesture",joinTopicOptions) ;
```

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### PublishTopicMessage[2/2]
#### 接口描述

```csharp
public abstract int PublishTopicMessage(string topic, byte[] message);
```

在指定 Topic 中发送二进制消息。消息在传输的过程中默认已经被 SSL/TLS 加密，以保证数据链路层安全。

成功调用该方法后，频道中订阅该 Topic 且订阅该消息发布者的用户会收到 [`onMessageEvent`](api-client-unity#onmessageevent) 事件回调。

> 注意：
> - 调用该方法前需先调用 [`JoinTopic`](#jointopic) 加入 Topic。
> - 不支持同时向多个 Topic 发送同一条消息。
> - 以下做法可有效提升消息收发的可靠性：
>   - 在调用 `JoinTopic` 时可将 `qos` 字段配置为 `RTM_MESSAGE_QOS_ORDERED` 以开启该 Topic 的消息保序能力。除此之外，
>   - 以串行的方式发送消息。
>   - 消息负载不要超过 1 KB，否则发送会失败。
>   - 单个客户端在单个 Topic 中发送消息的速率上限为 60 QPS，如果发送速率超限，将会有部分消息会被丢弃。在满足要求的情况下，速率越低越好。

| 参数 | 描述                          |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称代表同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `message` | 消息负载，长度在 1024 字节以内。  |


#### 基本用法
##### 向 Topic 中发送消息
```csharp
// 创建一个 JoinTopicOptions 实例。
JoinTopicOptions joinTopicOptions = new JoinTopicOptions() ;
// 配置 QoS 保障。
joinTopicOptions.qos = RTM_MESSAGE_QOS.RTM_MESSAGE_QOS_ORDERED ;
// 加入名为 gesture 的 Topic。
loc_stChannel.JoinTopic( "gesture",joinTopicOptions) ;
```
#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### SubscribeTopic
#### 接口描述

```csharp
public abstract int SubscribeTopic(string topic, TopicOptions options);
```

订阅 Topic 及 Topic 中的消息发送者。

`SubscribeTopic` 为增量方法。例如，第一次调用该方法时，订阅消息发布者列表为 `[UserA,UserB]`, 第二次调用该方法时，订阅消息发布者列表为 `[UserB,UserC]`，则最后成功订阅的结果是 `[UserA，UserB,UserC]`。你可以通过 [`GetSubscribedUserList`](#getsubscribeduserlist) 查询当前已经订阅的消息发布者名单列表。

频道中单个 Topic 的消息发布者的数量没有上限，但对于 Topic 订阅者，目前最多只能同时订阅 50 个 Topic，每个 Topic 中最多只能订阅 64 个消息发布者。

如果用户网络连接出现问题，RTM 2 将自动尝试重新连接，但在断连期间的消息会丢失。

调用该方法会触发 [`onTopicSubscribed`](api-client-unity#ontopicsubscribed) 回调。


| 参数    | 描述                                                    |
| ------------| ------------------------------------------------------------- |
| `topic`      | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options`    | （选填）订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-unity#topicoptions)。如果不填写该字段，SDK 将随机订阅该 Topic 中 64 个消息发布者。                  |


#### 基本用法

##### 订阅指定用户
```csharp
// 创建一个 TopicOptions 实例。
TopicOptions topicOptions = new TopicOptions();
// 订阅消息发布者：Tony 和 Mary。
topicOptions.users = ["Tony","Mary"];
 // 消息发布者数量为 2。
topicOptions.userCount = 2;
// 订阅名为 gesture 的 Topic，并且订阅消息发布者 Tony 和 Mary。
loc_stChannel.SubscribeTopic( "gesture",topicOptions) ;
// 订阅消息发布者：Lily 和 Jason。
topicOptions.users = ["Lily","Jason"];
 // 消息发布者数量为 2。
topicOptions.userCount = 2;
// 订阅名为 gesture 的 Topic，并且订阅消息发布者 Lily 和 Jason。
// 现在你已订阅了 Tony，Mary，Lily 和 Jason。
loc_stChannel.SubscribeTopic( "gesture",topicOptions) ;
```

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。


### UnsubscribeTopic
#### 接口描述

```csharp
public abstract int UnsubscribeTopic(string topic, TopicOptions options);
```

取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。

调用该方法会触发 [`OnTopicUnsubscribed`](api-client-unity#ontopicunsubscribed) 回调。

| 参数    | 描述                                                    |
| ------------| -------------------------------------------------------------- |
| `topic`      | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `options`    | （选填）取消订阅 Topic 时的配置选项，详见 [`TopicOptions`](api-topic-unity#topicoptions)。你可以指定想要取消订阅的消息发布者。<ul><li>如果 <code>options</code> 中配置的用户列表不在已订阅的用户名单中，API 会返回正常调用结果，但订阅用户列表不会有任何变化。</li><li>如果该字段为空，将取消订阅该 Topic 及取消订阅该 Topic 中所有消息发布者。</li></ul>        |


#### 基本用法

##### 取消订阅指定用户

```csharp
// 创建一个 TopicOptions 实例。
TopicOptions topicOptions = new TopicOptions();
// 取消订阅消息发布者：Tony 和 Mary。
topicOptions.users = ["Tony","Mary"];
// 取消名为 gesture 的 Topic 中的消息发布者 Tony 和 Mary。
loc_stChannel.UnsubscribeTopic( "gesture",topicOptions) ;
```

##### 取消订阅 Topic 

```csharp
// 取消订阅名为 gesture 的 Topic 及取消订阅该 Topic 中所有消息发布者。
loc_stChannel.UnsubscribeTopic( "gesture") ;
```


#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

### GetSubscribedUserList
#### 接口描述

```csharp
public abstract int GetSubscribedUserList(string topic, ref UserList users);
```

查询指定 Topic 中已订阅的消息发布者列表。

> 注意：请在加入频道后调用该方法。

| 参数 | 描述                                                    |
| ---------| -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。~40875530-6fb8-11ed-8dae-bf25bf08a626~ |
| `users`   | `UserList` 对象的引用，用于返回已订阅的用户列表。详见  [`UserList`](#userlist)。                                                  |


#### 基本用法

```csharp
//创建一个 UserList 实例。
UserList userList = new UserList();
// 获取名为 gesture 的 Topic 中已订阅的消息发布者列表。
loc_stChannel.GetSubscribedUserList("gesture",userList);
// 打印已订阅消息发布者数量。
Debug.Log("You have Subscribed" + userList.userCount + "Publishers");
// 打印已订阅消息发布者信息列表。
Debug.Log("They are  " + userList.users);
```

#### 返回值
- `0`：调用成功。
- < `0`：调用失败。

## Class

### JoinChannelOptions

```csharp
public class JoinChannelOptions
{
    public JoinChannelOptions()
    {
        this.token = "";
    }
    public string token { set; get; }
};
```

加入频道时的配置选项。

| 参数   | 描述      | 
| ------------ |  --------- |
| `token`        | （选填） 用于鉴权的 RTM Token。                 |



### JoinTopicOptions

```csharp
public class JoinTopicOptions
{ 
    public RTM_MESSAGE_QOS qos { set; get; }

    public IntPtr meta { set; get; }

    public uint metaLength { set; get; }
};
```

加入 Topic 的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `qos` | 指定后续发送 Topic 消息时的 QoS 保障，详见 [`RTM_MESSAGE_QOS`](#rtm_message_qos) 。默认值为 `RTM_MESSAGE_QOS_ORDERED`：开启消息保序。  |
| `meta`   |  （选填）Topic 的元数据。  |
| `metaLength`   |  （选填）Topic 的元数据的长度。  |

### TopicOptions

```csharp
public class TopicOptions
{
    public string[] users { set; get; }

    public uint userCount { set; get; }
};
```

订阅或取消订阅 Topic 时的配置选项。

| 参数 | 描述                         |
| ---------------- | ---------------- |
| `users`    | （选填）该 Topic 中想要订阅的消息发布者列表，消息发布者数量不能超过 64 个。              |
| `userCount` | （选填）订阅的消息发布者数量。                                                  |


### TopicSubUsersUpdated

```csharp
public class TopicSubUsersUpdated
{
    public string topic { set; get; }

    public string[] succeedUsers { set; get; }

    public uint succeedUserCount { set; get; }

    public string[] failedUsers { set; get; }

    public uint failedUserCount { set; get; }
};
```

Topic 中订阅的用户更新。

| 参数 | 描述                                                    |
| --------- | -------------------------------------------------------------- |
| `topic`   | Topic 名称，同一个频道内相同的 Topic 名称属于同一个 Topic。 |
| `succeedUsers`   | 订阅或取消订阅成功的用户的 User ID 列表。 |
| `succeedUserCount`   | 订阅或取消订阅成功的用户的数量。 |
| `failedUsers`   | 订阅或取消订阅失败的用户的 User ID 列表。 |
| `failedUserCount`   | 订阅或取消订阅失败的用户的数量。 |


### UserList

```csharp
public class UserList
{
    public string[] users { set; get; }

    public uint userCount { set; get; }
};
```

用户列表。

| 参数  | 描述    |
| --------------------------------- |---------------------------------------------------- |
| `users` | 用户 ID 列表。                                                                      |
| `userCount`   | 用户数量。                                                                    |


## Enum
### RTM_MESSAGE_QOS

```csharp
public enum RTM_MESSAGE_QOS
{
    RTM_MESSAGE_QOS_UNORDERED = 0,

    RTM_MESSAGE_QOS_ORDERED = 1,
};
```

消息保序配置。

| 枚举值 |  描述                                                    |
| --------- | -------------------------------------------------------------- |
| `RTM_MESSAGE_QOS_UNORDERED`   | 0: 不开启消息保序。 |
| `RTM_MESSAGE_QOS_ORDERED`   | 1: 开启消息保序。 |


