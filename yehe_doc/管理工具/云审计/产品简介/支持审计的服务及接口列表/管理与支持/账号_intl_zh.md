腾讯云账号中心提供账号信息管理、实名认证变更、账号安全管理等服务，用户在注册腾讯云账号后，通过账号中心控制台可以方便高效管理账号，减少维护时间成本。

下表为云审计支持的账号操作列表：

| 操作名称                   | 资源类型 | 事件名称                     |
| -------------------------- | -------- | ---------------------------- |
| 申请帐号注销               | account  | ApplyAccountDeactivation     |
| 帐号关联 “二次确认”        | account  | BindAccountByTicket          |
| 帐号关联 “Mail”            | account  | BindMailAccount              |
| 绑定 Token 卡              | account  | BindToken                    |
| 修改邮箱账号密码           | account  | ChangeMailPassword           |
| 账号登录        | account  | ConsoleLogin                 |
| 创建实名认证人脸核身 Token | account  | CreateAuthDetectToken        |
| 修改用户邮箱               | account  | ModifyMail                   |
| 修改用户手机号码           | account  | ModifyPhoneNum               |
| 查看 API 密钥明文          | account  | QueryKeyBySecretId           |
| 异地登录安全设置           | account  | SetOffsiteLoginFlag          |
| 设置安全保护               | account  | SetSafeAuthFlag              |
| 提交银行卡认证             | account  | SubmitBankAuthInfo           |
| 提交人脸核身实名认证       | account  | SubmitDetectAuthInfo         |
| 提交企业实名认证基本信息   | account  | SubmitEnterpriseBaseAuthInfo |
| 提交米大师实名认证         | account  | SubmitMidasAuthInfo          |
| 提交个人实名认证基本信息   | account  | SubmitPersonalBaseAuthInfo   |
| 提交充值认证               | account  | SubmitTopUpAuthInfo          |
| 帐号关联 “解绑”            | account  | UnbindAccount                |
| 解绑 Token 卡              | account  | UnbindToken                  |
| 更新企业联系人信息         | account  | UpdateEnterpriseContact      |