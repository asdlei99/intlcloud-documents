视频编辑，是对云点播中的视频进行剪辑和拼接的过程，是一种离线任务。视频编辑的功能包括以下几种：

* **视频剪辑**：对云点播中的一个文件进行剪辑，生成一个新的视频。
* **视频拼接**：对云点播中的多个文件进行拼接，生成一个新的视频。
* **视频剪辑后拼接**：对云点播中的多个文件进行剪辑，然后再拼接，生成一个新的视频。
* **直播流转视频**：对云点播中的一个流进行处理，生成一个新的视频。
* **流剪辑**：对云点播中的一个流进行剪辑，生成一个新的视频。
* **流拼接**：对云点播中的多个流进行拼接，生成一个新的视频。
* **流剪辑后拼接**：对云点播中的多个流进行剪辑，然后拼接，生成一个新的视频。

编辑后生成的新视频封装格式是 MP4。发起编辑时，可以指定是否对生成的新视频执行 [任务流](https://intl.cloud.tencent.com/document/product/266/33931#.E4.BB.BB.E5.8A.A1.E6.B5.81)。

## 任务发起

视频编辑任务，通过 [服务端 API](https://intl.cloud.tencent.com/document/product/266/34126) 方式发起。调用 API 的返回结果中包含任务 ID，用于关联 [结果获取](#.E7.BB.93.E6.9E.9C.E8.8E.B7.E5.8F.96) 时对应的任务结果。

## 结果获取

发起任务后，您可以通过异步等待 [结果通知](https://intl.cloud.tencent.com/document/product/266/33931#ResultNotification) 或同步进行 [任务查询](https://intl.cloud.tencent.com/document/product/266/33931#TaskQuery) 的方式获取编辑的执行结果。下面是发起视频编辑任务后，普通回调方式下结果通知的示例（省略了值为 null 的字段）：

```json
{
    "EventType":"EditMediaComplete",
    "EditMediaComplete":{
        "TaskId":"EditMedia-f5ac8127b3b6b85cdc13f237c6005d8",
        "Status":"FINISH",
        "ErrCode":0,
        "Message":"SUCCESS",
        "Input":{
            "InputType":"File",
            "FileInfoSet":[
                {
                    "FileId":"24961954183381008",
                    "StartTimeOffset":0,
                    "EndTimeOffset":300
                },
                {
                    "FileId":"24961954183381009",
                    "StartTimeOffset":0,
                    "EndTimeOffset":300
                },
                {
                    "FileId":"24961954183381010",
                    "StartTimeOffset":0,
                    "EndTimeOffset":300
                }
            ]
        },
        "Output":{
            "FileType":"mp4",
            "FileId":"24961954183923290",
            "FileUrl":"http://125676836723.vod2.myqcloud.com/xxx/xxx/f0.mp4"
        },
        "ProcedureTaskId":""
    }
}
```

回调结果中，`Input.InputType`为`File`，表示编辑的视频是文件类型。`Input.FileInfoSet`包含三个元素，其中`StartTimeOffset`为`0`，`EndTimeOffset`为`300`，表示对三个视频各剪辑前5分钟的片段后再做拼接，拼接后的视频时长为15分钟。`Output.FileId`是视频编辑后生成的新视频的 FileId，视频的播放 URL 是`FileUrl`中的值。
