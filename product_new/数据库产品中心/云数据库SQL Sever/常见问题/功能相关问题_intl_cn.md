
### 云数据库 SQL Server 产品架构是怎样的？
云数据库 SQL Server 由一主一镜像的 SQL Server 数据库组成，跨机架部署，每个库对应一组监控 Agent，通过心跳对数据库进行实时监控。独立部署的两组决策，调度集群和配置集群组成，作为集群的管理调度中心，主要管理数据库节点组、接入网关集群、HDFS 的正常运行。Hadoop 分布式文件系统（HDFS）提供数据灾备服务，提供冷备数据。接入网关集群，对外提供唯一的 IP，如果数据节点发送切换，IP 不会改变。

### 云数据库 SQL Server 如何创建实例并连接到数据库？
您可以通过 SQL Server 的管理控制台页面来完成管理数据库相关工作。

### 云数据库 SQL Server 实例帐号如何管理？
云数据库 SQL Server 禁止通过 Microsoft SQL Server Management 执行创建、删除数据库、创建、删除或修改账号操作，支持在 [SQL Server 控制台](https://console.cloud.tencent.com/sqlserver) > 实例详情页 > 【帐号管理】，创建、删除帐号或修改帐号权限。
目前仅支持用户使用“读写”、“只读”权限，如果需要更高级别的权限，请 [提交工单](https://console.cloud.tencent.com/workorder/category) 申请（不提供 sa 账号）。

### 云数据库 SQL Server 实例如何监控？
云数据库 SQL Server 支持 SQL Server 常见的25种参数，您可以通过配置 SSMS 的计数器，额外统计其他参数，详情参见 [监控告警](https://cloud.tencent.com/document/product/238/7524)。
目前支持通过云监控对以下监控指标进行告警，您可以在【[云监控](https://console.cloud.tencent.com/monitor/overview)】>【告警配置】>【告警策略】里面配置告警。
- CPU 利用率
- 连接数
- 输入流量
- 输出流量
- IOPS
- 存储空间

### 云数据库 SQL Server 实例如何回档？
云数据库 SQL Server 的全量备份和日志备份保存7天，因此可以回档到7天内的任意时刻。

### 云数据库 SQL Server 实例如何备份？
云数据库 SQL Server 控制台中可以通过下载的备份文件将数据库恢复到其他实例（如自建实例）上。


### 云数据库 SQL Server 如何迁移数据？
云数据库 SQL Server 支持 [CVM 自建 SQL Server 迁移数据](https://intl.cloud.tencent.com/document/product/238/31421) 和 [ 其他云厂商或自建 SQL Server 迁移](https://intl.cloud.tencent.com/document/product/238/32558)。

### 云数据库 SQL Server 升级流程是怎样的？
升级操作指将现有云数据库 SQL Server 实例的规格升级到更高规格。
1. 在 [SQL Server 控制台](https://console.cloud.tencent.com/sqlserver) 中选择选择相应的实例，在【操作】列，单击【升级】。
2. 在弹出的升级页，根据需要选择目标规格，并支付费用，成功后系统将自动升级实例规格。
升级费用 =（目标规格单价 - 原规格单价）x 剩余到期时间

>
>- 只能向更大规格实例进行升级，不支持降级。
>- 升级过程中，不能取消本次升级操作。
