为提升系统安全性，要求对目标凭据具备依赖性的应用配置同步更新。当多种应用系统在本地存储凭据内容时，在凭据更新时容易遗漏，从而带来应用中断风险。使用凭据管理系统，可以实现凭据管理系统内一处凭据更新，处处生效。此外，还可以为 [凭据创建配置多个版本](https://intl.cloud.tencent.com/document/product/1078/38603) ，实现凭据的灰度更新和轮换。
![](https://main.qcloudimg.com/raw/4b19949912a67d9984e2c55a18583969.png)
可以使用以下两种方式进行凭据轮换：

  - **方式一**：增加新的凭据版本，业务侧通过更新获取凭据的版本号实现灰度轮换，详情可参见 [多版本管理](https://intl.cloud.tencent.com/document/product/1078/38603)。
![](https://main.qcloudimg.com/raw/7a15a11f9ecfde828f8979c5a691b942.png)

- **方式二**：直接修改当前使用凭据的内容，业务侧下次调用接口获取凭据时，会自动更新凭据内容，详情可以参见 [凭据相关调用示例](https://intl.cloud.tencent.com/document/product/1078/38616)。
