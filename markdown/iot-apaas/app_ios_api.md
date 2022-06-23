本页提供 IoT Android SDK  的 API 参考。

## 概述

| 类 | 描述 |
| --- | --- |
| [IAgoraIotAppSdk](#sdk) | 提供总的服务接口，各子功能通过 `getInstance` 方法获取 | 
| [IAccountMgr](#account)  | 账号注册、登录与登出 |
| [IAccountMgrCallback](@accountcallback) | 账号注册、登录与登出事件 |
| [ICallkitMgr](#callkit) | 来电呼叫管理，包括拨号、接听、挂断等 |
| [ICallkitMgrCallback](#callkitcallback) | 来电呼叫管理事件 |
| [IDeviceMgr](#device)   | 设备管理接口，包括设备绑定、设备解绑、设备列表查询、设备分享管理等 |
| [IDeviceMgrCallback](#devicecallback) | 设备管理事件 |
| [IAlarmMgr](#alarm)     | 设备告警信息管理，包括移动侦察、有人经过等 |
| [IAlarmMgrCallback](#alarmcallback) | 设备告警信息管理事件 |
| [INotificationMgr](#notification) | 设备系统信息管理，包括设备上线、下线、绑定、解绑等事件管理 |
| [INotificationMgrCallback](#notificationcallback) | 设备系统信息管理事件 |
| [ErrCode](#err) | Iot SDK 在使用过程中的报错信息 |


<a name="sdk"></a>

## IAgoraIotAppSdk

#### initialize

```swift
func initialize(initParam: InitParam,netStatus:@escaping(NetStatus,String)->Void) -> Int
```

#### release

#### accounrMgr

#### callkitMgr

#### alarmMgr

#### notificationMgr

<a name="account"></a>

## IAccountMgr

#### ICallback

#### AccountInfo

#### updateToken

#### register

#### unregister

#### getCode

#### login

#### logout

#### changePassword

#### getUserId

#### getAccountInfo

<a name="accountcallback"></a>

## IAccountMgrCallback

#### onRegisterDone

#### onUnregisterDone

#### onLoginDone

#### onLogoutDone

#### onLoginOtherDevvice

#### onChangePasswordDone


<a name="callkit"></a>

## ICallkitMgr


<a name="callkitcallback"></a>

## ICallkitMgrCallback


<a name="device"></a>

## IDeviceMgr

<a name="devicecallback"></a>

## IDeviceMgrCallback


<a name="alarm"></a>

## IAlarmMgr

<a name="alarmcallback"></a>

## IAlarmMgrCallback

<a name="notification"></a>

## INotificationMgr


<a name="notificationcallback"></a>

## INotificationMgrCallback

## ErrCode

### 全局错误

| 错误码 | 错误代码 | 描述 |
| --- | --- | --- |
| `0` | `XERR_NONG` | 无错误。|

### 通用错误

### 文件操作错误

### 账号相关错误

### 呼叫系统相关错误

### 设备管理相关错误

### 告警管理相关错误

### 通知管理相关错误

## 数据类型

### InitParam 类

Iot SDK 初始化参数。

```swift
public struct InitParam {
    public var rtcAppId: String? = nil
    public var logFilePath : String? = nil
    public var publishAudio = true
    public var publishVideo = true
    public var subscribeAudio = true
    public var subscribeVideo = true
    public init(){}
}
```

| 参数  | 描述 |
| --- | --- |
| `rtcAppId` |    |
| `logFilePath` |    |


### IAlarmMgeQuertParam 类

### IotAlarm 类

### IotDevice 类

### IotNotification 类

### NetStatus 枚举

当前网络连接状态。

```swift
public enum NetStatus{
    case Reconnected
    case Disconnected
}
```

### PeerAction 枚举

### CallState 枚举

### AudioEffectId 枚举