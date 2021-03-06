## 简介

本文档主要介绍 SDK 如何在请求时携带自定义头部。

## SDK API 参考

SDK 所有接口的具体参数与方法说明，请参考 [SDK API](https://cos-ios-sdk-doc-1253960454.file.myqcloud.com/)。

#### 功能说明

COS 在上传对象时可以携带以 `x-cos-meta-` 开头的自定义头部，包括用户自定义元数据头部后缀和用户自定义元数据信息，这些头部将作为对象元数据保存。

如果您开通了万象服务，可以携带 `Pic-Operations` 头部，实现后台自动图片处理，详细的 API 说明请参考 [数据万象持久化](https://intl.cloud.tencent.com/document/product/1045/33695)。

#### 示例代码
**Objective-C**

[//]: # (.cssg-snippet-set-custom-headers)
```objective-c
request.customHeaders[@"custom-key"] = @"custom-value";
```

>?更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Objc/Examples/cases/SetCustomHeaders.m) 查看。

**Swift**

[//]: # (.cssg-snippet-set-custom-headers)
```swift
request.customHeaders["custom-key"] = "custom-value";
```

>?更多完整示例，请前往 [GitHub](https://github.com/tencentyun/cos-snippets/tree/master/iOS/Swift/Examples/cases/SetCustomHeaders.swift) 查看。

