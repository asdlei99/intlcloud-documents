创建负载均衡实例后，您需要为实例配置监听器。监听器负责监听负载均衡实例上的请求，并依据均衡策略来分发流量至后端服务器上。

负载均衡监听器需配置：
1. 监听协议和监听端口，负载均衡的监听端口，亦被称为前端端口，用来接收请求并向后端服务器转发请求的端口。
2. 监听策略，如均衡策略、[会话保持](https://intl.cloud.tencent.com/document/product/214/6154) 等。
3. [健康检查](https://intl.cloud.tencent.com/document/product/214/6097) 策略。
4. 绑定后端服务，选择后端服务器的 IP 和端口，服务端口亦被称为后端端口，后端服务用来接收请求的端口。

## 支持的协议类型
负载均衡监听器可以监听负载均衡实例上的四层和七层请求，并将这些请求分发到后端服务器上，而后由后端服务器处理请求。四层和七层负载均衡的区别主要体现在：对用户请求进行负载均衡时，是依据四层协议还是七层协议来进行转发流量。
- 四层协议：传输层协议，主要通过 VIP + Port 接受请求并分配流量到后端服务器。
- 七层协议：应用层协议，基于 URL、HTTP 头部等应用层信息进行流量分发。

腾讯云负载均衡支持以下协议的请求转发：
- TCP（传输层）
- UDP（传输层）
- TCP SSL（传输层）
- HTTP（应用层）
- HTTPS（应用层）
>TCP SSL 监听器正在内测中，当前仅支持公网负载均衡（不支持内网），不支持传统型负载均衡，如需使用，请通过  [工单申请](https://console.cloud.tencent.com/workorder/category/create?level1_id=6&level2_id=163&level1_name=%E8%AE%A1%E7%AE%97%E4%B8%8E%E7%BD%91%E7%BB%9C&level2_name=%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%20LB)。

## 四层监听器
| 协议    | 说明                    | 应用场景                                 |
| ------- | ------------------------ | ---------------------------------------- |
| TCP | 面向连接的、可靠的传输层协议<li style="width:430px">传输的源端和终端需先三次握手建立连接，再传输数据</li><li style="width:430px">支持基于客户端 IP（源 IP）的会话保持</li><li style="width:430px">在网络层可以看到客户端 IP</li><li  style="width:430px">服务端可直接获取客户端 IP</li>| 适用于对可靠性和数据准确性要求高、对传输速度要求较低的场景，如文件传输、收发邮件、远程登录等。<br>详情请参见 [配置 TCP 监听器](https://intl.cloud.tencent.com/document/product/214/32517)。|
| UDP | 无连接的传输层协议<li style="width:430px">传输的源端和终端不建立连接，不需维护连接状态</li><li style="width:430px">每一条 UDP 连接都只能是点到点的</li><li style="width:430px">支持一对一，一对多，多对一和多对多的交互通信</li><li style="width:430px">支持基于客户端 IP（源 IP）的会话保持</li><li style="width:430px">服务端可直接获取客户端 IP</li>|适用于对传输效率要求高、对准确性要求相对较低的场景，如即时通讯、在线视频等。<br>详情请参见 [配置 UDP 监听器](https://intl.cloud.tencent.com/document/product/214/32518)。|
| TCP SSL | 安全的 TCP <li style="width:430px">TCP SSL 监听器支持配置证书，阻止未经授权的访问</li><li style="width:430px">统一的证书管理服务，CLB 完成解密操作</li><li style="width:430px">支持单项认证和双向认证</li><li style="width:430px">服务端可直接获取客户端 IP</li>| 适用于 TCP 协议下对安全性要求非常高的场景，支持基于 TCP 的自定义协议。<br>详情请参见 [配置 TCP SSL 监听器](https://intl.cloud.tencent.com/document/product/214/32519)。|

如果您使用四层监听器（即使用四层协议转发），负载均衡实例会在监听端口上建立与后端实例的 TCP 连接，直接将请求转发到后端服务器，此过程中不修改任何数据包（透传模式），转发效率极高。

## 七层监听器
| 协议    | 说明                   | 应用场景                                 |
| ------- | --------------------- | ---------------------------------------- |
|HTTP|应用层协议<li style="width:424px">支持基于请求域名和 URL 的转发</li><li style="width:424px">支持基于 Cookie 的会话保持</li>|需要对请求的内容进行识别的应用，例如 Web 应用、App 服务等。<br>详情请参见 [配置 HTTP 监听器](https://intl.cloud.tencent.com/document/product/214/32515)。|
|HTTPS|加密的应用层协议<li style="width:424px">支持基于请求域名和 URL 的转发</li><li style="width:424px">支持基于 Cookie 的会话保持</li><li style="width:424px">统一的证书管理服务，CLB 完成解密操作</li><li style="width:424px">支持单项认证和双向认证</li>|需加密传输的 HTTP 应用。<br>详情请参见 [配置 HTTPS 监听器](https://intl.cloud.tencent.com/document/product/214/32516)。|

## 端口配置
<table>
<thead>
<tr>
<th width="25%">监听端口（前端端口）</th>
<th width="25%">服务端口（后端端口）</th>
<th width="50%">说明</th>
</tr>
</thead>
<tbody><tr>
<td>负载均衡提供服务时，接收请求并向后端服务器转发请求的端口。<br><br>用户可以为1 - 65535端口配置负载均衡，包括21（FTP）、25（SMTP）、80（HTTP）、443（HTTPS）等。</td>
<td>服务端口为云服务器提供服务的端口，接受并处理来自负载均衡的流量。<br><br>在一个负载均衡实例中，同一个负载均衡监听端口可以将流量转发到多个云服务器的多个端口上。</td>
<td>在同一个负载均衡实例内<li>监听端口不可重复。例如，不可以同时创建监听器 TCP:80 和监听器 HTTP:80。</li><li>仅 TCP 和 UDP 协议的端口可重复。例如，可以同时创建监听器 TCP:80 和监听器 UDP:80。</li><br>服务端口可以在同一个负载均衡实例内重复。例如，监听器 HTTP:80 和监听器 HTTPS:443 可以同时绑定同一台云服务器的同一个端口。</td>
</tr>
</tbody></table>
