DBbrain 提供性能趋势功能，不仅支持多种性能指标的选择，包括关键指标、全部指标、自定义指标等，也支持性能趋势的多种查看方式，包括单性能指标趋势的细粒度查看，多性能指标趋势的联动对比查看，多性能指标趋势的时间对比查看等。

>?性能趋势目前支持云数据库 MySQL（不含基础版）、云数据库 CynosDB（CynosDB for MySQL）。

![](https://main.qcloudimg.com/raw/21d421be543c040140dd7a6a86226ef0.png)


## 查看性能趋势指标
性能趋势功能支持多种性能指标的选择，包含资源监控、MySQL Server、InnoDB 引擎、MySQL Replication 4大类别，及类别下15个子类指标，用户可以根据不同的运维场景需求选定不同的指标，进而查看所选实例在选定指标下的性能趋势表现。

1. 登录 [DBbrain 控制台](https://console.cloud.tencent.com/dbbrain/analysis)，在左侧导航选择【诊断优化】，在上方选择对应数据库，然后选择【性能趋势】页。
2. 在性能趋势页，单击“性能指标选择”的下拉框。
3. 在性能指标选择页，勾选性能指标，也可在右上角快捷选择【关键指标】、【全选】或【全不选】，选择指标后，单击【确定】。
>?单击【应用于全部实例】，可将所选指标应用于全部的数据库实例。
>
![](https://main.qcloudimg.com/raw/11d33834252e836eebc908ac9c18cb07.png)


性能趋势功能目前支持的性能指标如下：
<table>
<tr>
<td rowspan=7>资源监控</th>
<td>CPU</th>
<td>CPU</th>
</tr>
<tr>
<td rowspan=2 >内存</td>
<td>内存</td>
</tr>
<tr>
<td>内存占用</td>
</tr>
<tr>
<td rowspan=2>存储空间</td>
<td>磁盘利用率</td>
</tr>
<tr>
<td>磁盘占用空间</td>
</tr>
<tr>
<td rowspan=2>流量</td>
<td>输出流量</td>
</tr>
<tr>
<td>输入流量</td>
</tr>
<tr>
<td rowspan=13>MySQL Server</td>
<td>TPS/QPS</td>
<td>TPS/QPS</td>
</tr>
<tr>
<td rowspan=4>连接</td>
<td>最大连接数</td>
</tr>
<tr>
<td>Connected Threads</td>
</tr>
<tr>
<td>Running Threads</td>
</tr>
<tr>
<td>已创建的线程数</td>
</tr>
<tr>
<td rowspan=6>请求数</td>
<td>Select</td>
</tr>
<tr>
<td>Update</td>
</tr>
<tr>
<td>Delete</td>
</tr>
<tr>
<td>Insert</td>
</tr>
<tr>
<td>Replace</td>
</tr>
<tr>
<td>总请求数</td>
</tr>
<tr>
<td rowspan=2>慢查询</td>
<td>慢查询数</td>
</tr>
<tr>
<td>全表扫描数</td>
</tr>
<tr>
<td rowspan=14>InnoDB 引擎</td>
<td rowspan=4>InnoDB Buffer Pool Pages</td>
<td>InnoDB 空页数</td>
</tr>
<tr>
<td>InnoDB 总页数</td>
</tr>
<tr>
<td>InnoDB 逻辑读</td>
</tr>
<tr>
<td>InnoDB 物理读</td>
</tr>
<tr>
<td rowspan=2>InnoDB Data 读写量</td>
<td>InnoDB 读取量</td>
</tr>
<tr>
<td>InnoDB 写入量</td>
</tr>
<tr>
<td rowspan=2>InnoDB Data 读写次数</td>
<td>InnoDB 总读取量</td>
</tr>
<tr>
<td>InnoDB 总写入量</td>
</tr>
<tr>
<td rowspan=4>InnoDB Row  Operations</td>
<td>InnoDB 行删除量</td>
</tr>
<tr>
<td>InnoDB 行插入量</td>
</tr>
<tr>
<td>InnoDB 行更新量</td>
</tr>
<tr>
<td>InnoDB 行读取量</td>
</tr>
<tr>
<td rowspan=2>InnoDB Row Lock</td>
<td>InnoDB 等待行锁次数</td>
</tr>
<td>InnoDB 平均获取行锁时间</td>
</tr>
<tr>
<td rowspan=4>MySQL Replication</td>
<td rowspan=2>复制状态</td>
<td>主从延迟距离</td>
</tr>
<tr>
<td>主从延迟时间</td>
</tr>
<tr>
<td rowspan=2>复制延迟</td>
<td>IO 线程状态</td>
</tr>
<tr>
<td>SQL 线程状态</td>
</tr>
</tbody></table>


## 开启图表联动
1. 登录 [DBbrain 控制台](https://console.cloud.tencent.com/dbbrain/analysis)，在左侧导航选择【诊断优化】，在上方选择对应数据库，然后选择【性能趋势】页。
2. 在性能趋势页，打开右上角的“图表联动”开关，可以查看多性能指标趋势的联动对比。
![](https://main.qcloudimg.com/raw/8f0143e681567f928b115fae7ddf23ca.png)
拉动视图框，可以对单性能指标趋势进行更加清晰的细粒度查看。
![](https://main.qcloudimg.com/raw/54b877aeee9d5ae2495b4496489caa2d.png)

## 切换实时/历史视图

1. 登录 [DBbrain 控制台](https://console.cloud.tencent.com/dbbrain/analysis)，在左侧导航选择【诊断优化】，在上方选择对应数据库，然后选择【性能趋势】页。
2. 单击【实时】或【历史】，查看对应的实时性能趋势和历史性能趋势。
 - 实时性能趋势视图中，用户可以查看实例近三分钟的性能趋势状况，默认情况下为自动刷新，单击【停止刷新】，可停止实时刷新趋势状况。
![](https://main.qcloudimg.com/raw/81e13852a0356469df194eb685f2a12a.png)
 - 历史性能趋势视图中，选择不同的时间段，会显示所选时间段内的性能趋势视图，支持近1小时、近3小时、近24小时、近7天以及自定义时间的切换查看。
![](https://main.qcloudimg.com/raw/8883b09c4f7b2b28b60299ba61c6b5d7.png)
单击【添加时间对比】，选定所关注的对比时间段，可以查看多性能指标趋势的时间对比。
![](https://main.qcloudimg.com/raw/696f8ccd768423e1aefaed587e8da3c2.png)
