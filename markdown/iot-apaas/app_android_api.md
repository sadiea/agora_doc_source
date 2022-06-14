# AIotAppSdkFactory 类

```java
public class AIotAppSdkFactory 
```

SDK 实例工厂类。

## public 方法

### getInstance 方法

```java
public static IAgoraIotAppSdk getInstance()
```

构造函数。

#### 返回值

一个 `IAgoraIotAppSdk` 对象。

<a id="IAgoraIotAppSdk"></a>

# IAgoraIotAppSdk 接口

```java
public interface IAgoraIotAppSdk
```

SDK 入口。

## 常量

```java
public static final int SDK_STATE_INVALID = 0x0000;
```

SDK 未初始化。

```java
public static final int SDK_STATE_READY = 0x0001;
```

SDK 初始化完成，用户还未登录。

```java
public static final int SDK_STATE_REGISTERING = 0x0002; 
```

SDK 正在注册用户。

```java
public static final int SDK_STATE_UNREGISTERING = 0x0003;  
```

SDK 正在注销用户。

```java
public static final int SDK_STATE_LOGINING = 0x0004;
```

用户正在登录。

```java
public static final int SDK_STATE_LOGOUTING = 0x0005;
```

用户正在登出。

```java
public static final int SDK_STATE_CODEREQING = 0x0006;
```

SDK 正在请求验证码。

```java
public static final int SDK_STATE_PSWDCHGING = 0x0007;
```

SDK 正在修改账号密码。

```java
public static final int SDK_STATE_PSWDRESETING = 0x0008;
```

SDK 正在重置账号密码。

```java
public static final int SDK_STATE_USRINFO_QUERYING = 0x0009;
```

SDK 正在查询用户信息。

```java
public static final int SDK_STATE_USRINFO_UPDATING = 0x000A;  
```

SDK 正在更新用户信息。

```java
public static final int SDK_STATE_RUNNING = 0x000B;
```

用户已经登录。

<a id="InitParam"></a>

## InitParam 类

```java
public static class InitParam
```

SDK 初始化参数。

### 参数

