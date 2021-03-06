## 配置场景

腾讯云 CDN 在进行缓存时使用的是 Key-Value 格式进行资源映射，其中的 Key 即缓存键，是缓存资源的唯一标识。

用户通过 URL 进行资源访问时，可能会携带一些具有特殊作用的参数，如使用以下链接来表示两张不同的图片：

```
http://cloud.tencent.com/1.jpg?version=1
http://cloud.tencent.com/1.jpg?version=2
```

这种场景需要关闭参数过滤，由完整的 URL 作为缓存键，分别进行图片内容的缓存，来进行资源区分。
在音视频场景下，若使用时间戳签名参数来进行访问认证：

```
http://cloud.tencent.com/1.mp4?sign=XXXXXX
```

这种场景需要开启参数过滤，由“?”之前的链接`http://cloud.tencent.com/1.mp4`作为缓存键，节点仅缓存一份资源，即使时间戳签名不断变化，通过签名校验后可直接命中缓存。

## 配置指南

### 查看配置

登录 [CDN 控制台](https://console.cloud.tencent.com/cdn)，在菜单栏里选择【域名管理】，单击域名右侧【管理】，即可进入域名配置页面，【缓存控制】中可看到过滤参数配置。

接入加速域名时：
- 若选择静态加速类型，默认关闭过滤参数。
- 下载、流媒体点播业务类型，默认开启过滤参数。

![](https://main.qcloudimg.com/raw/0c732e20d4fe5d4a8434cc8239f35c4d.png)

### 修改配置

开启参数缓存配置时，可以选择是否保留指定参数：
- 不保留：过滤全部参数。
![](https://main.qcloudimg.com/raw/270063cc7e622243b689f6a5e8074ac8.png)
- 保留指定参数：可指定要保留的参数，多个参数之间用“;”隔开，最多可输入6个参数。未指定的参数将会被过滤。
![](https://main.qcloudimg.com/raw/314351981551aa93e46bbeee48a0c67b.png)
提交后，可变更配置：可关闭过滤擦参数或编辑保留参数。
![](https://main.qcloudimg.com/raw/adf655a8545c3541848e0fa1126e3ae9.png)

### 配置示例

若加速域名 `http://www.test.com` 的 过滤参数配置如下：
![](https://main.qcloudimg.com/raw/96ab290f50fa9dbf364426cdb9f537fb.png)
当访问`http://www.test.com/1.jpg?version`时，会忽略URL请求中`?`之后的所有参数。


若加速域名 `http://www.test.com` 的 过滤参数配置如下：
![](https://main.qcloudimg.com/raw/9f7e0b1714f36c7c5c5e05a8bf26e4dd.png)
当访问`http://www.test.com/1.jpg?version`时，会精准匹配 URL 请求中的 version 参数而忽略其他参数。

> !
> - 若您的加速域名服务区域为全球加速，设置的参数缓存配置会全球生效，不支持境内、境外差异化配置。
> - 开启参数缓存配置，可有效提升缓存命中率。
