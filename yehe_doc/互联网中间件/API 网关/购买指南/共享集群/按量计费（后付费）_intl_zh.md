
## 计费模式
API 网关有多种计费方式，本文主要介绍按量付费方式，按量付费方式是默认计费方式。

## 计费项说明
API 网关的计费项包括：调用次数费用和流量费用。费用组成如下：
![](https://main.qcloudimg.com/raw/01a973e0c43322e5ec3adc6218a2e392.png)

> !
> - 如果您是内网 API 网关服务，内网产生的出流量和入流量均免费。
> - 如果您是公网 API 网关服务，公网入流量费用免费，公网出流量将会产生流量费用，具体流量费用请参照下文流量费用。

### 调用次数费用
| 按阶梯计费 | 月累计超过次数（N） | 单价（USD/万次） |
|---------|---------|---------|
| 免费阶梯 | 第一年每月（自然月）前100万次调用免费 | 0 |
| 第一阶梯 |  100 ＜ N ≤ 1000万 | 0.0089 |
| 第二阶段 | 1000万 ＜ N ≤ 1亿 | 0.0059 |
| 第三阶段 | N ＞ 1亿 | 0.0045 |

### 流量费用
| 地域                                      | 价格（单位：USD/GB） |
|-------------------------------------------|----------------------|
| 中国大陆 新加坡 首尔 法兰克福 莫斯科 东京 | 0.12                 |
| 中国香港                                  | 0.15                 |
| 多伦多 硅谷 弗吉尼亚 曼谷 孟买            | 0.074                |


#### 计费说明
-  计费项：API 调用量（次数）
-  计费方式：按量后付费
-  计费周期：小时
-  账单时间： 腾讯云每小时生成当前计费周期 API 调用费用记录，出账时间通常在当前计费周期结束后30分钟内。账单生成后会自动从账户余额中扣除费用以结算账单，若账户余额不足支付当期账单，则账户进入欠费状态
- 扣费方式： 账单生成后会自动从您的账户余额中扣除费用以结算账单
- 计费币种：美元
- 有效调用次数： API 网关收到的有效调用 API 请求，非 [前台错误](https://intl.cloud.tencent.com/document/product/628/31717) 认为是有效调用，会计入收费范围

**开通网关服务后，第一年每月（自然月）前一百万次调用免费，当月超过部分按照调用次数费用表阶梯计费。**

<span id="llfy"></span>

### 流量费用
流量费用根据流量的累计值进行计算。下面分别介绍流量的各计量项和计费说明：

| 计费项     | 计费项含义                                                   | 计费说明                                                     |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 内网出流量 | 数据通过腾讯云内网从 API 网关传输到后端服务产生的流量        | 免费                                                         |
| 内网入流量 | 数据通过腾讯云内网从客户端传输到 API 网关 产生的流量          | 免费                                                         |
| 外网出流量 | 数据通过互联网从客户端传输到 API 网关后，API 网关返回响应数据所产生的流量 | 按小时结算 每 GB 单价 * 小时累计出流量单价请参照 [公网计费模式（按流量计费）](https://buy.cloud.tencent.com/price/idc) |
| 外网入流量 | 数据通过互联网从客户端传输到 API 网关产生的流量              | 免费                                                         |


流量走向
![](https://main.qcloudimg.com/raw/8d816cd1a15d788a53a3eecb06eb94c4.png)
#### 计费说明
- 计费项：流量费用（外网出流量）
- 计费方式：按量计费 
- 计费周期：小时
- 账单时间： 账单出账时间通常在当前计费周期结束后一小时内。
- 扣费方式： 账单生成后会自动从您的账户余额中扣除费用以结算账单
- 计费币种：美元

**若您的后端服务与 API 网关不在同一区域，或后端服务不在腾讯云，我们将会额外收取 API 网关到您后端服务的流量费用。收费标准同 [流量费用](#llfy)。**

## 计费示例
以下示例反映了广州地域的定价
后端服务与 API 网关不在同一区域内，月累计共收到500万次 API 调用，每次 API 调用返回大小为3KB的响应，每次 API 网关转发数据到后端服务请求大小为1KB。
- API 网关调用费 = 500万 * 0.0089 USD/万次 = 4.45USD
- 数据传输总量 = 500万 * 3KB = 1500万/KB = 14.3GB
- 额外数据传输总量 = 500万 * 1KB= 500万/KB = 4.77GB
- 数据传输总费用 = 14.3GB * 0.12USD/GB + 4.77 GB * 0.12USD/GB = 2.29USD
- 总费用 = 4.45USD + 2.29USD = 6.74USD

## 补充说明
- 关于 API 网关 的欠费停服策略：数据的保留和销毁时间、以及相关计费说明，请参见 API 网关 [欠费说明](https://intl.cloud.tencent.com/document/product/628/11934)。
- 后端对接 WEBSOCKET 的 API 暂时不收取调用次数费用，仅收取外网出流量费用。

价格文档仅供参考，最终价格以账单为准
