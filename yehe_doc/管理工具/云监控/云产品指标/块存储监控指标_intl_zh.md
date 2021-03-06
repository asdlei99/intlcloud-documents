## 命名空间

Namespace=QCE/BLOCK_STORAGE

## 监控指标

| 指标英文名       | 指标中文名       | 指标含义                                                     | 单位 | 维度                |
| ---------------- | ---------------- | ------------------------------------------------------------ | ---- | ------------------- |
| DiskReadIops     | 硬盘读 IOPS      | 每秒从块存储读到内存中的 IO 次数                             | 次数 | diskId  |
| DiskReadTraffic  | 硬盘读流量       | 数据从块存储读取到内存中的速率                               | KB/s | diskId  |
| DiskWriteIops    | 硬盘写 IOPS      | 每秒从内存写到块存储中的 IO 次数                             | 次数 | diskId  |
| DiskWriteTraffic | 硬盘写流量       | 数据从内存写入到块存储中的速率                               | KB/s | diskId |
| DiskAwait        | 硬盘 IO 等待时间 | 在采样周期内有百分之几的时间 CPU 空闲并且有仍未完成的 I/O 请求 | ms   | diskId |
| DiskSvctm        | 硬盘 IO 服务时间 | IO 服务时间                                                  | ms   | diskId  |
| DiskUtil         | 硬盘 IO 繁忙比率 | 硬盘有 IO 操作的时间（即非空间时间）的比率                   | %    | diskId  |
| DiskUsage        | 磁盘分区使用率   | 磁盘分区已使用容量和总容量的百分比                           | %    |InstanceId |

> ?每个指标的统计粒度（Period）可取值不一定相同，可通过 [DescribeBaseMetrics](https://intl.cloud.tencent.com/document/product/248/33882) 接口获取每个指标支持的统计粒度

## 各维度对应参数总览

| 参数名称                       | 维度名称   | 维度解释                  | 格式                                |
| ------------------------------ | ---------- | ------------------------- | ----------------------------------- |
| Instances.N.Dimensions.0.Name  | diskId     | 块存储实例 ID 的维度名称    | 输入 String 类型维度名称：diskId     |
| Instances.N.Dimensions.0.Value | diskId     | 块存储实例的具体 ID       | 输入实例具体ID，例如：disk-test    |
| Instances.N.Dimensions.0.Name  | InstanceId | 磁盘分区实例 ID 的维度名称 | 输入 String 类型维度名称：InstanceId |
| Instances.N.Dimensions.0.Value | InstanceId | 磁盘分区实例的具体 ID     | 输入实例具体ID，例如：ins-mm8bs222 |

## 入参说明

块存储支持以下两种维度组合的查询方式，两种入参取值说明如下：

#### 1. 查询块存储监控数据，入参取值如下：

&Namespace=QCE/BLOCK_STORAGE
&Instances.N.Dimensions.0.Name=diskId
&Instances.N.Dimensions.0.Value=块存储 ID

#### 2. 查询服务器磁盘分区使用率 ，入参取值如下： 
&Namespace=QCE/BLOCK_STORAGE
&Instances.N.Dimensions.0.Name=InstanceId 
&Instances.N.Dimensions.0.Value=磁盘分区实例的具体 ID
