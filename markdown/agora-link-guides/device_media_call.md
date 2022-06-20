本文介绍如何使用 Agora 灵隼设备端 SDK 实现媒体传输与呼叫功能。

## 准备工作

在使用设备端 SDK 之前，你需要进行以下准备工作。

### 配置开发环境

环境要求详见[产品概述](agora_link_overview)。

### 获取 License 并通过 License 对设备鉴权

设备端 SDK 通过 License 对设备鉴权。License 与设备绑定，一个 License 在同一时间只能绑定一个设备。你需要联系 sales@agora.io 购买商业 License。

你可以参考以下示例代码激活购买的 License。

1. 定义以下常量：

    ```c
    #define PROTOCOL_HEADER "https://"
    #define WEBCLIENT_SESSION_CREATE(header_sz) webclient_session_create(header_sz, AGORA_LICENSE_CERT, AGORA_LICENSE_CERT_LEN)
    #else
    #define PROTOCOL_HEADER "http://"
    #define WEBCLIENT_SESSION_CREATE(header_sz) webclient_session_create(header_sz, NULL, 0)
    #endif
    #define LICENSE_SERVER_HOST_NAME "api.agora.io"
    #define LICENSE_SERVICE "/dev/v2/apps/"
    #define LICENSE_ACTIVATE "/licenses"

    #define HTTP_AUTH_BUF_LEN_MAX 128
    #define HTTP_PARAM_BUF_LEN_MAX 128
    #define HTTP_REQUEST_BUF_LEN_MAX 1024
    #define HTTP_BODY_BUF_LEN_MAX (HTTP_REQUEST_BUF_LEN_MAX / 2)
    #define HTTP_RESPONSE_BUF_LEN_MAX 4096

    #define DEFAULT_CREDENTIAL "YOUR_CREDENTIAL"
    ```

2. 向 Agora License 业务服务端发送 HTTPS 请求，激活购买的 License。

    ```c
    int agora_license_activate(const char *appid, const char *key, const char *secret, const char *client_id, char **cert) {
        int result = -1;
        char *uri = NULL;
        cJSON *root = NULL;
        char *response_buf = NULL;
        webclient_session *session = NULL;
        if (!appid || !key || !secret || !client_id || !cert) {
            printf("agora_license_activate args is wrong !\n");
            result = -1;
            goto EXIT;
        }

        /* Establish URI */
        int len = strlen(PROTOCOL_HEADER) + strlen(LICENSE_SERVER_HOST_NAME) + strlen(LICENSE_SERVICE) + strlen(appid) + strlen(LICENSE_ACTIVATE) + 1;
        uri = (char *)calloc(len + 1, sizeof(char));
        if (NULL == uri) {
            printf("cannot malloc for license URI !\n");
            result = -1;
            goto EXIT;
        }
        snprintf(uri, len, "%s%s%s%s%s", PROTOCOL_HEADER, LICENSE_SERVER_HOST_NAME, LICENSE_SERVICE, appid, LICENSE_ACTIVATE);
        printf("agora_license_activate uri: %s\n", uri);

        /* post HTTP request and receive HTTP response */
        session = WEBCLIENT_SESSION_CREATE(HTTP_REQUEST_BUF_LEN_MAX);
        if (NULL == session) {
            printf("webclient_session_create for license failed !\n");
            result = -1;
            goto EXIT;
        }
        
        char basic_auth[128] = {0};
        char basic_auth_base64[128] = {0};
        size_t ret_len = 0;
        snprintf(basic_auth, sizeof(basic_auth) - 1, "%s:%s", key, secret);
        if (0 != mbedtls_base64_encode((unsigned char *)basic_auth_base64, sizeof(basic_auth_base64), &ret_len, 
                                    (const unsigned char *)basic_auth, strlen(basic_auth))) {
            printf("mbedtls_base64_encode failed!\n");
            result = -1;
            goto EXIT;
        }
        webclient_header_fields_add(session, "Authorization: Basic %s\r\n", basic_auth_base64);
        char body[256] = {0};
        snprintf(body, sizeof(body) - 1, "{\"credential\":\"%s\",\"custom\":\"%s\"}", DEFAULT_CREDENTIAL, client_id);
        webclient_header_fields_add(session, "Content-Length: %d\r\n", strlen(body));
        webclient_header_fields_add(session, "Content-Type: application/json\r\n");
        if ((result = webclient_post(session, uri, body, strlen(body))) < 0) {
            printf("post failed: %d\n", result);
            result = -1;
            goto EXIT;
        }
        response_buf = (char *)calloc(HTTP_RESPONSE_BUF_LEN_MAX, sizeof(char));
        if (NULL == response_buf) {
            printf("cannot malloc for license response !\n");
            result = -1;
            goto EXIT;
        }
        len = webclient_read(session, response_buf, HTTP_RESPONSE_BUF_LEN_MAX);
        printf("response length: %d\n", len);
        if (len <= 0) {
            result = -1;
            goto EXIT;
        }
        printf("response data: \n%s\n", response_buf);

        /* parse the json format of response's body */
        root = cJSON_Parse(response_buf);
        if (NULL == root) {
            printf("cannot parse http responce: \n%s\n", response_buf);
            result = -1;
            goto EXIT;
        }
        if (0 != json_read_item_string(root, "cert", cert)) {
            printf("cannot found cert in responce: \n");
            result = -1;
            goto EXIT;
        }
        result = 0;

    EXIT:
        if (root) {
            cJSON_Delete(root);
        }
        webclient_close(session);
        if (uri) {
            free(uri);
        }
        if (response_buf) {
            free(response_buf);
        }
        return result;
    }
    ```

