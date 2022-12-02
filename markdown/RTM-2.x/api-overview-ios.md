RTM 2 客户端 SDK 包含以下方法和回调接口：

### 初始配置

| 方法       | 描述                                       |
| ---------- | ------------------------------------------ |
| [initWithConfig](api-client-ios#initwithconfig)| 初始化 `AgoraRtmClientKit` 单实例。                   |
| [destroy](api-client-ios#destroy)   | 销毁一个 `AgoraRtmClientKit` 单实例以释放资源。 |

### 频道

#### Stream Channel

| 方法                | 描述                                       |
| ------------------- | ------------------------------------------ |
| [createStreamChannel](api-client-ios#createstreamchannel) | 创建一个 `AgoraRtmStreamChannel` 类型实例。       |
| [destroy](api-channel-ios#destroy)            | 销毁一个 `AgoraRtmStreamChannel` 类型实例。       |
| [joinWithOption](api-channel-ios#joinwithoption)               | 加入频道。                                 |
| [getChannelName](api-channel-ios#getchannelname)      | 获取频道名称。                             |
| [leave](api-channel-ios#leave)               | 离开频道。                                 |
| [joinTopic](api-channel-ios#jointopic)                | 加入一个 Topic。                                             |
| [leaveTopic](api-channel-ios#leavetopic)               | 离开一个 Topic。                                             |
| [publishMessage](api-channel-ios#publishmessage)      | 在指定 Topic 中发送文本消息。                                |
| [subscribeTopic](api-channel-ios#subscribetopic)           | 订阅 Topic 及 Topic 中的消息发送者。                         |
| [unsubscribeTopic](api-channel-ios#unsubscribetopic)         | 取消订阅某 Topic 或取消对该 Topic 中指定的消息发布者的订阅。 |
| [getSubscribedUserList](api-channel-ios#getsubscribeduserlist)    | 查询指定 Topic 中已订阅的消息发布者列表。                    |

| 回调       | 描述                                       |
| ---------- | ------------------------------------------ |
| [joinResult](api-client-ios#onjoinresult) | [joinWithOption](api-channel-ios#joinwithoption) 调用的异步回调，在加入频道时触发。     |
| [leaveResult](api-client-ios#onleaveresult)    | [leave](api-channel-ios#leave) 调用的异步回调，在离开频道时触发。 |
| [joinTopic](api-client-ios#jointopic) | [joinTopic](api-channel-ios#jointopic) 调用的异步回调，在加入 Topic 时触发。     |
| [leaveTopic](api-client-ios#leavetopic) | [leaveTopic](api-channel-ios#leavetopic) 调用的异步回调，在离开 Topic 时触发。     |
| [subscribe](api-client-ios#subscribe) | [subscribeTopic](api-channel-ios#subscribetopic) 调用的异步回调，订阅 Topic 或订阅 Topic 中消息发布者时会触发该回调。     |
| [unsubscribe](api-client-ios#unsubscribe) | [unsubscribeTopic](api-channel-ios#unsubscribetopic) 调用的异步回调，取消订阅 Topic 或取消订阅 Topic 中消息发布者时会触发该回调。     |

### 事件通知

#### 频道

| 事件                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| [connectionStateChange](api-client-ios#connectionstatechange) | SDK 连接状态发生改变时会发送该事件通知。     |
| [OnPresenceEvent](api-client-ios#onpresenceevent)  | 当频道中有用户的 Presence 状态发生变更时，SDK 会发送该事件通知。 |

#### 消息

| 事件       | 描述                                       |
| ---------- | ------------------------------------------ |
| [OnMessageEvent](api-client-ios#onmessageevent) | 当你在 Message Channel 的频道或 Stream Channel 的 Topic 中订阅的用户发布消息时，SDK 会发送该事件通知。     |

