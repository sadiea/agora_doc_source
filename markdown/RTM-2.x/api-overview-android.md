RTM 2 客户端 SDK 包含以下方法和回调接口：

### 初始配置

| 方法       | 描述                                       |
| ---------- | ------------------------------------------ |
| [create](api-client-android#create)| 创建 `RtmClient` 单实例。                   |
| [initialize](api-client-android#initialize)| 初始化 `RtmClient` 单实例。                   |
| [release](api-client-android#release)   | 销毁一个 `RtmClient` 单实例以释放资源。 |


### 频道

#### Stream Channel

| 方法                | 描述                                       |
| ------------------- | ------------------------------------------ |
| [createStreamChannel](api-client-android#createstreamchannel) | 创建一个 `StreamChannel` 类型实例。       |
| [release](api-channel-android#release)            | 销毁一个 `StreamChannel` 类型实例。       |
| [join](api-channel-android#join)               | 加入频道。                                 |
| [getChannelName](api-channel-android#getchannelname)      | 获取频道名称。                             |
| [leave](api-channel-android#leave)               | 离开频道。                                 |
| [joinTopic](api-channel-android#jointopic)                | 加入一个 Topic。                                             |
| [leaveTopic](api-channel-android#leavetopic)               | 离开一个 Topic。                                             |
| [publishTopicMessage](api-channel-android#publishtopicmessage)      | 在指定 Topic 中发送文本消息。                                |
| [subscribeTopic](api-channel-android#subscribetopic)           | 订阅 Topic 及 Topic 中的消息发送者。                         |
| [unsubscribeTopic](api-channel-android#unsubscribetopic)         | 取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。 |
| [getSubscribedUserList](api-channel-android#getsubscribeduserlist)    | 查询指定 Topic 中已订阅的消息发布者列表。                    |


| 回调       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onJoinResult](api-client-android#onjoinresult) | [join](api-channel-android#join) 调用的异步回调，在加入频道时触发。     |
| [onLeaveResult](api-client-android#onleaveresult)    | [leave](api-channel-android#leave) 调用的异步回调，在离开频道时触发。 |
| [onJoinTopicResult](api-client-android#onjointopicresult) | [joinTopic](api-channel-android#jointopic) 调用的异步回调，在加入 Topic 时触发。     |
| [onLeaveTopicResult](api-client-android#onleavetopicresult) | [leaveTopic](api-channel-android#leavetopic) 调用的异步回调，在离开 Topic 时触发。     |
| [onTopicSubscribed](api-client-android#ontopicsubscribed) | [subscribeTopic](api-channel-android#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。     |
| [onTopicUnsubscribed](api-client-android#ontopicunsubscribed) | [unsubscribeTopic](api-channel-android#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。     |

### 事件通知

#### 频道

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onConnectionStateChange](api-client-android#onconnectionstatechange) | SDK 连接状态发生改变时会发送该事件通知。     |
| [onPresenceEvent](api-client-android#onpresenceevent)  | 当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。 |

#### 消息

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [onMessageEvent](api-client-android#onmessageevent) | 当你在 Message Channel 的频道或 Stream Channel 的 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。     |