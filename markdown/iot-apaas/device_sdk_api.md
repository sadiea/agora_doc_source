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

### agora_iot_deinit

```c
void agora_iot_deinit(agora_iot_handle_t handle);
```

### agora_iot_push_video_frame

```c
int agora_iot_push_video_frame(agora_iot_handle_t handle, ago_video_frame_t *frame);
```

### agora_iot_push_audio_frame

```c
int agora_iot_push_audio_frame(agora_iot_handle_t handle, ago_audio_frame_t *frame);
```

## 宏定义

### __cplusplus

```c
#ifdef __cplusplus
extern "C" {
#endif
```

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

## 枚举类型

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

SDK 事件回调。

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

| 参数 | 描述 |
| --- | --- |
| `app_id` |  |
| `product_id` | 产品 ID。 |
| `device_id` | 设备 ID。 |
| `domain` |  |
| `root_ca` |  |
| `client_crt` |  |
| `client_key` |  |

# agora_iot_base.h

# agora_iot_call.h

# agora_iot_device_manager.h

# agora_iot_dp.h
