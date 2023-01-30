Channels（频道） 是 RTM 2 实时网络中一种数据传输的管理机制，任何订阅或加入频道的用户都可以在 100ms 内接收到频道中传输的消息和事件，RTM 2 允许客户端订阅数百甚至数千个频道。大多数 RTM 2 API 都将以频道为基础进行发送、接收、加密等行为。

## 频道类型

基于声网的能力，RTM 2 的频道分成两种类型以匹配不同的应用场景：Stream Channel 和 Message Channel。两种频道类型的主要区别如下：

| 频道类型| 主要特征| 适用场景|
| --------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Message Channel | 遵循行业最标准的 Pub/Sub （发布/订阅）模式，频道无需提前创建，随用随取，频道内的生产者和消费者数量没有上限，但对于单个频道的上行 QPS（Queries per second，每秒查询数) 有限制。| 常见的基于 Pub/Sub 场景的应用及多设备接入的应用，例如：IoT 行业的多设备管理和指令收发，智能设备中的多终端管理和指令收发位置跟踪，大型社区等。 |
| Stream Channel  | 遵循行业类似观察者模式的 Room （房间）概念，用户需要进入频道（Join Channel）才能进行下一步操作。频道中允许创建不同的 Topic，频道内最多容纳 1000 个用户，单个 Topic 的上行 QPS 可以提升至较高值。 | 元宇宙中高并发上行数据的传输应用场景，以及与 RTC 合并使用的场景，例如：元宇宙，平行驾驶，云游戏等。|

RTM 2 网络中两种频道的特性对比如下：

| 特性| Message Channel| Stream Channel|
| -------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| 频道创建   | 用户不必提前定义或创建频道，<p>在频道上发布消息时会自动创建频道。</p>| 频道需要提前创建，<p>加入频道后才能收发消息或事件通知。</p>|
| 使用限制   | 用户发送消息（Publish）不限制频道数量。<p>单个频道不限制订阅者数量。</p><p>单个用户可以同时订阅最多 100 个的频道，</p>| 单个频道可容纳最多 1000 个用户。<p>只有在房间中的用户才可以发布/订阅频道中的 Topics。</p>|
| 频道类型     | 频道按照使用场景可以分为：<li>1on1 Channel（一对一频道）</li><li>Group Channel（多人频道）</li><li>Broadcast Channel（直播频道）</li><li>Unicast Channel（单播频道）</li> | 类似于 Room 的概念，属于标准的 Group Channel。|
| 频道名称     | 单个 VID 下频道名唯一。频道名称不能为空，可为长度在 64 字节以内的字符串。<p>以下为支持的字符集范围:</p><li>26 个小写英文字母 a - z</li><li>26 个大写英文字母 A - Z</li><li>10 个数字 0 - 9</li><li>空格</li><li>"!"、"#"、"$"、"%"、"&"、"("、")"、"+"、"-"、":"、";"、"&lt;"、"="、"&gt;"、"?"、"@"、"["、"]"、"^"、"_"、"{"、"}"、"&vert;"、"~"、","</li><p>声网建议你使用频道用途或者从这个频道传输的消息的类型来定义频道名称。</p>| 同 Message Channel。|
| 频道安全     | 可采用 Token 来控制客户端对频道的访问。<p>频道传输默认开启 TSL 传输层加密。</p><p>用户可以在初始化的时候配置 cipherKey 对消息负载再次加密。</p>| 同 Message Channel。 |
| Topic   | 无 Topic 概念。| 频道中可以创建多个 Topic，<p>用户消息通过频道中的 Topics 进行投递。</p>|
| Presence | 四个 UID 级别的事件：<li>Join（加入频道）</li><li>Leave（离开频道）</li><li>Timeout（超时）</li><li>State change（状态变化）</li><p>两个 Channel 级别的事件：</p><li>Active（活跃）</li><li>Inactive（非活跃）</li>| 同 Message Channel。 |
| Storage  | 具备频道属性。| 具备频道属性。|

