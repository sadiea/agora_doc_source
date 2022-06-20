本文提供小程序 CallKit SDK 的 API 参考。

## 初始化

### loginAndInitMqtt

用户登录并初始化 SDK 和 MQTT 的连接。

#### 方法原型

```javascript
CallKitSDK.loginAndInitMqtt()
```

#### 示例代码

```javascript
CallKitSDK.loginAndInitMqtt(username).then(
    () => {
        console.log("初始化成功")
    }).catch((err) => {
        console.log("初始化失败", err)
    })
```

#### 参数

该方法参数为 `username`，数据类型为 String。该参数至少需要 6 位字符，且至少包含一个数字或一个字母。支持的字符集包含：
- 10 个数字 0-9
- 26 个大写英文字母 A-Z
- 26 个小写英文字母 a-z

请确保同一频道内的 `username` 是唯一的。


#### 返回值

该方法返回值为 `Promise<void>`。

## 用户管理

### userGetData

获取缓存中当前登录的用户信息。

#### 方法原型

```javascript
CallkitSDK.getInstanceManager().userGetData()
```

#### 示例代码

```javascript
const userData = CallkitSDK.getInstanceManager().userGetData()
```

#### 返回值

该方法返回一个用户信息的 Object，其中包含如下字段：
- `username`: 当前登录的用户 ID。
- `clientId`: 当前登录的用户的 client ID。

### userLogOut

用户登出。同时终止 SDK 与 RTC 服务和 MQTT 的连接。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().userLogOut()
```

#### 示例代码

```javascript
CallKitSDK.getInstanceManager().userLogOut()
```

## 呼叫管理

### userPublishStream

发布音视频流。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().userPublishStream()
```

#### 示例代码

```javascript
CallKitSDK.getInstanceManager().userPublishStream().then(
    (pushUrl) => {
        console.log("发布音视频流成功", pushUrl)
    }).catch((err) => {
        console.log("发布音视频流失败", err)
    })
```

#### 返回值

该方法返回一个 `Promise` 对象，包含如下参数：
- `pushUrl`: 发布的音视频流的 URL 地址，数据类型为 String。

### callDevice

发出呼叫邀请。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().callDevice()
```

#### 示例代码

```javascript
CallKitSDK.getInstanceManager().callDevice({
    deviceId: "mydoorbell",
    attachMsg: "Hello World",
}).then(() => {
    console.log("呼叫设备成功")
}).catch((err) => {
    console.log("呼叫设备失败")
})
```

#### 参数

该方法参数为包含了被叫端设备信息的 Object 对象，包含如下字段：
- `deviceId`: 被叫设备端的 ID，数据类型为 String。
- `attachMsg`: 发送给被叫设备端的呼叫邀请消息，数据类型为 String。

#### 返回值

该方法返回值为 `Promise<void>`。

### answerDevice

接听来电。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().answerDevice()
```

#### 示例代码

```javascript
CallKitSDK.getInstanceManager().answerDevice().then((res) => {
    log.e("接听成功")
}).catch((err) => {
    log.e("接听失败")
})
```

#### 返回值

该方法返回值为 `Promise<void>`。

### hangupDevice

挂断来电。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().hangupDevice()
```

#### 示例代码

```javascript
CallKitSDK.getInstanceManager().hangupDevice().then((res) => {
    log.e("挂断成功")
}).catch((err) => {
    log.e("挂断失败")
})
```

#### 返回值

该方法返回值为 `Promise<void>`。

## 事件管理

### peerRequestEventCallback

接收到对端用户发起的呼叫邀请时，SDK 会触发该回调。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().peerRequestEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.peerRequestEventCallbackUnsubscribe = CallKitSDK.getInstanceManager().peerRequestEventCallback((event) => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.peerRequestEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<event>`，其中 `event` 对象为呼叫时必需的参数，包含：
-  `attachMsg`，即对端发起呼叫邀请时发出的消息。 

#### 返回值

该方法会返回一个同步的 `peerRequestEventCallbackUnsubscribe` 函数，用于取消监听 `peerRequestEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。


### peerStreamAddedEventCallback

接收到对端用户发布的音视频流时，SDK 会触发该回调。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().peerStreamAddedEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.peerStreamAddedEventCallbackUnsubscribe = CallKitSDK.getInstanceManager().peerStreamAddedEventCallback((playUrl) => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.peerStreamAddedEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<playUrl>`，其中 `playUrl` 为用于音视频流下行播放的 URL 地址。

#### 返回值

该方法会返回一个同步的 `peerStreamAddedEventCallbackUnsubscribe` 函数，用于取消监听 `peerStreamAddedEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。


### peerAnswerEventCallback

对端用户接听发出的呼叫邀请时，SDK 会触发该回调。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().peerAnswerEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.peerAnswerEventCallbackUnsubscribe = CallKitSDK.getInstance().peerAnswerEventCallback(() => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.peerAnswerEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<void>`。

#### 返回值

该方法会返回一个同步的 `peerAnswerEventCallbackUnsubscribe` 函数，用于取消监听 `peerAnswerEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。

### peerHangupEventCallback

对端用户挂断电话时，SDK 会触发该回调。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().peerHangupEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.peerHangupEventCallbackUnsubscribe = CallKitSDK.getInstanceManager().peerHangupEventCallback(() => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.peerHangupEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<void>`。

#### 返回值

该方法会返回一个同步的 `peerHangupEventCallbackUnsubscribe` 函数，用于取消监听 `peerHangupEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。

### peerBusyEventCallback

对端用户占线时，SDK 会触发该回调。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().peerBusyEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.peerBusyEventCallbackUnsubscribe = CallKit.getInstanceManager().peerBusyEventCallback(() => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.peerBusyEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<void>`。

#### 返回值

该方法会返回一个同步的 `peerBusyEventCallbackUnsubscribe` 函数，用于取消监听 `peerBusyEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。

### userSessionEndEventCallback

MQTT 连接状态或用户登录状态发生改变时，SDK 会触发该回调。你需要提醒用户退出登录。

#### 方法原型

```javascript
CallKitSDK.getInstanceManager().userSessionEndEventCallback()
```

#### 示例代码

```javascript
onLoad: function() {
    this.userSessionEndEventCallbackUnsubscribe = CallKitSDK.getInstanceManager().userSessionEndEventCallback(() => {
        // 代码
    })
},
onUnload: function() {
    // 取消订阅事件
    this.userSessionEndEventCallbackUnsubscribe()
}
```

#### 参数

该方法参数为一个 `callback` 对象 `function<void>`。

#### 返回值

该方法会返回一个同步的 `userSessionEndEventCallbackUnsubscribe` 函数，用于取消监听 `userSessionEndEventCallback` 事件。当小程序退出、页面被卸载，或者不想再监听该事件时，可以调用该方法。
