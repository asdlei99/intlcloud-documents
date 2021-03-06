实例端口验通功能可以帮助您排查云服务器实例的安全组端口放通情况，定位故障所在，提升使用体验。
本功能可以验证的端口如下：
 <table>
<thead>
<tr>
<th>规则类型</th>
<th>端口</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td rowspan="7">入站规则</td>
<td>TCP:3389</td>
<td>用于放通 Windows 远程登录。</td>
</tr>
<tr>
<td>TCP:22</td>
<td>用于放通 Linux SSH 登录。</td>
</tr>
<tr>
<td>TCP:443</td>
<td>用于提供网站 HTTPS 服务。</td>
</tr>
<tr>
<td>TCP:80</td>
<td>用于提供网站 HTTP 服务。</td>
</tr>
<tr>
<td>TCP:21</td>
<td rowspan="2">用于放通 FTP 服务的上传、下载。</td>
</tr>
<tr>
<td>TCP:20</td>
</tr>
<tr>
<td>ICMP 协议</td>
<td>用于提供网站 HTTP 服务。ICMP 为控制协议，不涉及端口号。</td>
</tr>
<tr>
<td>出站规则</td>
<td>ALL</td>
<td>用于放通所有出站流量，以访问外部网络。</td>
</tr>
</tbody></table>

## 操作指南
1. 登录 [私有网络控制台](https://console.cloud.tencent.com/vpc)。
2. 单击左侧目录下方的【诊断工具】>【实例端口验通】，进入管理页面。
3. 在页面上方选择【地域】，并在列表中，找到您要验证的实例所在行，单击【一键检测】。
![](https://main.qcloudimg.com/raw/04780280964c1d63698423b22ce764f5.png)
4. 您可以在弹窗中看到端口验通的详情，如需放通所有未放通端口，单击【一键放通】即可。
![](https://main.qcloudimg.com/raw/b7f208cca3943ecefacd791f30533911.png)
如果您仅需要放通部分端口（例如 `TCP:22`），请在 [安全组控制台](https://console.cloud.tencent.com/vpc/securitygroup) 进入实例绑定的安全组，添加一条放通 `TCP:22` 端口的入站规则，您还可以根据实际需求，在【来源】中默认选择 all 放通全部 IP，或填写指定 IP（IP 段），如下图所示。
![](https://main.qcloudimg.com/raw/6780b4df05168e19718ffe6c699c5159.png)

## 相关信息
- 如需了解安全组相关内容，请参见 [安全组概述](https://intl.cloud.tencent.com/document/product/215/38750) 、[添加安全组规则](https://intl.cloud.tencent.com/document/product/215/35513)。
- 如需了解服务器常用端口相关说明，请参见 [服务器常用端口](https://intl.cloud.tencent.com/document/product/215/35520)。