两种频道的区别可见下图：
![](https://web-cdn.agora.io/docs-files/1667463090215)

## Message Channel

### 订阅 Message Channel

#### API 描述

```javascript
subscribe({string msChannel, string msChannelGroup, Boolean withPresence, Boolean beQuite, Boolean withMetaData })
```

客户端一旦设置了事件监听处理程序，就可以发出订阅频道的请求。请求订阅频道的同时也会请求连接 RTM 服务器，连接成功后会触发一个连接状态事件。

客户端只要成功订阅了一个频道，就会为该频道建立起 TCP 长连接。该 TCP 连接会持续打开，只要有用户在该频道发送消息，任何该频道的订阅者都会在 100 毫秒内收到消息。

调用 `subscribe` 方法可以订阅频道并在客户端和 RTM 服务器之间建立起 TCP 长连接，客户端可以从监听的回调和事件中获取已订阅频道的消息和状态。

#### 参数介绍

|      参数       |  类型   |                   是否必填                   | 默认值 |             描述              |
| :------------------: | :-----: | :------------------------------------------: | :------: | :----------------------------------: |
|   `msChannel`    | String  |                     必填                      |     无     |           指定订阅的频道名。如需订阅多个频道，将频道名以字符串数组的方式填入该参数。<p>**Note**: 单个客户端可以同时订阅最多 100 个频道。</p>           |
|   `withMessage`    | Boolean |                   选填                   |   true   |   是否订阅 `msChannel` 中的消息：<li>true：订阅</li><li>false：不订阅</li>  |
|  `withPresence`  | Boolean |                   选填                   |  true  |        是否订阅远端用户的 Presence 事件：<li>true：订阅</li><li>false：不订阅</li>        |
|    `beQuite`     | Boolean |                   选填                   |  false   |       是否禁止本地用户发送 Presence 事件：<li>true：禁止</li><li>false：不禁止</li>       |
|   `withMetaData`   | Boolean |                   选填                   |   true   | 是否订阅频道属性：<li>true：订阅</li><li>false：不订阅</li>  |

#### 基本用法

示例 1：仅订阅频道

```javascript
rtm.subscribe({
    msChannel: 'my_channel_1'
});
```

示例 2：订阅频道和频道内远端用户的 Presence 事件

```javascript
rtm.subscribe({
    msChannel: 'my_channel',
    withPresence: true,
    beQuite:false
});
```

示例 3：订阅频道和频道内远端用户的 Presence 事件，但禁止本地用户发送 Presence 事件，即隐身登陆

```javascript
rtm.subscribe({
    msChannel: 'my_channel',
    withPresence: true,
    beQuite:true
});
```

如果远端用户订阅 `my_channel` 频道，你会收到 `join` 事件：

```javascript
{
    "channelType":"msChannel",
    "channelName": "my_channel",
    "channel": "my_channel",
    "subscription": null,
    "actualChannel": null,
    "subscribedChannel": "my_channel-rtmres",
    "action": "join",
    "timetoken": "15119466699655811",
    "occupancy": 2,
    "uid": "User1",
    "timestamp": 1511946669
}
```

如果远端用户取消订阅 `my_channel` 频道，你会收到 `leave` 事件：

```javascript
{
    "channelType":"msChannel",
    "channelName": "my_channel",
    "channel": "my_channel",
    "subscription": null,
    "actualChannel": null,
    "subscribedChannel": "my_channel-rtmpres",
    "action": "leave",
    "timetoken": "15119446002445794",
    "occupancy": 1,
    "uid": "User1",
    "timestamp": 1511944600
}
```

如果远端用户和 `my_channel` 频道的连接超时，你会收到 `timeout` 事件：

```javascript
{
   "channelType":"msChannel",
   "channelName": "my_channel",
   "channel": "my_channel",
   "subscription": null,
   "actualChannel": null,
   "subscribedChannel": "my_channel-rtmpres",
   "action": "timeout",
   "timetoken": "15119519897494311",
   "occupancy": 3,
   "uid": "User2",
   "timestamp": 1511951989
}
```

如果远端用户在 `my_channel` 频道中的自定义状态发生改变，你会收到 `state-change` 事件：

```javascript
{
    "channelType":"msChannel",
    "channelName": "my_channel",
    "channel": "my_channel",
    "subscription": null,
    "actualChannel": null,
    "subscribedChannel": "my_channel-rtmpres",
    "action": "state-change",
    "state": {
        "isTyping": true
    },
    "timetoken": "15119477895378127",
    "occupancy": 5,
    "uid": "User4",
    "timestamp": 1511947789
}
```

如果你设置了定时通知模式，当远端用户在设置的时间段内发生状态变化时，你会收到 `interval` 事件：

```javascript
{
    "channelType":"msChannel",
    "channelName": "my_channel",
    "action":"interval",
    "timestamp":1548340678,
    "occupancy":2,
    "join":["Tony","Zhao"]},
    "timedout":["Robbin"],
    "b":"my-channel-rtmpres"
}
```

示例 4：带用户自定义状态的订阅：

```javascript
var rtm = new RTM({
    /* initiation arguments */
})
rtm.onEvent({
    status: function(statusEvent) {
        if (statusEvent.category === "RTMConnectedCategory") {
            var newState = {
                new: 'state'
            };
            rtm.setState(
                {
                    state: newState
                },
                function (status) {
                    // handle state setting response
                }
            );
        }
    },
    message: function(message) {
        // handle message
    },
    presence: function(presenceEvent) {
        // handle presence
    }
})

rtm.subscribe({
    msChannel: 'ch1', 'ch2', 'ch3'
});
```

### 取消订阅 Message Channel

#### API 描述

```javascript
unsubscribe({string msChannel}): Promise<UnsubscribeResponse>;
```

如果你无需关注某个频道，调用 `unsubscribe` 方法取消订阅该频道。每次调用该方法只能取消订阅一个频道，如需取消订阅多个频道，你需要多次调用该方法。

成功取消订阅该频道后，其他订阅该频道的远端用户会收到 `leave` 事件。

#### 参数介绍

|      参数       |  类型   |                   是否必填                   | 默认值 |             描述              |
| :------------------: | :-----: | :------------------------------------------: | :------: | :----------------------------------: |
|   `msChannel`    | String  |                     必填                      |    无      |           指定订阅的频道名称。如需订阅多个频道，将频道名称以字符串数组的方式填入该参数。<p>**Note**: 单个客户端最多可以同时订阅 100 个频道。</p>           |

#### 基本用法

```javascript
rtm.unsubscribe({msChannel: "chats.room1"}): Promise<UnsubscribeResponse>;
```

## Stream Channel

### 创建 Stream Channel

#### API 描述

```javascript
createStreamChannel({String stChannel}):Promise<CreateStreamChannelResponse>;
```

在使用 Stream Channel 之前，你需要先调用 `createStreamChannel` 方法创建 `StreamChannel` 实例。成功创建 `StreamChannel` 实例后，你可以调用 Stream Channel 相关方法进行加入频道、离开频道、销毁频道、发送消息、订阅消息等操作。

注意：
每次调用该方法只能创建一个 `StreamChannel` 实例，如需创建多个 `StreamChannel` 实例，你需要多次调用该方法。

#### 参数介绍

|      参数       |  类型   |                   是否必填                   | 默认值 |             描述              |
| :------------------: | :-----: | :------------------------------------------: | :------: | :----------------------------------: |
|   `stChannel`    | String  |                    必填                      |     无     |           频道名称。           |

#### 基本用法

```javascript
try{
    const Loc_stChannel = await rtm.createStreamChannel({
        stChannel:"Location"
        });
    console.log("Create Stream Channel w/ server response: ", response);
} catch (status){
    console.log("Create Stream Channel failed w/ status: ", status);
}
```

### 加入 Stream Channel

#### API 描述

```javascript
rtm.streamChannel.join({Boolean withPresence, Boolean beQuite, Boolean withMetaData })
```

成功创建 Stream Channel 后，你可以调用 `join` 方法加入 Stream Channel。

注意：
- 每次调用该方法只能加入一个频道，如需加入多个频道，你需要多次调用该方法。单个客户端可以同时加入最多 100 个频道。
- 用户需要收到成功加入频道的 `onJoinResult` 回调才能继续进行频道相关的操作。

#### 参数介绍

|     参数      |  类型   | 是否必填 | 默认值 |                         描述                          |
| :----------------: | :-----: | :------: | :------: | :----------------------------------------------------------: |
|  `withPresence`  | Boolean |                   选填                   |  true  |        是否订阅远端用户的 Presence 事件：<li>true：订阅</li><li>false：不订阅</li>        |
|    `beQuite`     | Boolean |                   选填                   |  false   |       是否禁止本地用户发送 Presence 事件：<li>true：禁止</li><li>false：不禁止</li>       |
|   `withMetaData`   | Boolean |                   选填                   |   true   | 是否订阅频道属性：<li>true：订阅</li><li>false：不订阅</li> |

#### 基本用法

示例 1：仅加入频道

```javascript
Loc_stChannel.join();
```

示例 2：加入频道并订阅频道内远端用户的 Presence 事件

```javascript
Loc_stChannel.join({
    withPresence: true,
    beQuite:false
});
```

示例 3：加入频道并订阅频道内远端用户的 Presence 事件，但禁止本地用户发送 Presence 事件，即隐身登陆

```javascript
Loc_stChannel.join({
    withPresence: true,
    beQuite:true
});
```

如果远端用户订阅 `my_channel` 频道，你会收到 `join` 事件：

```javascript
{
    "channelType":"stChannel",
    "channelName": "my_channel",
    "channelGroup": null,
    "channelTopic": null,
    "action": "join",
    "timetoken": "15119466699655811",
    "occupancy": 2,
    "uid": "User1",
    "timestamp": 1511946669
}
```

如果远端用户取消订阅 `my_channel` 频道，你会收到 `leave` 事件：

```javascript
{
    "channelType":"stChannel",
    "channelName": "my_channel",
    "channelGroup": null,
    "channelTopic": null,
    "action": "leave",
    "timetoken": "15119466699655811",
    "occupancy": 2,
    "uid": "User1",
    "timestamp": 1511946669
}
```

如果远端用户和 `my_channel` 频道的连接超时，你会收到 `timeout` 事件：

```javascript
{
    "channelType":"stChannel",
    "channelName": "my_channel",
    "channelGroup": null,
    "channelTopic": null,
    "action": "leave",
    "timetoken": "15119466699655811",
    "occupancy": 2,
    "uid": "User1",
    "timestamp": 1511946669
}
```

如果远端用户在 `my_channel` 频道中的自定义状态发生改变，你会收到 `state-change` 事件：

```javascript
{
    "channelType":"stChannel",
    "channelName": "my_channel",
    "channelGroup": null,
    "channelTopic": null,
    "action": "state-change",
    "state": {
        "isTyping": true
    },
    "timetoken": "15119477895378127",
    "occupancy": 5,
    "uid": "User4",
    "timestamp": 1511947789
}
```

如果你设置了定时通知模式，当远端用户在设置的时间段内发生状态变化时，你会收到 `interval` 事件：

```javascript
{
    "channelType":"stChannel",
    "channelName": "my_channel",
    "channelGroup": null,
    "channelTopic": null,
    "action":"interval",
    "timestamp":1548340678,
    "occupancy":2,
    "join":["Tony","Zhao"],
    "timedout":["Robbin"],
}
```

示例 4：带用户自定义状态的加入

```javascript
var rtm = new RTM({
    /* initiation arguments */
})
rtm.addListener({
    status: function(statusEvent) {
        if (statusEvent.category === "RTMConnectedCategory") {
            var newState = {
                new: 'state'
            };
            rtm.setState(
                {
                    state: newState
                },
                function (status) {
                    // handle state setting response
                }
            );
        }
    },
    message: function(message) {
        // handle message
    },
    presence: function(presenceEvent) {
        // handle presence
    }
})

Loc_stChannel.join();
```

### 离开 Stream Channel

#### API 描述

```javascript
rtm.streamChannel.leave():Promise<LeaveStreamChannelResponse>;
```

如果你不再需要待在某个频道中，你可以调用 `leave` 离开该频道。SDK 会触发如下事件：

- 调用该方法后，SDK 会触发 `onLeaveResult` 回调。
- 成功离开频道后，频道中的远端用户会收到 `leave` 事件。

#### 基本用法

```javascript
try{
    const result = await rtm.leave();
    console.log("leave Stream Channel w/ server response: ", response);


} catch (status){
    console.log("leave Stream Channel failed w/ status: ", status);
}
```

### 销毁 Stream Channel

#### API 描述

```javascript
rtm.streamChannel.release():Promise<ReleaseStreamChannelResponse>;
```

如果你不再需要某个频道，你可以调用 `release` 方法销毁对应的 `StreamChannel` 实例以释放资源。调用该方法销毁 `StreamChannel` 实例不会销毁该频道，后续可通过再次调用 `createStreamChannel` 和 `join` 重新加入该频道。

注意：
如果不先调用 `leave` 离开频道而直接调用 `release` 销毁频道实例，SDK 会自动调用 `leave` 并触发对应的事件。

#### 基本用法

```javascript
try{
    const result = await Loc_stChannel.release();
    console.log("release Stream Channel w/ server response: ", response);


} catch(status) {
    console.log("release Stream Channel failed w/ status: ", status);
}
```
