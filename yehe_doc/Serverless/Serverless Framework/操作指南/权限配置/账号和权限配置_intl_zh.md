## 授权方式
Serverless Framework 在部署项目时，需要使用**扫码授权**或**密钥授权**，通过 [角色](https://intl.cloud.tencent.com/document/product/598/19420) 授权 Serverless Framework 去操作您的云产品资源。

### 扫码授权
执行 sls 操作命令时，会查询环境变量中是否有密钥信息，如果未配置密钥，则弹出二维码要求扫码授权。

- 主账号扫码后，一键授权即可部署。一键授权时获取的权限详情参考 [SLS_QcsRole 角色权限列表](#list )。
- 子账号如果需要进行扫码部署则需要进行策略授权配置。配置详情参考 [子账号权限配置](#2)。

扫码授权后，会生成临时密钥信息（过期时间15分钟）写入当前目录下的 .env 文件中：

```
TENCENT_APP_ID=xxxxxx     #授权账号的 AppId
TENCENT_SECRET_ID=xxxxxx  #授权账号的 SecretId
TENCENT_SECRET_KEY=xxxxxx #授权账号的 SecretKey
TENCENT_TOKEN=xxxxx       #临时 token
```

### 密钥授权

为了避免扫码授权过期进行重复授权，您可以采用密钥授权方式。在部署的根目录下创建 `.env` 文件，并配置腾讯云的 SecretId 和 SecretKey 信息：

```
# .env
TENCENT_SECRET_ID=xxxxxxxxxx #您账号的 SecretId
TENCENT_SECRET_KEY=xxxxxxxx #您账号的 SecretKey
```

 SecretId 和 SecretKey可以在 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 中获取 。 

为了账号安全性，密钥授权时建议使用子账号密钥。子账号必须授予相关权限才能进行部署。配置详情参考[子账号权限配置](#2)。



## 权限配置

 Serverless Framework 部署项目时需要通过 [角色授权](https://intl.cloud.tencent.com/document/product/598/19420)，说明如下：
-  需要授权账号有操作Serverless Framework产品的权限。
-  需要授予账号有调用角色的权限。
- 被调用的角色要有对应操作资源的策略。

### 主账号权限配置
主账号默认拥有操作Serverless Framework产品的权限和调用角色的权限。只需 [开通 Serverless Framework](https://console.cloud.tencent.com/sls ) 就会默认创建的 `SLS_QcsRole`  角色 。该角色拥有 Serverless Framework 在部署中所需要用到的关联产品的对应策略。 查看 [SLS_QcsRole角色权限列表](#list) 。

<span id="2"></span>
### 子账号权限配置
子账号没有默认操作权限，因此需要**主账号（或拥有授权操作的子账号）**进行如下授权操作：
1. [授权操作 Serverless Framework 产品的权限](#3)。
2. [授权调用 `SLS_QcsRole` 角色的权限](#4)。

>?SLS_QcsRole角色拥有 Serverless Framework 在部署中所需要用到的关联产品的对应策略。如您想要收紧策略，可参考[指定操作角色权限](#指定操作角色权限 ) 。

<span id="3"></span>
#### 授权操作 Serverless Framework 产品的权限

对子账号进行 Serverless Framewok 产品授权时，可以选择对全部资源授权和指定资源授权的方式。

##### 授予子账号 Serverless Framework 全部资源的操作权限

如果希望子账号对 Serverless Framework 拥有全部资源操作权限，可通过如下步骤操作：

1. 在 [CAM 用户列表](https://console.cloud.tencent.com/cam/user) 页，选取对应子账号，单击用户名称，进入用户详情页。
2. 单击【关联策略】，在添加策略页面单击【从策略列表中选取策略关联】。
3. 搜索并关联 `QcloudSLSFullAccess`，单击【下一步】。
4. 单击【确定】，即可授予子账号 Serverless Framework 所有资源的操作权限。
   策略语法如下：

```json
{
    "version": "2.0",
    "statement": [
        {
            "action": [
                "sls:*"
            ],
            "resource": "*",
            "effect": "allow"
        }
    ]
}
```

##### 授予子账号 Serverless Framework 特定资源的操作权限

如果希望子账号仅对 Serverless Framework 某些特定资源有操作权限，可通过如下步骤操作：

1. 在 [CAM 用户列表](https://console.cloud.tencent.com/cam/user) 页，选取对应子账号，单击用户名称，进入用户详情页。
2. 单击【关联策略】，在添加策略页面单击【从策略列表中选取策略关联】。
3. 并单击【新建自定义策略】，按策略语法创建一条自定义策略并关联到用户，参考策略语法如下：

```json
{
    "version": "2.0",
    "statement": [
        {
            "action": [
                "sls:*"
            ],
            "resource": "qcs::sls:ap-guangzhou::appname/${appname}/stagename/${stagename}",
            "effect": "allow"
        }
    ]
}
```

配置完毕后，则子账号仅对 `${appname}` 和 `${stagename}` 下的 Serverless 应用具有操作权限。

<span id="4"></span>
#### 授权调用 `SLS_QcsRole` 角色的权限

子账号需要主账号进行授权，才能拥有调用`SLS_QcsRole`  角色的权限。

1. 在 [CAM 用户列表](https://console.cloud.tencent.com/cam/user) 页，选取对应子账号，单击用户名称，进入用户详情页。
2. 单击【关联策略】，在添加策略页面单击【从策略列表中选取策略关联】。
3. 选择【新建自定义策略】>【按策略语法创建】>【空白模版】，填入如下内容，注意角色参数替换为您自己的 uin（账号 ID）：
```
{
    "version": "2.0",
    "statement": [
        {
            "action": [
                "cam:PassRole"
            ],
            "resource": [
                 "qcs::cam::uin/${填入账号的uin}:roleName/SLS_QcsRole"
            ],
            "effect": "allow"
        },
        {
            "resource": [
                "*"
            ],
            "action": [
                "name/sts:AssumeRole"
            ],
            "effect": "allow"
        }
    ]
}
```
4. 单击【确定】，即可授予子账号 SLS_QcsRole 的操作权限。

####  指定操作角色权限配置 

 除了授权调用角色 SLS_QcsRole 外，也可给子账号授权调用自定义角色。通过自定义角色中的细粒度权限策略，达到权限收缩的目的。查看 [指定操作角色配置](https://intl.cloud.tencent.com/document/product/1040/36819) 详情。

<span id="list"></span>
## SLS_QcsRole 角色权限列表 

| 策略                    | 描述                                  |      
| ----------------------- | ------------------------------------- | 
| QcloudCOSFullAccess     | COS（对象存储）全读写访问权限         |      
| QcloudSCFFullAccess     | SCF（云函数）全读写权限               |      
| QcloudSSLFullAccess     | SSL证书（SSL）全读写访问权限          |      
| QcloudTCBFullAccess     | TCB（云开发）全读写权限               |      
| QcloudAPIGWFullAccess   | APIGW（API网关）全读写权限            |         
| QcloudVPCFullAccess     | VPC（私有网络）全读写权限             |      
| QcloudMonitorFullAccess | Monitor（云监控）全读写权限           |      
| QcloudSLSFullAccess     | SLS（Serverless Framework）全读写权限 |      
| QcloudCDNFullAccess     | CDN（内容分发网络）全读写权限         |      
| QcloudCKafkaFullAccess  | CKafka（消息队列 CKafka）全读写权限   |  
| QcloudCodingFullAccess  | CODING DevOps 全读写访问权限    |
| QcloudPostgreSQLFullAccess | 云数据库 PostgreSQL 全读写访问权限 |
| QcloudAccessForSLSRole | 该策略供 Serverless Framework（SLS）服务角色（SLS_QCSRole）进行关联，用于 SLS 一键体验功能访问其他云服务资源。包含访问管理（CAM）相关操作权限。|

>!SLS（Serverless Framework）全读写权限为新版本 Serverless Framework 新增的权限。如您使用旧版本的密钥进行部署，如希望切换至新版本能力，则需要删除密钥并重新登录。



