云直播提供将直播画面进行录制并将文件存储到云点播中，可通过云点播对录制视频进行下载、预览等处理。本文将为您介绍如何创建、修改以及删除录制模板。
创建录制模板有以下两种方式：
- 通过云直播控制台创建录制模板，具体操作步骤请参见 [创建录制模板](#C_record)。
- 通过 API 创建录制模板，具体参数及示例说明请参见  [创建录制模板](https://intl.cloud.tencent.com/document/product/267/30845)。

## 注意事项
- 录制的视频文件默认保存至 [云点播](https://console.cloud.tencent.com/vod/overview) 控制台，建议提前开通点播服务，并可提前选购点播相关资源包，避免点播业务欠费停用，详细请参见 [点播快速入门](https://intl.cloud.tencent.com/document/product/266/8757)。
- 开启录制功能后请确保云点播服务处于正常使用状态。云点播服务未开通或账号欠费导致云点播服务停服等情况将影响直播无法进行录制，期间不会产生录制文件和录制费用。
- 直播过程中预计在录制结束5分钟左右可获取对应文件。例如，某直播从12:00开始录制，则12:35左右可获取12:00 - 12:30的对应片段，以此类推。
- 受限于音视频文件格式（FLV/MP4/HLS）对编码类型的支持，视频编码类型支持H.264，音频编码类型支持 AAC。
- 录制模板创建成功后，可与推流域名进行关联，相关文档可参见  [录制配置](https://intl.cloud.tencent.com/document/product/267/34224)，关联成功后约5分钟 - 10分钟生效。
- 若需了解生成的录制文件命名规则，请参见 [录制模板参数-VodFileName](https://intl.cloud.tencent.com/document/product/267/30767#RecordParam)。
- 模板绑定、修改和解绑均只影响更新后的直播流，已经在直播中的流不会受影响；直播中的流需要断流重推才会使用新的规则。


## 前提条件
- 已开通腾讯云直播服务，并添加 [推流域名](https://intl.cloud.tencent.com/document/product/267/35970)。
- 已开通 [云点播服务](https://intl.cloud.tencent.com/document/product/266/8757#.E6.AD.A5.E9.AA.A41.EF.BC.9A.E5.BC.80.E9.80.9A.E4.BA.91.E7.82.B9.E6.92.AD)。

<span id="C_record"></span>
## 创建录制模板
1. 登录云直播控制台，进入【功能配置】>[【直播录制】](https://console.cloud.tencent.com/live/config/record)。
2. 单击【+】设置基本信息，进行如下配置：
![](https://main.qcloudimg.com/raw/93c0660fe1c5eaf8bfc2a72e7c7944dc.png)
   <table>
   <thead><tr><th>配置项</th><th>配置描述</th></tr></thead>
   <tbody><tr>
   <td>默认模板</td>
   <td>直播录制文件默认模板类型，可选择  FLV、MP4、HLS 三种。</td>
   </tr><tr>
   <td>模板名称</td>
   <td>直播录制模板名称，可自定义（仅支持中文、英文、数字、_、-）。</td>
   </tr><tr>
   <td>模板描述</td>
   <td>直播录制模板介绍描述，可自定义（仅支持中文、英文、数字、_、-）。</td>
   </tr><tr>
   <td>录制文件类型</td>
   <td>录制视频针对直播原始码率录制，输出格式有  HLS、MP4、FLV 和 AAC 四种，其中 AAC 为纯音频录制。</td>
   </tr><tr>
   <td>单个录制文件时长（分钟）</td>
   <td><ul style="margin-bottom:0px">
       <li>录制 HLS 格式最长单个文件时长无限制，如果超出续录超时时间则新建文件继续录制。</li>
       <li>录制 MP4、FLV 或 AAC 格式单个文件时长限制为5分钟 - 120分钟。</li>
       </ul></td>
   </tr><tr>
   <td>文件保存时长（天）</td>
   <td>单个录制文件保存最大时长均为1080天，文件保存时长0为永久。</td>
   </tr><tr>
   <td>续录超时时长（秒）</td>
   <td>仅  HLS 格式支持文件推流中断续录，续录超时时长可设置为0s - 1800s。</td>
   </tr><tr>
   <td>录制至子应用</td>
   <td>支持录制至云点播 VOD 指定 <a href="https://console.cloud.tencent.com/vod/app-manage">子应用</a> 中，默认录制至账号主应用，仅支持写入状态为开启的子应用。</td>
   </tr>
   </tbody></table>
3. 单击【保存】即可。


<span id="conect"></span>
## 关联域名
1. 登录云直播控制台，进入【功能配置】>[【直播录制】](https://console.cloud.tencent.com/live/config/record)。
	- **直接关联域名：**单击左上方的【绑定域名】。
	- **新录制模板创建成功后关联域名：**[录制模板创建](#C_record) 成功后，单击提醒框中的【去绑定域名】。
1. 在域名绑定窗口中，选择您需绑定的**录制模板**及**推流域名**，单击【确定】即可绑定成功。

>? 支持通过单击【添加】为当前模板绑定多个推流域名。

<span id="unite"></span>
## 解除绑定

1. 登录云直播控制台，进入【功能配置】>[【直播录制】](https://console.cloud.tencent.com/live/config/record)。
2. 选择已关联域名的录制模板，单击【解绑】。
3. 确认是否解绑当前关联域名，单击【确定】即可解绑。


<span id="change"></span>
## 修改模板
1. 进入【功能配置】>[【直播录制】](https://console.cloud.tencent.com/live/config/record)。
2. 选择您已创建成功的录制模板，并单击右侧的【编辑】，即可进入修改模板信息。
3. 单击【保存】即可。

![](https://main.qcloudimg.com/raw/68f4dd1a6452a3e97e7ff4dea6493d7b.png)

<span id="delete"></span>
## 删除模板
1. 进入【功能配置】>【直播录制】。
2. 选择您已创建成功的录制模板，单击上方的![](https://main.qcloudimg.com/raw/220ada95a4b631349543cc8cde96226e.png)删除按钮。
3. 确认是否删除当前录制模板，单击【确定】即可成功删除。
![](https://main.qcloudimg.com/raw/e25f0566348f69e8c6afb64439c5e40f.png)

>! 
>- 若模板已被关联，需要先 [解除绑定](#unite)，才可以进行删除操作。
>- 控制台的录制模板管理为域名维度，暂时无法取消关联接口创建的规则，如果是通过录制管理接口关联指定流的，则需要通过调用 [删除录制规则](https://intl.cloud.tencent.com/document/product/267/30843) 解除关联。 


## 相关操作
**域名维度绑定**和**解绑**录制模板的具体操作及相关说明，请参见 [录制配置](https://intl.cloud.tencent.com/document/product/267/34224)。


## 相关问题
<span id="que1"></span>
#### 直播录制后生成的视频名称是按什么规则生成？
控制台创建的录制模板，回调后生成的录制文件按照默认拼接方式命名，格式为：
`{StreamID}*{StartYear}-{StartMonth}-{StartDay}-{StartHour}-{StartMinute}-{StartSecond}*{EndYear}-{EndMonth}-{EndDay}-{EndHour}-{EndMinute}-{EndSecond} `

其中：

| 占位符             | 含义          |
| ------------------ | ------------- |
| {StreamID}         | 流 ID          |
| {StartYear}        | 开始时间-年   |
| {StartMonth}       | 开始时间-月   |
| {StartDay}         | 开始时间-日   |
| {StartHour}        | 开始时间-小时 |
| {StartMinute}      | 开始时间-分钟 |
| {StartSecond}      | 开始时间-秒   |
| {EndYear}          | 结束时间-年   |
| {EndMonth}         | 结束时间-月   |
| {EndDay}           | 结束时间-日   |
| {EndHour}          | 结束时间-小时 |
| {EndMinute}        | 结束时间-分钟 |
| {EndSecond}        | 结束时间-秒   |