## 初始化 SDK

参考以下步骤调用 `agora_iot_init` 初始化 SDK。

```c
  agora_iot_rtc_callback_t rtc_cb = {
      .cb_start_push_frame        = iot_cb_start_push_frame,
      .cb_stop_push_frame         = iot_cb_stop_push_frame,
      .cb_target_bitrate_changed  = iot_cb_target_bitrate_changed,
      .cb_key_frame_requested     = iot_cb_key_frame_request,
      .cb_receive_audio_frame     = iot_cb_receive_audio
  };
  agora_iot_call_callback_t call_cb = {
      .cb_call_request        = iot_cb_call_request,
      .cb_call_hung_up        = iot_cb_call_hung_up,
      .cb_call_answered       = iot_cb_call_answered,
      .cb_call_local_timeout  = iot_cb_call_local_timeout,
      .cb_call_peer_timeout   = iot_cb_call_peer_timeout
  };
  agora_iot_config_t cfg = { .app_id = CONFIG_AGORA_APP_ID,
                             .product_id = CONFIG_PRODUCT_KEY,
                             .device_id = device_id,
                             .domain = device_info.domain, // domain host for dp
                             .root_ca = CONFIG_AWS_ROOT_CA, // aws root ca buffer for dp
                             .client_crt = device_info.certificate, // client certificate buffer for dp
                             .client_key = device_info.private_key, //  client private key buffer for dp
                             .enable_rtc = true,
                             .certificate = CERTIFICATE,
                             .enable_recv_audio = true,
                             .enable_recv_video = true,
                             .enable_audio_config = true,
                             .audio_config = audio_config,
                             .rtc_cb = rtc_cb,
                             .call_cb = call_cb };
  g_iot_handle = agora_iot_init(&cfg);
  if (!g_iot_handle) {
    // if init failed, no need to deinit
    printf("#### agora_iot_init failed\n");
    goto EXIT;
  }
```

## 注册并绑定设备

参考以下步骤调用 SDK 的 `agora_iot_register_and_bind` 注册并绑定设备。

```c
  agora_iot_device_info_t device_info = { 0 };
  if (0 != agora_iot_register_and_bind(CONFIG_LINK_HOST_URL, CONFIG_PRODUCT_KEY, client_id, NULL, CONFIG_DEVICE_ID,
                                       &device_info)) {
    printf("register device to aws failure\n");
    return -1;
  }
```

## 实现媒体流传输

参考以下步骤实现媒体流传输功能。

