RTM 2 客户端 SDK 包含以下方法和回调接口：

### 初始配置

| 方法       | 描述                                       |
| ---------- | ------------------------------------------ |
| [createAgoraRtmClient](api-client-flutter#createagorartmclient)| 创建 `RtmClient` 单实例。                   |
| [initialize](api-client-flutter#initialize)| 初始化 `RtmClient` 单实例。                   |
| [release](api-client-flutter#release)   | 销毁 `RtmClient` 单实例以释放资源。 |

### 频道

#### Stream Channel

| 方法                | 描述                                       |
| ------------------- | ------------------------------------------ |
| [createStreamChannel](api-client-flutter#createstreamchannel) | 创建一个 `StreamChannel` 类型实例。       |
| [release](api-channel-flutter#release)            | 销毁一个 `StreamChannel` 类型实例。       |
| [join](api-channel-flutter#join)               | 加入频道。                                 |
| [getChannelName](api-channel-flutter#getchannelname)      | 获取频道名称。                             |
| [leave](api-channel-flutter#leave)               | 离开频道。                                 |
| [joinTopic](api-channel-flutter#jointopic)                | 加入一个 Topic。                                             |
| [leaveTopic](api-channel-flutter#leavetopic)               | 离开一个 Topic。                                             |
| [publishTopicMessage](api-channel-flutter#publishtopicmessage12)      | 在指定 Topic 中发送文本消息。                                |
| [subscribeTopic](api-channel-flutter#subscribetopic)           | 订阅 Topic 及 Topic 中的消息发送者。                         |
| [unsubscribeTopic](api-channel-flutter#unsubscribetopic)         | 取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。 |
| [getSubscribedUserList](api-channel-flutter#getsubscribeduserlist)    | 查询指定 Topic 中已订阅的消息发布者列表。                    |

| 回调       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onJoinResult](api-client-flutter#onjoinresult) | [join](api-channel-flutter#join) 调用的异步回调，在加入频道时触发。     |
| [onLeaveResult](api-client-flutter#onleaveresult)    | [leave](api-channel-flutter#leave) 调用的异步回调，在离开频道时触发。 |
| [onJoinTopicResult](api-client-flutter#onjointopicresult) | [joinTopic](api-channel-flutter#jointopic) 调用的异步回调，在加入 Topic 时触发。     |
| [onLeaveTopicResult](api-client-flutter#onleavetopicresult) | [leaveTopic](api-channel-flutter#leavetopic) 调用的异步回调，在离开 Topic 时触发。     |
| [onTopicSubscribed](api-client-flutter#ontopicsubscribed) | [subscribeTopic](api-channel-flutter#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。     |
| [onTopicUnsubscribed](api-client-flutter#ontopicunsubscribed) | [unsubscribeTopic](api-channel-flutter#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。     |

### 事件通知

#### 频道

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onConnectionStateChange](api-client-flutter#onconnectionstatechange) | SDK 连接状态发生改变时会发送该事件通知。     |
| [onPresenceEvent](api-client-flutter#onpresenceevent)  | 当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。 |

#### 消息

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onMessageEvent](api-client-flutter#onmessageevent) | 当你在 Message Channel 的频道或 Stream Channel 的 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。     |