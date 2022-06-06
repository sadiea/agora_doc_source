# agora_iot_api.h

## 引用文件

```c
#include "agora_iot_base.h"
#include "agora_iot_call.h"
#include "agora_iot_dp.h"
```

## 函数

### agora_iot_init

```c
agora_iot_handle_t agora_iot_init(const agora_iot_config_t *cfg);
```

初始化 SDK。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `cfg` | SDK 配置。详见 [agora_iot_config_t](#agora_iot_config_t)。 |

#### 返回值

SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。

### agora_iot_deinit

```c
void agora_iot_deinit(agora_iot_handle_t handle);
```

释放 SDK 的资源。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `handle` | [agora_iot_init](#agora_iot_init) 返回的 SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。 |

### agora_iot_push_video_frame

```c
int agora_iot_push_video_frame(agora_iot_handle_t handle, ago_video_frame_t *frame);
```

发送视频帧。

### agora_iot_push_audio_frame

```c
int agora_iot_push_audio_frame(agora_iot_handle_t handle, ago_audio_frame_t *frame);
```

发送音频帧。

## 常量

### AGORA_IOT_ACCOUNT_NAME_LENGTH

```c
#define AGORA_IOT_ACCOUNT_NAME_LENGTH 64
```

IoT 账号名长度。

### AGORA_IOT_CHANNEL_LENGTH

```c
#define AGORA_IOT_CHANNEL_LENGTH 64
```

IoT 频道名长度。

### AGORA_IOT_TOKEN_LENGTH

```c
#define AGORA_IOT_TOKEN_LENGTH 1024
```

IoT Token 长度。


## 类型定义

<a id="ago_av_data_type_e"></a>

### ago_av_data_type_e

音视频编码格式。

```c
typedef enum {
  AGO_VIDEO_DATA_TYPE_YUV420 = 0,
  AGO_VIDEO_DATA_TYPE_H264 = 1,
  AGO_VIDEO_DATA_TYPE_JPEG = 2,
  AGO_AUDIO_DATA_TYPE_PCM = 10,
  AGO_AUDIO_DATA_TYPE_OPUS = 11,
  AGO_AUDIO_DATA_TYPE_G711A = 12,
  AGO_AUDIO_DATA_TYPE_G711U = 13,
  AGO_AUDIO_DATA_TYPE_G722 = 14,
  AGO_AUDIO_DATA_TYPE_AACLC = 15,
  AGO_AUDIO_DATA_TYPE_HEAAC = 16,
} ago_av_data_type_e;
```

| 枚举值 | 描述 |
| --- | --- |
| `AGO_VIDEO_DATA_TYPE_YUV420` | 0：YUV420 |
| `AGO_VIDEO_DATA_TYPE_H264` | 1：H.264 |
| `AGO_VIDEO_DATA_TYPE_JPEG` | 2：JPEG |
| `AGO_AUDIO_DATA_TYPE_PCM` | 10：PCM |
| `AGO_AUDIO_DATA_TYPE_OPUS` | 11：Opus |
| `AGO_AUDIO_DATA_TYPE_G711A` | 12：G711A |
| `AGO_AUDIO_DATA_TYPE_G711U` | 13：G711U |
| `AGO_AUDIO_DATA_TYPE_G722` | 14：G722 |
| `AGO_AUDIO_DATA_TYPE_AACLC` | 15：AACLC |
| `AGO_AUDIO_DATA_TYPE_HEAAC` | 16：HEAAC |

<a id="ago_video_frame_t"></a>

### ago_video_frame_t

视频帧设置。

```c
typedef struct {
  ago_av_data_type_e data_type;
  bool is_key_frame;
  uint8_t *video_buffer;
  uint32_t video_buffer_size;
} ago_video_frame_t;
```

| 参数 | 描述 |
| --- | --- |
| `data_type` | 视频帧编码类型。 |
| `is_key_frame` | 该帧是否是关键帧。 <ul><li>true: 该帧是关键帧。</li><li>true: 该帧不是关键帧。</li></ul> |
| `video_buffer` | 视频帧缓冲区。 |
| `video_buffer_size` | 视频帧缓冲区大小。 |

<a id="ago_audio_frame_t"></a>

### ago_audio_frame_t

音频帧设置。

```c
typedef struct {
  ago_av_data_type_e data_type;
  uint8_t *audio_buffer;
  uint32_t audio_buffer_size;
} ago_audio_frame_t;
```

| 参数 | 描述 |
| --- | --- |
| `data_type` | 音频编码类型。 |
| `audio_buffer` | 音频帧缓冲区。 |
| `audio_buffer_size` | 音频帧缓冲区大小。 |

<a id="agora_iot_rtc_callback_t"></a>

### agora_iot_rtc_callback_t

```c
typedef struct agora_iot_rtc_callback {
  void (*cb_start_push_frame)(void);
  void (*cb_stop_push_frame)(void);
  void (*cb_receive_audio_frame)(ago_audio_frame_t *frame);
  void (*cb_receive_video_frame)(ago_video_frame_t *frame);
  void (*cb_key_frame_requested)(void);
  void (*cb_target_bitrate_changed)(uint32_t target_bps);
} agora_iot_rtc_callback_t;
```

SDK 音视频事件回调。

#### cb_start_push_frame

在 SDK 可以开始发送视频或音频帧时触发。该回调提醒你发送音视频帧。

#### cb_stop_push_frame

在 SDK 停止发送视频或音频帧时触发。该回调提醒你停止发送音视频帧。

#### cb_receive_audio_frame

在 SDK 接收到音频帧时触发。

| 参数 | 描述 |
| --- | --- |
| `frame` | 音频帧设置。详见 [ago_audio_frame_t](#ago_audio_frame_t)。 |

#### cb_receive_video_frame

在 SDK 接收到视频帧时触发。

| 参数 | 描述 |
| --- | --- |
| `frame` | 视频帧设置。详见 [ago_video_frame_t](#ago_video_frame_t)。 |

在 SDK 接收到视频帧时触发。

#### cb_key_frame_requested

在 SDK 需要你发送关键帧给对端时触发。该回调提醒你在本地生成一个新的关键帧并发送给对端。

#### cb_target_bitrate_changed

在 SDK 侦测到网络带宽变化时触发。你需要将发送码率调整为 SDK 推荐的值。

| 参数 | 描述 |
| --- | --- |
| `target_bps` | SDK 推荐你使用的码率 (bps)。 |

<a id="ago_audio_codec_type_e"></a>

### ago_audio_codec_type_e

音频编码格式。

```c
typedef enum {
  AGO_AUDIO_CODEC_DISABLED = 0,
  AGO_AUDIO_CODEC_TYPE_OPUS = 1,
  AGO_AUDIO_CODEC_TYPE_G722 = 2,
 AGO_AUDIO_CODEC_TYPE_G711A = 3,
 AGO_AUDIO_CODEC_TYPE_G711U = 4,
} ago_audio_codec_type_e;
```

| 枚举值 | 描述 |
| --- | --- |
| `AGO_AUDIO_CODEC_DISABLED` | 0：PCM |
| `AGO_AUDIO_DATA_TYPE_OPUS` | 1：Opus |
| `AGO_AUDIO_DATA_TYPE_G722` | 2：G722 |
| `AGO_AUDIO_DATA_TYPE_G711A` | 3：G711A |
| `AGO_AUDIO_DATA_TYPE_G711U` | 4：G711U |

<a id="agora_iot_audio_config_t"></a>

### agora_iot_audio_config_t

音频设置选项。

```c
typedef struct _agora_iot_audio_config {
  ago_audio_codec_type_e audio_codec_type;
  int pcm_sample_rate;
  int pcm_channel_num;
} agora_iot_audio_config_t;
```

| 参数 | 描述 |
| --- | --- |
| `audio_codec_type` | 音频编码类型。详见 [ago_audio_codec_type_e](#ago_audio_codec_type_e)。 |
| `pcm_sample_rate` | PCM 音频数据的采样率（Hz）。 |
| `pcm_channel_num` | PCM 音频数据的通道数。 |

<a id="agora_iot_config"></a>

### agora_iot_config

SDK 基本设置。

```c
typedef struct agora_iot_config {
  const char *app_id;
  const char *product_id;
  const char *device_id;
  const char *domain; 
  const char *root_ca;
  const char *client_crt; 
  const char *client_key;

  bool enable_rtc;
  const char *certificate; 
  bool enable_recv_audio;
  bool enable_recv_video;
  agora_iot_rtc_callback_t rtc_cb;

  bool disable_rtc_log; 
  uint32_t max_possible_bitrate;
  bool enable_audio_config;
  agora_iot_audio_config_t audio_config;

  agora_iot_call_callback_t call_cb;
} agora_iot_config_t;
```

<!-- TODO: 增加其他 .h 文件中的结构体文档链接与跑通示例项目文档链接 --> 

| 参数 | 描述 |
| --- | --- |
| `app_id` | Agora 为 app 开发者签发的 App ID，详见[获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token#get-an-app-id)。使用同一个 App ID 的 SDK 才能互通。 |
| `product_id` | 你的产品 ID。可以设置为任意值。 |
| `device_id` | 你的设备 ID。每个设备的设备 ID 必须是唯一的。 |
| `domain` |  设备端域名。可以从 `agora_iot_device_manager.h::agora_iot_register_and_bind` 方法返回的 `agora_iot_device_info_t::domain` 参数获取。 |
| `root_ca` | AWS 服务根证书。你需要将值设为示例项目中的 `CONFIG_AWS_ROOT_CA` 的值。 |
| `client_crt` | 设备端证书。可以从`agora_iot_device_manager.h::agora_iot_register_and_bind` 方法返回的 `agora_iot_device_info_t::certificate` 参数获取。 |
| `client_key` | 设备端私钥。可以从 `agora_iot_device_manager.h::agora_iot_register_and_bind` 方法返回的 `agora_iot_device_info_t::private_key` 参数获取。 |
| `enable_rtc` | 是否开启音视频传输。<ul><li>true：开启音视频传输。</li><li>false：关闭音视频传输。</li></ul> |
| `certificate` | 使用 Agora License 机制生成的 certificate。详见示例项目的 `license_activate.c` 文件。 |
| `enable_recv_audio` | 本地是否接收音频。<ul><li>true：接收音频。</li><li>false：不接收音频。</li></ul>  |
| `enable_recv_video` | 本地是否接收视频。<ul><li>true：接收视频。</li><li>false：不接收视频。</li></ul> |
| `rtc_cb` | 音视频传输事件回调。详见 `agora_iot_rtc_callback_t` 结构体。 |
| `disable_rtc_log` | 是否关闭日志。 <ul><li>true：关闭日志。</li><li>false：开启日志。</li></ul>|
| `max_possible_bitrate` | 可能出现的最大码率（bps）。 |
| `enable_audio_config` | 是否开启音频配置。 <ul><li>true：开启音频配置。你可以通过 `audio_config` 参数配置音频。</li><li>false：关闭音频配置。`audio_config` 参数的设置无效。</li></ul>|
| `audio_config` | 音频配置。详见 [agora_iot_audio_config_t](#agora_iot_audio_config_t) 结构体。 |
| `call_cb` | 呼叫事件回调。详见 `agora_iot_call.h::agora_iot_call_callback_t` 结构体。 |

# agora_iot_base.h

## 引用文件

无

## 函数

无

## 常量

无

## 类型定义

<a id="agora_iot_handle_t"></a>
### agora_iot_handle_t

```c
typedef void* agora_iot_handle_t;
```

SDK 的句柄。

<a id="agora_iot_error_e"></a>
### agora_iot_error_e

```c
typedef enum {
  ERR_SUCCESS = 0,
  ERR_FAILED = -1,
  ERR_INVALID_ARGUMENT = -2,
} agora_iot_error_e;
```

错误码。

| 错误码 | 描述 |
| --- | --- |
| `ERR_SUCCESS` | 0：调用成功。 |
| `ERR_FAILED` | -1：通用错误。 |
| `ERR_INVALID_ARGUMENT` | -2：参数无效。 |

# agora_iot_call.h

## 引用文件

```c
#include "agora_iot_base.h"
```

## 函数

<a id="agora_iot_call"></a>
### agora_iot_call

```c
agora_iot_call_result_e agora_iot_call(agora_iot_handle_t handle, const char *peer, const char *extra_msg);
```

呼叫远端用户。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `handle` | [agora_iot_init](#agora_iot_init) 返回的 SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。 |
| `peer` | 应答呼叫的远端用户账号。 |
| `attach_msg` | 呼叫的附加信息。 |

#### 返回

[agora_iot_call_result_e](#agora_iot_call_result_e) 中的枚举值。

<a id="agora_iot_answer"></a>
### agora_iot_answer

```c
agora_iot_call_result_e agora_iot_answer(agora_iot_handle_t handle);
```

应答远端用户发来的呼叫。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `handle` | [agora_iot_init](#agora_iot_init) 返回的 SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。 |

#### 返回

[agora_iot_call_result_e](#agora_iot_call_result_e) 中的枚举值。

<a id="agora_iot_hang_up"></a>
### agora_iot_hang_up

```c
agora_iot_call_result_e agora_iot_hang_up(agora_iot_handle_t handle);
```

拒绝远端用户发来的呼叫。

#### 参数

| 参数 | 描述 |
| --- | --- |
| `handle` | [agora_iot_init](#agora_iot_init) 返回的 SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。 |

#### 返回

[agora_iot_call_result_e](#agora_iot_call_result_e) 中的枚举值。

<!-- TODO: agora_iot_alarm 有错误码？ -->

<a id="agora_iot_alarm"></a>
### agora_iot_alarm

```c
int agora_iot_alarm(agora_iot_handle_t handle, const char *peer, const char *extra_msg, int8_t type);
```

向远端用户发送告警信息。


#### 参数

| 参数 | 描述 |
| --- | --- |
| `handle` | [agora_iot_init](#agora_iot_init) 返回的 SDK 句柄。详见 [agora_iot_handle_t](#agora_iot_handle_t)。 |
| `peer` | 远端用户账号。 |
| `extra_msg` | 附加信息。 |
| `type` | 告警类型。详见 [agora_iot_alarm_type_e](#agora_iot_alarm_type_e)。 |

#### 返回

- 0：方法调用成功。
- < 0：方法调用失败。


## 常量

无

## 类型定义

<a id="agora_iot_call_result_e"></a>
### agora_iot_call_result_e

```c
typedef enum {
  ERR_AG_CALL_SUCCESS            = ERR_SUCCESS,
  ERR_AG_CALL_FAILED             = ERR_FAILED,
  ERR_AG_CALL_INVALID_ARGUMENT   = ERR_INVALID_ARGUMENT,

  ERR_AG_CALL_PEER_BUSY          = -100001,
  ERR_AG_CALL_CAN_NOT_ANSWER     = -100002,
  ERR_AG_CALL_CAN_NOT_HANGUP     = -100003,
  ERR_AG_CALL_PEER_TIMEOUT       = -100004,
  ERR_AG_CALL_IS_CALLING         = -100005,
  ERR_AG_CALL_ILLEGAL_ANSWER     = -100006,
  ERR_AG_CALL_CAN_NOT_CALL_YOURSELF = -100007,
  ERR_AG_CALL_UNEXPECTEDLY_ERROR = -999999,
} agora_iot_call_result_e;
```

呼叫返回结果。

| 枚举值 | 描述 |
| --- | --- |
| `ERR_AG_CALL_SUCCESS` | 0：调用成功。 |
| `ERR_AG_CALL_FAILED` | -1：通用错误。 |
| `ERR_AG_CALL_INVALID_ARGUMENT` | -2：参数无效。 |
| `ERR_AG_CALL_PEER_BUSY` | -100001：本地用户无法重复呼叫。因为被呼叫的远端用户已经处于通话中。 |
| `ERR_AG_CALL_CAN_NOT_ANSWER` | -100002：本地用户无法应答呼叫。因为没有外来或发出的呼叫。|
| `ERR_AG_CALL_CAN_NOT_HANGUP` | -100003：本地用户无法挂断呼叫。因为本地用户不在呼叫中，也没有发送呼叫。 |
| `ERR_AG_CALL_PEER_TIMEOUT` | -100004：本地用户呼叫远端用户时等待超时。 |
| `ERR_AG_CALL_IS_CALLING` | -100005： 已经有远端用户呼叫本地用户或本地用户已呼叫远端用户。|
| `ERR_AG_CALL_ILLEGAL_ANSWER` | -100006：错误的应答操作。例如本地用户应答从本地发出的呼叫。 |
| `ERR_AG_CALL_CAN_NOT_CALL_YOURSELF` | -100007：呼叫错误。因为本地用户无法呼叫自己。 |
| `ERR_AG_CALL_UNEXPECTEDLY_ERROR` | -999999：未知错误。 |

<a id="agora_iot_alarm_type_e"></a>
### agora_iot_alarm_type_e

```c
typedef enum {
  AG_ALARM_TYPE_VAD           = 0,
  AG_ALARM_TYPE_MOD           = 1,
  AG_ALARM_TYPE_OTHERS        = 99
} agora_iot_alarm_type_e;
```

告警类型。

| 枚举值 | 描述 |
| --- | --- |
| `AG_ALARM_TYPE_VAD` | 0：语音监测。 |
| `AG_ALARM_TYPE_MOD` | 1：动作监测。 |
| `AG_ALARM_TYPE_OTHERS` | 99：其他告警类型。 |

<a id="agora_iot_call_callback"></a>
### agora_iot_call_callback

```c
typedef struct agora_iot_call_callback {
  void (*cb_call_request)(const char *peer_id, const char *attach_msg);
  void (*cb_call_answered)(const char *peer_id);
  void (*cb_call_hung_up)(const char *peer_id);
  void (*cb_call_local_timeout)(const char *peer_id);
  void (*cb_call_peer_timeout)(const char *peer_id);
} agora_iot_call_callback_t;
```

SDK 呼叫事件回调。

#### cb_call_request

在本地用户接收到远端用户呼叫时触发。

| 参数 | 描述 |
| --- | --- |
| `peer_id` | 发送呼叫的远端用户账号。 |
| `attach_msg` | 呼叫的附加信息。 |

#### cb_call_answered

本地用户发出的呼叫被远端用户应答时触发。

| 参数 | 描述 |
| --- | --- |
| `peer_id` | 应答呼叫的远端用户账号。 |

#### cb_call_hung_up

本地用户发送的呼叫被远端用户拒绝。

| 参数 | 描述 |
| --- | --- |
| `peer_id` | 拒绝呼叫的远端用户账号。 |

<!-- TODO: 超时时间是多少？ --> 
#### cb_call_local_timeout

远端用户呼叫本地用户时，由于本地用户没有应答或拒绝操作导致超时。

| 参数 | 描述 |
| --- | --- |
| `peer_id` | 拒绝呼叫的远端用户账号。 |

#### cb_call_peer_timeout

本地用户呼叫远端用户时，由于远端用户没有应答或拒绝操作导致超时。

| 参数 | 描述 |
| --- | --- |
| `peer_id` | 拒绝呼叫的远端用户账号。 |




# agora_iot_device_manager.h



# agora_iot_dp.h