1. 发送音视频。你需要自行使用当前设备的音视频采集 API 获取音视频帧。你还需要自行设置视频帧的发送帧率和音频帧的发送时长。设备端发送呼叫之后，在应用端 SDK 加入频道时，会在设备端触发 `iot_cb_start_push_frame` 回调。因此在与客户端 SDK 配合实现门铃场景时，一般通过 `iot_cb_start_push_frame` 和 `iot_cb_stop_push_frame` 回调触发音视频发送逻辑。

    ```c
    static void *video_push_func(void *threadid)
    {
    app_config_t *config = &g_app.config;
    int video_send_interval_ms = 1000 / config->send_video_frame_rate;
    void *pacer = pacer_create(video_send_interval_ms);

    while (g_app.b_call_session_ongoing) {
        app_send_video();
        // sleep and wait until time is up for next send
        wait_for_next_pace(pacer);
    }
    pacer_destroy(pacer);
    return NULL;
    }

    static void *audio_push_func(void *threadid)
    {
    int audio_send_interval_ms = DEFAULT_SEND_AUDIO_FRAME_PERIOD_MS;
    void *pacer = pacer_create(audio_send_interval_ms);

    while (g_app.b_call_session_ongoing) {
        app_send_audio();
        // sleep and wait until time is up for next send
        wait_for_next_pace(pacer);
    }
    pacer_destroy(pacer);
    return NULL;
    }
    ```

    `app_send_video` 和 `app_send_audio` 函数的具体实现调用了 SDK 的 `agora_iot_push_video_frame` 和 `agora_iot_push_audio_frame`：

    ```c
    static int app_send_video(void)
    {
    int rval;
    app_config_t *config = &g_app.config;

    frame_t frame;
    if (file_parser_obtain_frame(g_app.video_file_parser, &frame) < 0) {
        printf("#### The file parser failed to obtain video frame\n");
        return -1;
    }

    ago_video_frame_t ago_frame = { 0 };
    ago_frame.is_key_frame = frame.u.video.is_key_frame ? true : false;
    ago_frame.data_type = config->video_data_type;
    ago_frame.video_buffer = frame.ptr;
    ago_frame.video_buffer_size = frame.len;
    rval = agora_iot_push_video_frame(g_iot_handle, &ago_frame);
    if (rval < 0) {
        printf("#### Failed to push video frame\n");
        return -1;
    }

    file_parser_release_frame(g_app.video_file_parser, &frame);
    return 0;
    }

    static int app_send_audio(void)
    {
    int rval = 0;
    app_config_t *config = &g_app.config;

    frame_t frame;
    if (file_parser_obtain_frame(g_app.audio_file_parser, &frame) < 0) {
        printf("#### The file parser failed to obtain audio frame\n");
        return -1;
    }

    // API: send audio data
    ago_audio_frame_t ago_frame = { 0 };
    ago_frame.data_type = config->audio_data_type;
    ago_frame.audio_buffer = frame.ptr;
    ago_frame.audio_buffer_size = frame.len;
    rval = agora_iot_push_audio_frame(g_iot_handle, &ago_frame);
    if (rval < 0) {
        printf("#### Failed to push audio frame\n");
        return -1;
    }

    file_parser_release_frame(g_app.audio_file_parser, &frame);
    return 0;
    }
    ```

2. 通过 `iot_cb_receive_audio` 回调接收客户端发送的音频。当客户端有音频信号发送至设备端时，触发 `iot_cb_receive_audio` 回调。

    ```c
    static void iot_cb_receive_audio(ago_audio_frame_t *frame)
    {
    //printf("#### iot_cb_receive_audio %zu\n", frame->audio_buffer_size);

    int ret = 0;
    static FILE *fp = NULL;
    if (!fp) {
        fp = fopen("receive.bin", "wb");
    }

    ret = fwrite(frame->audio_buffer, 1, frame->audio_buffer_size, fp);
    }
    ```

## 实现呼叫

参考以下步骤实现呼叫功能。

1. 通过 `iot_cb_call_request` 向客户端发送呼叫。

    ```c
    static void iot_cb_call_request(const char *peer_name, const char *attach_msg)
    {
    if (!peer_name) {
        return;
    }

    printf("#### Get call from peer \"%s\", attach message: %s\n", peer_name, attach_msg ? attach_msg : "null");

    #if 0 // If you want to auto-answer, enable this
    agora_iot_answer(g_iot_handle);
    #endif
    }
    ```

2. 在设备端通过 `agora_iot_answer` 接听或 `agora_iot_hang_up` 挂断来自客户端的呼叫，或者通过 `agora_iot_alarm` 对客户端发送告警信息。

    ```c
    // 接听
    agora_iot_answer(g_iot_handle);
    // 挂断
    agora_iot_hang_up(g_iot_handle);
    // 发送告警信息
    agora_alarm_file_info_t file_info = {
        /* If always use the default image, you should not rename the image */
        .rename = false
      };
      if (0 != _get_file_info(&file_info)) {
        printf("#### Can not access the default image file\n");
        continue;
      }

      char extra_msg[] = "someone passes by";
      result = agora_iot_alarm(g_iot_handle, peer_id, extra_msg, AG_ALARM_TYPE_VAD, &file_info);
    ```

## 收尾工作

退出设备端应用时，你可以通过 `agora_iot_deinit` 回收 SDK 资源。

```c
static void app_signal_handler(int sig)
{
  switch (sig) {
  case SIGQUIT:
  case SIGABRT:
  case SIGINT:
    // deinit agora iot
    agora_iot_deinit(g_iot_handle);
    exit(0);
    break;
  default:
    printf("#### no handler, sig %d", sig);
  }
}
```