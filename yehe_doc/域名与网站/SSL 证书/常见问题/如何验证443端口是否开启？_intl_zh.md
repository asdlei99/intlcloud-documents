
### Linux 服务器如何验证443端口是否开启？
1. 请在 [腾讯云控制台](https://console.cloud.tencent.com/cvm/instance/index?rid=1) 登录您的 Linux 实例。
2. 请您在服务器终端中输入命令行 `lsof -i:443` 来验证是否开启。若有输出相关信息，说明已经开启。若没有输出相关信息，说明未开启。
 - 已开启状态。如下图所示：
![](https://main.qcloudimg.com/raw/66407c9fd01217b95d58631223d0e9e7.png)
 - 未开启状态。如下图所示：
![](https://main.qcloudimg.com/raw/651876723c7c40cde9552b87ea9f1567.png)

### Windows 服务器验证443端口是否开启？

1. 在 [腾讯云控制台](https://console.cloud.tencent.com/cvm/instance/index?rid=1) 登录您的 Windows 实例。
2. 在服务器终端中输入命令行 `netstat -ano -p tcp | find "443" >nul 2>nul && echo 443端口已开启 || echo 443端口未开启` 来验证是否开启。如果输出 “443端口已开启” ，说明已经开启。如果输出 “443端口未开启”，说明未开启。
 - 已开启状态。如下图所示：
![](https://main.qcloudimg.com/raw/941bc5fa4071a699011ec29b166ae2fe.jpg)
 - 未开启状态。如下图所示：
![](https://main.qcloudimg.com/raw/cdb01dd8cf5e7ef496f25fe8eaecdc7e.png)

