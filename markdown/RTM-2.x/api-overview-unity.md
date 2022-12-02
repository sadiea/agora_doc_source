RTM 2 客户端 SDK 包含以下方法和回调接口：

### 初始配置

| 方法       | 描述                                       |
| ---------- | ------------------------------------------ |
| [CreateAgoraRtmClient](api-client-unity#createagorartmclient)| 创建 `IRtmClient` 单实例。                   |
| [Initialize](api-client-unity#initialize)| 初始化 `IRtmClient` 单实例。                   |
| [Release](api-client-unity#release)   | 销毁 `IRtmClient` 单实例以释放资源。 |

### 频道

#### Stream Channel

| 方法                | 描述                                       |
| ------------------- | ------------------------------------------ |
| [CreateStreamChannel](api-client-unity#createstreamchannel) | 创建一个 `IStreamChannel` 类型实例。       |
| [Release](api-channel-unity#release)            | 销毁一个 `IStreamChannel` 类型实例。       |
| [Join](api-channel-unity#join)               | 加入频道。                                 |
| [GetChannelName](api-channel-unity#getchannelname)      | 获取频道名称。                             |
| [Leave](api-channel-unity#leave)               | 离开频道。                                 |
| [JoinTopic](api-channel-unity#jointopic)                | 加入一个 Topic。                                             |
| [LeaveTopic](api-channel-unity#leavetopic)               | 离开一个 Topic。                                             |
| [PublishTopicMessage[1/2]](api-channel-unity#publishtopicmessage12)      | 在指定 Topic 中发送文本消息。                                |
| [PublishTopicMessage[2/2]](api-channel-unity#publishtopicmessage22)      | 在指定 Topic 中发送文本消息。                                |
| [SubscribeTopic](api-channel-unity#subscribetopic)           | 订阅 Topic 及 Topic 中的消息发送者。                         |
| [UnsubscribeTopic](api-channel-unity#unsubscribetopic)         | 取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。 |
| [GetSubscribedUserList](api-channel-unity#getsubscribeduserlist)    | 查询指定 Topic 中已订阅的消息发布者列表。                    |

| 回调       | 描述                                       |
| ---------- | ------------------------------------------ |
| [OnJoinResult](api-client-unity#onjoinresult) | [Join](api-channel-unity#join) 调用的异步回调，在加入频道时触发。     |
| [OnLeaveResult](api-client-unity#onleaveresult)    | [Leave](api-channel-unity#leave) 调用的异步回调，在离开频道时触发。 |
| [OnJoinTopicResult](api-client-unity#onjointopicresult) | [JoinTopic](api-channel-unity#jointopic) 调用的异步回调，在加入 Topic 时触发。     |
| [OnLeaveTopicResult](api-client-unity#onleavetopicresult) | [LeaveTopic](api-channel-unity#leavetopic) 调用的异步回调，在离开 Topic 时触发。     |
| [OnTopicSubscribed](api-client-unity#ontopicsubscribed) | [SubscribeTopic](api-channel-unity#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。     |
| [OnTopicUnsubscribed](api-client-unity#ontopicunsubscribed) | [UnsubscribeTopic](api-channel-unity#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。     |

### 事件通知

#### 频道

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [OnConnectionStateChange](api-client-unity#onconnectionstatechange) | SDK 连接状态发生改变时会发送该事件通知。     |
| [OnPresenceEvent](api-client-unity#onpresenceevent)  | 当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。 |

#### 消息

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [OnMessageEvent](api-client-unity#onmessageevent) | 当你在 Message Channel 的频道或 Stream Channel 的 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。     |