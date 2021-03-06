登录 [即时通信 IM 控制台](https://console.cloud.tencent.com/im) ，单击目标应用卡片，在左侧导航栏选择【回调配置】，您可以根据实际业务需求配置回调 URL 以及启用哪些回调。

## 配置回调 URL

1. 在【回调配置】页面，单击回调URL配置区域的【编辑】。
2. 在弹出的回调 URL 配置对话框中，输入回调URL。
 >
 >- 回调 URL 必须以`http://`或`https://`开头。
 >- 若您暂未申请域名，可直接配置 IP，例如`http://123.123.123.123/imcallback`。
 >
3. 单击【确定】保存配置。

## 配置事件回调
1. 在【回调配置】页面，单击事件回调配置区域的【编辑】。
2. 在弹出的事件回调配置对话框中，勾选所需的回调。
3. 单击【确定】保存配置。

## 下载 HTTPS 双向认证证书
配置回调 URL 后，您可以在控制台下载 HTTPS 双向认证证书供后续使用。
>您可以根据实际需求配置双向认证，具体配置方法参见 [双向认证配置](https://intl.cloud.tencent.com/document/product/1047/34379)。

1. 在【回调配置】页面，单击回调 URL 配置区域的【HTTPS 双向认证证书下载】。
2. 在弹出的证书下载对话框中，单击【下载】。
3. 保存证书文件。


## 后续操作
配置回调 URL 并启用相应的事件回调后，您可以参考 [第三方回调](https://intl.cloud.tencent.com/document/product/1047/34354) 使用相应的回调功能，实时了解用户信息和操作。
