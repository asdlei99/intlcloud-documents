在开通了直播录制功能后，您可在直播录制回调模板中配置注册的回调域名，则云直播后台会将录制结果回调给您。

# 注意事项

- 阅读本文之前，希望您已经了解腾讯云直播是如何配置回调功能、您是如何接收回调消息的，具体请参见 [如何接收事件通知](https://intl.cloud.tencent.com/zh/document/product/267/38080)。
- 录制的视频文件默认保存至 [云点播](https://console.cloud.tencent.com/vod/overview) 控制台，建议提前开通点播服务，并可提前选购点播相关资源包，避免点播业务欠费停用。


## 录制事件参数说明

### 事件类型参数

| 事件类型 | 字段取值说明           |
| :------- | :------------- |
| 直播录制 | event_type = 100 |

### 回调公共参数
<table>
<tr><th>字段名称</th><th>类型</th><th>说明</th></tr>
<tr>
<td>t</td>
<td>int64</td>
<td>过期时间，事件通知签名过期 UNIX 时间戳。<ul style="margin:0"><li>来自腾讯云的消息通知默认过期时间是10分钟，如果一条消息通知中的 t 值所指定的时间已经过期，则可以判定这条通知无效，进而可以防止网络重放攻击。<li>t 的格式为十进制 UNIX 时间戳，即从1970年01月01日（UTC/GMT 的午夜）开始所经过的秒数。</ul></td>
</tr><tr>
<td>sign</td>
<td>string</td>
<td>事件通知安全签名 sign = MD5（key + t）。<br>说明：腾讯云把加密 <a href="#key">key</a> 和 t 进行字符串拼接后通过 MD5 计算得出 sign 值，并将其放在通知消息里，您的后台服务器在收到通知消息后可以根据同样的算法确认 sign 是否正确，进而确认消息是否确实来自腾讯云后台。</td>
</tr></table>

>? <span id="key"></span>key 为【功能模板】>[【回调配置】](https://console.cloud.tencent.com/live/config/callback)中的回调密钥，主要用于鉴权。为了保护您的数据信息安全，建议您填写。
>![](https://main.qcloudimg.com/raw/48f919f649f84fd6d6d6dd1d8add4b46.png)


### 回调消息参数

| 字段名称     | 类型   | 说明                                                 |
| :----------- | :----- | :--------------------------------------------------- |
| appid        | int    | 用户 [APPID](https://console.cloud.tencent.com/developer)                                           |
| stream_id    | string | 直播流名称                                           |
| channel_id   | string | 同直播流名称                                         |
| file_id      | string | 点播 file ID，在 [云点播平台](https://intl.cloud.tencent.com/zh/document/product/266/33895) 可以唯一定位一个点播视频文件 || file_format  | string | flv，hls，mp4，aac                                   |
| start_time   | int64  | 录制文件起始时间戳                                   |
| end_time     | int64  | 录制文件结束时间戳                                   |
| duration     | int64  | 录制文件时长，单位秒                                 |
| file_size    | uint64 | 录制文件大小，单位字节                               |
| stream_param | string | 用户推流 URL 所带参数                                |
| video_url    | string | 录制文件文件下载 URL                                 |

### 回调消息示例

```
{
"event_type":100,

"appid":12345678,

"stream_id":"stream_test",

"channel_id":"stream_test",

"file_id":"1234567890",

"file_format":"hls",

"start_time":1545047010,

"end_time":1545049971,

"duration":2962,

"file_size":277941079,

"stream_param":"stream_param=test",

"video_url":"http://12345678.vod2.myqcloud.com/xxxx/yyyy/zzzz.m3u8",

"sign":"ca3e25e5dc17a6f9909a9ae7281e300d",

"t":1545030873
}
```





