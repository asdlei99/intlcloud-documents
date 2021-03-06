### SSH 密钥登录与密码登录有何区别？
SSH 密钥是一种远程登录 Linux 服务器的方式，其原理是利用密钥生成器制作一对密钥（公钥和私钥）。将公钥添加到服务器，然后在客户端利用私钥即可完成认证并登录，这种方式更加注重数据的安全性，同时区别于传统密码登录方式的手动输入，又具有更高的便捷性。
目前 Linux 实例有密码和 SSH 密钥两种登录方式，Windows 实例目前只有密码登录一种方式。相关文档参考：
- [登录 Linux 实例](https://intl.cloud.tencent.com/document/product/213/5436)
- [登录 Windows 实例](https://intl.cloud.tencent.com/document/product/213/5435)

### 使用 SSH 密钥登录还可以同时使用密码登录吗？
用户 [使用 SSH 密钥登录 Linux 实例](https://intl.cloud.tencent.com/document/product/213/32501)，默认禁用密码登录，以提高安全性，所以密钥登录后用户将不能再使用密码登录。

### 忘记密码怎么办？
您可以重置密码，具体操作详情请参见 [重置实例密码](https://intl.cloud.tencent.com/document/product/213/16566)。

### 如何创建 SSH 密钥以及密钥丢失怎么办？
针对创建密钥问题，可参考 [创建 SSH 密钥](https://intl.cloud.tencent.com/document/product/213/16691) 进行创建。
针对密钥丢失问题，我们提供两种方法解决：
 - 通过云服务器的 [SSH 密钥控制台](https://console.cloud.tencent.com/cvm/sshkey) 创建新的密钥，并使用新的密钥绑定原有实例。
  1. [创建 SSH 密钥](https://intl.cloud.tencent.com/document/product/213/16691)。
  2. 待完成创建密钥后，进入 [云服务器实例控制台](https://console.cloud.tencent.com/cvm)。
  3. 选择待绑定密钥的原有实例，单击【更多】>【密码/密钥】>【加载密钥】，即可使用新的密钥登录实例。
 - 通过云服务器控制台重置密码，再使用新密码登录实例。具体操作详情请参见 [重置实例密码](https://intl.cloud.tencent.com/document/product/213/16566)。

### 如何将 SSH 密钥绑定/解绑服务器？

请参考 [SSH 密钥操作指南](https://intl.cloud.tencent.com/document/product/213/16691) **密钥绑定/解绑服务器**部分。

### 如何修改 SSH 密钥名称/描述？

请参考 [SSH 密钥操作指南](https://intl.cloud.tencent.com/document/product/213/16691) **修改 SSH 密钥名称/描述**部分。

### 如何删除 SSH 密钥？

请参考 [SSH 密钥操作指南](https://intl.cloud.tencent.com/document/product/213/16691) **删除 SSH 密钥**部分。

### SSH 密钥有哪些使用限制？

请参考 [SSH 密钥简介](https://intl.cloud.tencent.com/document/product/213/6092) **使用限制**部分。

### 使用 SSH 密钥无法登录 Linux 实例，如何排查？

请参考 [无法通过 SSH 方式登录 Linux 实例](https://intl.cloud.tencent.com/document/product/213/32486)。