| 参数 | 描述 |
| --- | --- |
| `mContext` | Android activity 的上下文。 |
| `mRtcAppId` | Agora 为 app 开发者签发的 App ID，详见[获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token#get-an-app-id)。使用同一个 App ID 的 SDK 才能互通。 |
| `mMetaData` | 元数据。 |
| `mLogFilePath` | 日志文件路径，必须为绝对路径且包含文件名。 |
| `mPublishAudio` | 是否发布音频。<ul><li>true：发布音频。</li><li>false：不发布音频。</li></ul> |
| `mPublishVideo` | 是否发布视频。<ul><li>true：发布视频。</li><li>false：不发布视频。</li></ul> |
| `mSubscribeAudio` | 是否订阅音频。<ul><li>true：订阅音频。</li><li>false：不订阅音频。</li></ul> |
| `mSubscribeVideo` | 是否订阅视频。<ul><li>true：订阅视频。</li><li>false：不订阅视频。</li></ul> |

## public 方法

### initialize 方法

```java
int initialize(InitParam initParam)
```

初始化 SDK。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `initParam` | 初始化参数。详见 [InitParam](#InitParam)。 |

#### 返回

- 0：方法调用成功
- < 0：方法调用失败。

### release 方法

```java
void release()
```

释放 SDK 所有资源。

### getStateMachine 方法

```java
int getStateMachine();
```

获取当前 SDK 状态。

#### 返回

SDK 状态。详见 [IAgoraIotAppSdk](#IAgoraIotAppSdk) 接口的常量。

### getAccountMgr 方法

```java
IAccountMgr getAccountMgr()
```

获取 `IAccountMgr` 对象用于账号管理。

#### 返回

一个 `IAccountMgr` 对象。

### getCallkitMgr 方法

```java
ICallkitMgr getCallkitMgr()
```

获取 `ICallkitMgr` 对象用于呼叫管理。

#### 返回

一个 `ICallkitMgr` 对象。

### getDeviceMgr 方法

```java
IDeviceMgr getDeviceMgr()
```

获取 `IDeviceMgr` 对象用于呼叫管理。

#### 返回

一个 `IDeviceMgr` 对象。

### getAlarmMgr 方法

```java
IAlarmMgr getAlarmMgr()
```

获取 `IAlarmMgr` 对象用于告警管理。

#### 返回

一个 `IAlarmMgr` 对象。

### getDevMessageMgr 方法

```java
IDevMessageMgr getDevMessageMgr()
```

获取 `IDevMessageMgr` 对象用于设备信息管理。

#### 返回

一个 `IDevMessageMgr` 对象。

### getNotificationMgr 方法

```java
INotificationMgr getNotificationMgr()
```

获取 `INotificationMgr` 对象用于通知消息管理。

#### 返回

一个 `INotificationMgr` 对象。

### getPlaybackMgr 方法

```java
IPlaybackMgr getPlaybackMgr()
```

获取 `IPlaybackMgr` 对象用于媒体播放管理。

#### 返回

一个 `IPlaybackMgr` 对象。

# IAccountMgr 接口

## 常量

```java
public static final int ACCOUNT_STATE_IDLE = 0x0000;
```

用户没有登录。

```java
public static final int ACCOUNT_STATE_REGISTERING = 0x0001; 
```

SDK 正在注册用户账号。

```java
public static final int ACCOUNT_STATE_UNREGISTERING = 0x0002;
```

SDK 正在注销用户账号。

```java
public static final int ACCOUNT_STATE_LOGINING = 0x0003;     
```

SDK 正在登录用户账号。

```java
public static final int ACCOUNT_STATE_LOGOUTING = 0x0004; 
```

SDK 正在登出用户账号。

```java
public static final int ACCOUNT_STATE_PSWDCHGING = 0x0005;
```

SDK 正在修改用户账号密码。

```java
public static final int ACCOUNT_STATE_PSWDRESETING = 0x0006;
```

SDK 正在重置用户账号密码。

```java
public static final int ACCOUNT_STATE_CODE_REQING = 0x0007;
```

SDK 正在请求验证码。

```java
public static final int ACCOUNT_STATE_USRINF_QUERYING = 0x0008;
```

SDK 正在查询用户信息。

```java
public static final int ACCOUNT_STATE_USRINF_UPDATING = 0x0009;
```

SDK 正在更新用户信息。

```java
public static final int ACCOUNT_STATE_RUNNING = 0x000A; 
```

用户已经登录。

<a id ="AccountInfo"></a>
## AccountInfo 类

```java
public static class AccountInfo
```

### 参数

/// TODO: Interface subject to changes. Some params will be removed.

| 参数 | 描述 |
| --- | --- |
| `mAccount` |  |
| `mEndpoint` |  |
| `mRegion` |  |
| `mGranwinToken` |  |
| `mExpiration` | |
| `mRefresh` |  |
| `mPoolIdentifier` |  |
| `mPoolIdentityId` |  |
| `mPoolToken` |  |
| `mIdentityPoolId` |  |
| `mProofAccessKeyId` |  |
| `mProofSecretKey` |  |
| `mProofSessionToken` |  |
| `mProofSessionExpiration` |  |
| `mInventDeviceName` |  |
| `mAgoraScope` |  |
| `mAgoraTokenType` |  |
| `mAgoraAccessToken` |  |
| `mAgoraRefreshToken` |  |
| `mAgoraExpriesIn` |  |

<a id ="UserInfo"></a>
## UserInfo 类

```java
public static class UserInfo
```

### 参数

/// TODO: Interface subject to changes. Some params will be removed.

| 参数 | 描述 |
| --- | --- |
| `mId` |  |
| `mAccount` |  |
| `mEmail` |  |
| `mPhoneNumber` |  |
| `mStatus` | |
| `mDeleted` |  |
| `mIdentityId` |  |
| `mIdentityPoolId` |  |
| `mMerchantId` |  |
| `mMerchantName` |  |
| `mCreateBy` |  |
| `mCreateTime` |  |
| `mUpdateBy` |  |
| `mUpdateTime` |  |
| `mParam` |  |
| `mName` |  |
| `mAvatar` |  |
| `mSex` |  |
| `mAge` |  |
| `mBirthday` |  |
| `mHeight` |  |
| `mWeight` |  |
| `mBackground` |  |
| `mCountryId` |  |
| `mCountry` |  |
| `mProvinceId` |  |
| `mProvince` |  |
| `mCityId` |  |
| `mCity` |  |
| `mAreaId` |  |
| `mArea` |  |
| `mAddress` |  |

<a id="ICallback"></a>
## ICallback 接口

```java
public static interface ICallback
```

账号回调。

### onGetCodeDone 回调

```java
default void onGetCodeDone(int errCode, String account) {}
```

账号获取验证码回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onRegisterDone 回调

```java
default void onRegisterDone(int errCode, String account) {}
```

账号注册完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onUnregisterDone 回调

```java
default void onUnregisterDone(int errCode, String account) {}
```

账号注销完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |


### onLoginDone 回调

```java
default void onLoginDone(int errCode, String account) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onLogoutDone 回调

```java
default void onLogoutDone(int errCode, String account) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onLoginOtherDevice 回调

```java
default void onLoginOtherDevice(String account) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onPasswordChangeDone 回调

```java
default void onPasswordChangeDone(int errCode, String account) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onPasswordResetDone 回调

```java
default void onPasswordResetDone(int errCode, String account) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `account` | 用户账号。 |

### onQueryUserInfoDone 回调

```java
default void onQueryUserInfoDone(int errCode, UserInfo userInfo) {}
```

账号登录完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。详见 [ErrorCode](#ErrorCode)。 |
| `userInfo` | 用户信息。详见 [UserInfo](#UserInfo)。 |

### onTokenInvalid 回调

```java
default void onTokenInvalid() {}
```

Token 过期回调。你需要重新登录。

# IAlarmMgr 接口

```java
public interface IAlarmMgr 
```

告警信息管理。

## 常量

```java
public static final int ALARMMGR_STATE_IDLE = 0x0001; 
```

空闲状态。

```java
public static final int ALARMMGR_STATE_ADDING = 0x0002; 
```

正在添加告警信息。

```java
public static final int ALARMMGR_STATE_DELETING = 0x0003;
```

正在删除告警信息。

```java
public static final int ALARMMGR_STATE_MARKING = 0x0004;
```

正在标记告警信息为已读。

```java
public static final int ALARMMGR_STATE_QUERYING = 0x0005; 
```

正在查询中。

## public 方法

### getStateMachine 方法

```java
int getStateMachine();
```

获取当前账号的告警状态。

#### 返回

当前账号的告警状态。

### registerListener 方法

```java
int registerListener(IAlarmMgr.ICallback callback);
```

注册回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `callback` | 回调接口。详见 [ICallback](#ICallback)。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### unregisterListener 方法

```java
int unregisterListener(IAlarmMgr.ICallback callback);
```

注销回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `callback` | 回调接口。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### insert 方法

```java
int insert(InsertParam insertParam);
```

插入告警信息。触发 [onAlarmAddDone](#onAlarmAddDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `insertParam` | 要插入的告警信息。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### delete 方法

```java
int delete(List<Long> alarmIdList);
```

插入告警信息。触发 [onAlarmDeleteDone](#onAlarmDeleteDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `alarmIdList` |  要删除的告警信息 ID 列表。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### mark 方法

```java
int mark(List<Long> alarmIdList);
```

标记多个告警信息为已读，触发 [onAlarmMarkDone](#onAlarmMarkDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `alarmIdList` |  要标记的告警信息 ID 列表。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### queryById 方法

```java
int queryById(long alarmId);
```

根据告警 ID 查询详细的告警信息，触发 [onAlarmInfoQueryDone](#onAlarmInfoQueryDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `alarmId` |  要标记的告警信息 ID。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### queryByPage 方法

```java
int queryByPage(QueryParam queryParam);
```

查询指定页面的设备列表，触发 [onAlarmInfoQueryDone](#onAlarmInfoQueryDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `queryParam` |  查询参数。详见 [QueryParam](#QueryParam)。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

### queryNumber 方法

```java
int queryNumber(QueryParam queryParam);
```

根据条件查询所有告警数量，触发 [onAlarmInfoQueryDone](#onAlarmInfoQueryDone) 回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `queryParam` |  查询参数。详见 [QueryParam](#QueryParam)。 |

#### 返回

错误码。

- 0：成功。
- <0：失败。

<a id="InsertParam"></a>
## InsertParam 类

```java
    public static class InsertParam {
        public String mProductId;   ///< 产品Id
        public String mDeviceId;    ///< 设备Id
        public String mDeviceName;  ///< 设备名
        public String mMessage;     ///< 告警消息内容
        public int mMsgType;        ///< 告警消息类型，{0:声音检测, 1:移动侦测，99：其他}
        public int mMsgStatus;      ///< 默认是0，{0：未读, 1：已读}
        public String mFileUrl;     ///< 录像文件Url

        @Override
        public String toString() {
            String infoText = "{ mProductId=" + mProductId
                    + ", mDeviceId=" + mDeviceId
                    + ", mDeviceId=" + mDeviceId
                    + ", mDeviceName=" + mDeviceName
                    + ", mMsgType=" + mMsgType
                    + ", mMsgStatus=" + mMsgStatus
                    + ", mFileUrl=" + mFileUrl + " }";
            return infoText;
        }
    }
```

告警消息插入参数。

### 参数

| 参数 | 描述 |
| --- | --- |
| `mProductId` | 产品 ID。 |
| `mDeviceId` | 设备 ID。 |
| `mDeviceName` | 设备名称。 |
| `mMessage` | 告警消息内容。 |
| `mMsgType` | 告警消息类型。<ul><li>0：声音检测。</li><li>1：移动侦测。</li><li>99：其他。</li></ul> |
| `mMsgStatus` | 消息状态。<ul><li>0：未读。</li><li>1：已读。</li></ul> |
| `mFileUrl` | 录像文件 URL。 |

<a id="QueryParam"></a>
## QueryParam 类

```java
public static class QueryParam {
    public String mProductId;   
    public String mDeviceId;    
    public int mMsgType;       
    public int mMsgStatus;      
    public String mBeginDate;   
    public String mEndDate;    
    public int mPageIndex;      
    public int mPageSize;     
    public boolean mAscSort;   

    @Override
    public String toString() {
        String infoText = "{ mProductId=" + mProductId
                + ", mDeviceId=" + mDeviceId
                + ", mMsgType=" + mMsgType
                + ", mMsgStatus=" + mMsgStatus
                + ", mBeginDate=" + mBeginDate
                + ", mEndDate=" + mEndDate
                + ", mPageIndex=" + mPageIndex
                + ", mPageSize=" + mPageSize + " }";
        return infoText;
    }
};
```

告警信息页查询参数。

### 参数

| 参数 | 描述 |
| --- | --- |
| `mProductId` | 产品 ID。 |
| `mDeviceId` | 设备 ID。 |
| `mMsgType` | 告警消息类型。<ul><li>0：声音检测。</li><li>1：移动侦测。</li><li>99：其他。</li></ul> |
| `mMsgStatus` | 消息状态。<ul><li>0：未读。</li><li>1：已读。</li></ul> |
| `mFileUrl` | 录像文件 URL。 |
| `mBeginDate` | 开始时间。 |
| `mEndDate` | 结束时间。 |
| `mPageIndex` | 要查询的页面索引，从 1 开始。 |
| `mPageSize` | 页面告警数量大小。 |
| `mAscSort` | 结果是否以升序排序。 |

## ICallback 接口

告警信息回调接口。

```java
public static interface ICallback {
        default void onReceivedAlarm(IotAlarm alarm) {}
        default void onAlarmAddDone(int errCode,  InsertParam insertParam) {}
        default void onAlarmDeleteDone(int errCode, List<Long> deletedIdList) {}
        default void onAlarmMarkDone(int errCode, List<Long> markedIdList) {}
        default void onAlarmInfoQueryDone(int errCode, final IotAlarm iotAlarm) {}
        default void onAlarmPageQueryDone(int errCode, final QueryParam queryParam, final IotAlarmPage alarmPage) {}
        default void onAlarmNumberQueryDone(int errCode, final QueryParam queryParam, long alarmNumber) {}
    }
```

### onReceivedAlarm

接收到一个新的告警事件。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `alarm` | 告警消息。 |

<a id="onAlarmAddDone"></a>
### onAlarmAddDone

插入告警完成事件。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `insertParam` | 插入的告警参数。详见 [InsertParam](#InsertParam)。 |

<a id="onAlarmDeleteDone"></a>
### onAlarmDeleteDone

告警删除完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `deletedIdList` | 删除的告警信息 ID 列表。 |

<a id="onAlarmMarkDone"></a>
### onAlarmMarkDone

告警标记完成回调。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `markedIdList` | 标记的告警信息 ID 列表。 |

<a id="onAlarmInfoQueryDone"></a>
### onAlarmInfoQueryDone

根据 ID 查询到的告警信息。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `iotAlarm` | 查询到的告警信息。 |

<a id="onAlarmPageQueryDone"></a>
### onAlarmPageQueryDone

分页查询到的告警信息。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `alarmPage` | 查询到的告警信息页面。 |

<a id="onAlarmNumberQueryDone"></a>
### onAlarmNumberQueryDone

根据条件查询到的告警信息。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `errCode` | 错误码。<ul><li>0：成功。</li><li><0：失败。</li></ul> |
| `alarmNumber` | 查询到的告警数量。 |

