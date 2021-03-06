## 简介

>?节点池功能目前为内测发布，如需使用请 [提交工单](https://console.cloud.tencent.com/workorder/category) 进行申请。
>

为帮助您更好地管理 Kubernetes 集群内节点，腾讯云容器服务 TKE 引入节点池概念。借助节点池基本功能，您可以方便快捷地创建、管理和销毁节点，以及实现节点的动态扩缩容：
- 当集群中出现因资源不足而无法调度的实例（Pod）时，自动触发扩容，为您减少人力成本。
- 当满足节点空闲等缩容条件时，自动触发缩容，为您节约资源成本。

节点池整体架构图如下所示：
![](https://main.qcloudimg.com/raw/ee033cab974b375a1e59fb5a96aaa503.png)

通常情况下，节点池内的节点均具有如下相同属性：
- 节点操作系统。
- 计费类型（目前支持按量计费和竞价实例）。
- CPU/内存/GPU。
- 节点 Kubernetes 组件启动参数。
- 节点自定义启动脚本。
- 节点 Kubernetes Lable 和 taint 设置。

此外，TKE 将同时围绕节点池扩展以下属性：
- 节点池级别操作系统。
- 节点池级别每节点的 Pod 数上限。
- 包年包月计费类型节点池。
- 节点池级别自动修复与自动升级。

## 应用场景
当业务需要使用大规模集群时，推荐您使用节点池进行节点管理，以提高大规模集群易用性。下表介绍了多种大规模集群管理场景，并分别展示节点池在每种场景下发挥的作用：

| 场景 | 作用 |
| ---- | ---- |
| 集群存在较多异构节点（机型配置不同）| 通过节点池可规范节点分组管理。 |
| 集群需要频繁扩缩容节点  | 通过节点池可降低操作成本。 |
| 集群内应用程序调度规则复杂 | 通过节点池标签可快速指定业务调度规则。 |
| 集群内节点日常维护 | 通过节点池可便捷操作 Kubernetes 版本升级、Docker 版本升级。 |

## 相关概念
TKE 的弹性伸缩实现是基于腾讯云弹性伸缩（AS）以及 Kubernetes 社区的 [cluster-autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) 实现的。相关概念介绍：
- CA：[cluster-autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)，社区开源组件。
- AS：autoscaler，腾讯云弹性伸缩服务。
- ASG：as group，具体某个伸缩组。
- ASA：as activity，某次伸缩活动。
- ASC：as config，AS 启动配置。

## 节点池内节点种类
为了满足不同场景下的需求，节点池内的节点可以分为两个类型。

| 节点类型 |节点来源| 是否支持弹性伸缩 | 从节点池移除方式| 节点数目是否受【调整数量】影响|
|---------|---------|---------|---|---|
| 伸缩组内节点 | 弹性扩容或手动调整数量 | 是 | 弹性缩容或手动调整数量 | 是|
| 伸缩组外节点 |用户手动加入节点池 | 否 | 用户手动移除|否|

## 节点池弹性伸缩原理
节点池级别的弹性伸缩包含 K8s 提供的 cluster autoscaler（CA）功能及腾讯云弹性伸缩服务提供的 AS（AutoScaling）能力两个层级的扩缩容策略。
> ?
>- 在存在多个节点池的情况下，CA 会为您选择一个合适的节点池进行扩缩容，并且判断具体扩缩容的数目。
>- CA 会触发 AS，AS 负责执行具体节点池的扩缩容行为。
> 

- **节点池弹性扩容**
CA 会 watch 集群内的 pod、node 资源，当集群内出现有 pending 的 pod 时，会触发调用 cloud-provider 即 AS（伸缩组）的接口，调整相应伸缩组期望数量，触发扩容操作。
- **节点池弹性缩容**
当集群内的节点利用率计算出来的值小于用户给定的阈值时，会触发调用 cloud-provider 即 AS 的缩容接口，进行指定节点进行缩容。



## 功能点及注意事项

<table>
<thead>
<tr>
<th width="14%">功能点</th>
<th width="45%">功能说明</th>
<th width="41%">注意事项</th>
</tr>
</thead>
<tbody><tr>
<td>创建节点池</td>
<td>新增节点池， 暂不支持包年包月类型。</td>
<td>单个集群不建议超过20个节点池。</td>
</tr>
<tr>
<td>删除节点池</td>
<td><ul class="params"><li>删除节点池时可选择是否销毁节点池内节点。</li><li>保留的节点不归属任何伸缩组和节点池。</li></ul></td>
<td>删除节点池时选择销毁节点，节点将不会保留，后续如需使用新节点可重新创建。</td>
</tr>
<tr>
<td>节点池开启弹性伸缩</td>
<td>开启弹性伸缩后，节点池内节点数量将随集群负载情况自动调整。</td>
<td rowspan=2>请勿在伸缩组控制台开启和关闭弹性伸缩。</td>
</tr>
<tr>
<td>节点池关闭弹性伸缩</td>
<td>关闭弹性伸缩后，节点池内节点数量不随集群负载情况自动调整。</td>
</tr>
<tr>
<td>调整节点池大小</td>
<td>支持直接调整节点池内节点数量。若减小节点数量，将从伸缩组内随机缩容节点。</td>
<td>开启弹性伸缩后，不建议手动调整节点池大小。</td>
</tr>
<tr>
<td>调整节点池配置</td>
<td>可修改节点池名称、伸缩组节点数量范围、kubernetes label 及 taint。</td>
<td>修改 label 和 taint 会对节点池内节点全部生效，可能会引起 Pod 重新调度，请谨慎变更。</td>
</tr>
<tr>
<td>添加已有节点</td>
<td>可添加不属于集群的实例到节点池。要求如下：<ul class="params"><li>实例与集群属于同一私有网络。</li><li>实例未被其他集群使用且实例与节点池配置相同（机型、计费模式）。</li></ul>可添加集群内 node-without-pool 的节点，要求节点实例与节点池配置（相同机型、计费模式）。</td>
<td>无特殊情况时，不建议添加已有节点，推荐直接新建节点池。</td>
</tr>
<tr>
<td>移出节点池内节点</td>
<td>不支持直接移出伸缩组内节点，可移除手动添加的节点，移除后节点可选择保留到集群。</td>
<td>建议通过调整节点池大小的方式加入或移出节点。</td>
</tr>
<tr>
<td>原伸缩组转换节点池</td>
<td><ul class="params"><li>支持存量伸缩组切换为节点池。转化后，节点池完全继承原伸缩组的功能，该伸缩组信息将不再展示。</li><li>集群内存量所有伸缩组切换完成后，不再提供伸缩组入口。</li></td>
<td>操作不可逆，请熟悉节点池功能后再进行切换。</td>
</tr>
</tbody></table>


## 相关操作
您可以登录 [容器服务控制台](https://console.cloud.tencent.com/tke2) 并参考以下文档， 进行对应节点池操作：
- [创建节点池](https://intl.cloud.tencent.com/document/product/457/35901)
- [查看节点池](https://intl.cloud.tencent.com/document/product/457/35902)
- [调整节点池](https://intl.cloud.tencent.com/document/product/457/35903)
- [删除节点池](https://intl.cloud.tencent.com/document/product/457/35904)


<style>
	.params{margin:0px !important}
</style>
