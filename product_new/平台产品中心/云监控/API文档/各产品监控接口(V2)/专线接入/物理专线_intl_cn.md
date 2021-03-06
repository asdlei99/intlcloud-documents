## 接口描述
域名：`monitor.api.qcloud.com`
接口：`GetMonitorData`
物理专线指连接腾讯云与本地数据中心的物理线路连接。
查询物理专线监控数据，入参取值如下：
namespace: qce/dc
dimensions.0.name=directConnectId
dimensions.0.value 为专线 ID

## 请求参数

以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见 <a href="/doc/api/405/公共请求参数" title="公共请求参数">公共请求参数</a> 页面。其中，此接口的 Action 字段为 GetMonitorData。

| 参数名称               | 必选   | 类型       | 输入内容            | 描述                                       |
| ------------------ | ---- | -------- | --------------- | ---------------------------------------- |
| namespace          | 是    | String   | qce/dc          | 命名空间，每个云产品会有一个命名空间，详情请参见输入内容一栏        |
| metricName         | 是    | String   | 具体的指标名称         | 指标名称，详情请参见 [指标名称说明](#zhibiao)  |
| dimensions.0.name  | 是    | String   | directConnectId | 入参为专线 ID                                  |
| dimensions.0.value | 是    | String   | 专线的具体 ID         | 调用 [DescribeInstances](https://intl.cloud.tencent.com/document/product/213/15728) 接口获取的 unInstanceId 字段 |
| period             | 否    | Int      | 60 / 300          | 监控统计周期，支持 60 s 统计粒度。                        |
| startTime          | 否    | Datetime | 起始时间            | 起始时间，如：2016-01-01 10:25:00</br> 默认时间为当天的 “00:00:00” |
| endTime            | 否    | Datetime | 结束时间            | 结束时间，默认为当前时间</br>endTime 不能小于 startTime       |

<span id="zhibiao"></span>
**指标名称说明**

| 指标名称（metricName） | 含义   | 单位   | 统计粒度（period） |
| ---------------- | ---- | ---- | ------------ |
| inbandwidth      | 入带宽  | bps  | 60 s          |
| outbandwidth     | 出带宽  | bps  | 60 s          |


## 响应参数

| 参数       | 类型       | 描述                  |
| ---------- | -------- | ------------------- |
| code       | Int      | 错误码</br>0：成功</br>其他值：失败 |
| message    | String   | 返回信息                |
| startTime  | Datetime | 起始时间                |
| endTime    | Datetime | 结束时间                |
| metricName | String   | 指标名称                |
| period     | Int      | 监控统计周期              |
| dataPoints | Array    | 监控数据列表              |


## 错误码

| 错误代码 | 错误描述    | 英文描述                                 |
| ---- | ------- | ------------------------------------ |
| -502 | 资源不存在   | OperationDenied.SourceNotExists      |
| -503 | 请求参数有误  | InvalidParameter                     |
| -505 | 参数缺失    | InvalidParameter.MissingParameter    |
| -507 | 超出限制    | OperationDenied.ExceedLimit          |
| -509 | 错误的维度组合 | InvalidParameter.DimensionGroupError |
| -513 | DB 操作失败  | InternalError.DBoperationFail        |

## 代码示例

### 请求示例

<pre>
https://monitor.api.qcloud.com/v2/index.php?
&<a href="/doc/api/405/公共请求参数" title="公共请求参数">公共请求参数</a>
&namespace=qce/dc
&metricName=inbandwidth
&dimensions.0.name=directConnectId
&dimensions.0.value=dc-e6p0gw5f
&startTime=2016-06-28 14:10:00
&endTime=2016-06-28 14:20:00
</pre>

### 响应示例

```shell
{
	"code": 0,
	"message": "",
	"metricName": "inbandwidth",
	"startTime": "2016-06-28 14:10:00",
	"endTime": "2016-06-28 14:20:00",
	"period": 300,
	"dataPoints": [
		15.6,
		17.9,
		20.3
	]
}
```
