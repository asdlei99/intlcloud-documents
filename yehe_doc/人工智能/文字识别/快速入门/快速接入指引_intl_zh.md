## 操作场景
该指南指导您在开通文字识别服务后，通过 API 3.0 Explorer 在线接口调试页面调用文字识别 API 3.0 相关接口，并将该接口对应语言的 SDK 集成到项目。


## 前提条件
在调用文字识别相关接口前，您需要先 [申请开通对应的文字识别服务](https://console.cloud.tencent.com/ocr/general)。
申请成功后，进入文字识别 [API 3.0 Explorer 在线接口调试页面](https://console.cloud.tencent.com/api/explorer?Product=ocr&Version=2018-11-19&Action=GeneralBasicOCR&SignVersion=)，按照下面的操作步骤调用接口。

## 操作步骤
1. 左侧导航栏，选择需要调用的接口。
![](https://main.qcloudimg.com/raw/60d9789c50f2c76215a9c5b836636b27.jpg)
2. 填写个人密钥和输入参数中的所需内容。
![](https://main.qcloudimg.com/raw/5f736e1aa25757f39a220b0ed8f3b5ce.png)
 - Region 参数：域名中的地域信息，该参数决定访问的接入点，例如`ocr.ap-shanghai.tencentcloudapi.com`就是要访问上海这个接入点。公共参数 Region 决定的是要访问业务资源所在区，例如`Region=ap-beijing`就是要操作北京区的资源，如果域名中不指定地域信息则默认就近接入。就近接入可能会存在问题。如果解析不到 IP 会默认到广州地域里面。另外域名地域和公共参数 Region 可以不一致，但是可能会增加耗时。建议域名和公共参数 Region 选择相同的地域：华南地区(广州)，ap-guangzhou。
 - Config 是 String 类型，解析之后为 Json 格式。
![](https://main.qcloudimg.com/raw/470a4a1c8bd5c6d1aebf8abcebd45ba3.png)
3. 选择语言生成对应代码。
您填写左侧的参数值，然后生成代码，生成代码中的部分字段信息和填写内容是关联的。如需调整传入参数，需要在左侧修改参数值后重新生成代码。
4. 集成 SDK 到项目。
参考右上角的 SDK 使用说明，将 SDK 引入到项目，通过 【步骤3】生成的代码即可调用对应的接口。
![](https://main.qcloudimg.com/raw/5efed9d740f7a50ba40069c70806424c.png)

## 简易版 Demo（推荐使用）

```
const tencentcloud = require("../../../../tencentcloud-sdk-nodejs");

const OcrClient = tencentcloud.ocr.v20181119.Client;
const models = tencentcloud.ocr.v20181119.Models;

const Credential = tencentcloud.common.Credential;
const ClientProfile = tencentcloud.common.ClientProfile;
const HttpProfile = tencentcloud.common.HttpProfile;

let cred = new Credential(" SecretId ", " SecretKey ");
let httpProfile = new HttpProfile();
let clientProfile = new ClientProfile();
/*
推荐使用 V3 鉴权。当内容超过 1M 时，必须使用 V3 签名鉴权。除 Node SDK 外，其他语言 SDK 都支持 V3。
clientProfile.signMethod = "TC3-HMAC-SHA256";
*/
clientProfile.httpProfile = httpProfile;
let client = new OcrClient(cred, "ap-guangzhou", clientProfile);

let req = new models.IDCardOCRRequest();

req.ImageUrl = "[https://test.jpg](https://test.jpg/)";
req.CardSide = "FRONT";
let config = {"CropPortrait":true};
req.Config = JSON.stringify(config)

client.IDCardOCR(req, function(errMsg, response) {

    if (errMsg) {
        console.log(errMsg);
        return;
    }

    console.log(response.to_json_string());
		
	});
```
## 注意事项

- SDK 调用公共参数时只需要关注 Region 字段，建议域名和 Region 统一使用 “ap-guangzhou”。
- SecretId/SecretKey 生成地址：[访问管理 - API 密钥管理](https://console.cloud.tencent.com/cam/capi)。文字识别相关接口目前仅支持主账号调用，我们会尽快支持子账号调用。
- 图片/视频转 Base64时，需要去掉相关前缀`data:image/jpg;base64,`和换行符`\n`。
- 如果请求结果提示如下， 需要手动设置签名类型：

  ```
  [TencentCloudSDKException]message:AuthFailure.SignatureFailure-The provided credentials
  could not be validated because of exceeding request size limit, please use new signature 
  method `TC3-HMAC-SHA256`. requestId:719970d4-5814-4dd9-9757-a3f11ecc9b20
  ```

  设置签名类型：

  ```js
  clientProfile.setSignMethod("TC3-HMAC-SHA256"); // 指定签名算法（默认为 HmacSHA256）
  ```

  如果接口请求内容超过 1M，只能使用 V3 鉴权（TC3-HMAC-SHA256）。除 Node SDK 外，其他语言 SDK 都支持 V3。
- API 3.0 SDK 支持语言：Node、Python、Java、PHP、Go、.Net。其他语言如 C++ ，暂时不支持 SDK 方式调用，需要自行实现 V3 鉴权 进行接口调用，建议使用 [API 3.0 Explorer](https://console.cloud.tencent.com/api/explorer?Product=faceid&Version=2018-03-01&Action=GetActionSequence) 中签名串生成工具进行核验签名有效性。
![](https://main.qcloudimg.com/raw/bfc3170146bca20407ce18852b2fd045.png)





