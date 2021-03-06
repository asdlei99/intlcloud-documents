用户可以利用 DBbrain 的实时会话功能查看当前实例的实时会话信息，包含性能监控、连接数监控、当前活动线程、SQL 限流、热点更新保护。本文将介绍实时会话功能的使用。

>?实时会话目前支持云数据库 MySQL（不含基础版）、云数据库 CynosDB（CynosDB for MySQL）。其中 SQL 限流和热点更新保护仅支持云数据库MySQL（不含基础版）。
## 性能和连接数监控
登录 [DBbrain 控制台](https://console.cloud.tencent.com/dbbrain/session)，在左侧导航选择【诊断优化】，在上方选择对应数据库，然后选择【实时会话】页。
- “性能监控”栏：显示实例实时的 Running Threads 数、CPU 使用率。
- “连接数监控”栏：显示实例实时的最大连接数、当前打开连接数。
![](https://main.qcloudimg.com/raw/03c23c17ff637f50f40a8b4d8e8e8dce.png)

## 当前活动线程
数据库实例的 SHOW PROCESSLIST 可视化展示，默认5秒刷新，用户可根据不同场景的使用需求，设置不同维度的筛选：
- 线程种类可选择 All、Query、Not Sleep。
- 条数可选择限制20条、50条、100条。
- 刷新间隔时间可选择5秒、15秒、30秒。
- 可选择停止刷新或开启自动刷新。
![](https://main.qcloudimg.com/raw/baf2ebab28d566ad37730b489379a309.png)

## 结束（Kill）会话
DBbrain 提供在线结束（Kill）会话的功能，方便用户对会话进行管理，可选中所需会话，并单击【Kill 会话】完成操作。
- 目前支持对“单个会话”或“多个会话”进行 Kill 会话操作，单次批量操作上限暂时为100。
- 执行 Kill 会话时，须先选中所需要结束（Kill）的对应会话行。 

## SQL 限流
>?SQL 限流仅支持云数据库MySQL（不含基础版）。
>
DBbrain 提供 SQL 限流功能，您可以通过创建 SQL 限流任务，自主设置 SQL 类型、最大并发数、限流时间、SQL 关键词，来控制数据库的请求访问量和 SQL 并发量，进而达到服务的可用性，不同的任务之间不会发生冲突。
>?创建 SQL 限流任务之前需要先登录数据库帐号。
>
- SQL 类型：包含 select、update、delete、insert、replace。
- 最大并发数：为 SQL 最大并发数，当包含关键词的 SQL 达到最大并发数时会触发限流策略。如果该值设为0，则表示限制所有匹配的 SQL 执行。
- 执行方式：支持“定时关闭”和“手动关闭”。
- 限流时间：选择“定时关闭”时，需选择 SQL 限流的生效时间。
- SQL 关键词：为需要限流的 SQL 关键词，当包含多个关键词时，需要以英文逗号分隔，逗号分隔的条件是逻辑与的关系，且逗号不能作为关键词。

SQL 限流列表中，包含 SQL 类型、状态、关键词、开始时间、剩余时间、最大并发数以及操作。
- 单击“操作”列的【详情】，可以查看 SQL 限流详情。
- 限流任务开启后，若还在所设置的限流时间以内，列表中的状态为“运行中”，单击“操作”列的【关闭】，可以提前关闭限流任务，状态列将变为“已终止”。
- 限流任务开启后，若自动达到所设定的限流时间，列表中的状态将变为“已终止”。
- 单击“操作”列的【删除】，可以对状态为“已终止”和“已完成”的限流任务进行删除。


## 热点更新保护
>?热点更新保护仅支持云数据库MySQL（不含基础版）。
>
DBbrain 提供热点更新保护功能，针对语句的排队机制，尽可能把具有相同冲突的语句放在内存队列排队，通过开启热点更新保护减少锁冲突的开销，提高高并发场景的数据库性能。

单击【创建任务】，可以创建热点更新保护任务，您可以自主设置等待超时阈值、执行方式，其中执行方式包括定时关闭和手动关闭，在定时关闭的执行方式下，您可以自主设置执行时间。

热点更新保护列表中，包含状态、开始时间、执行时间、剩余时间、等待超时阈值以及操作。
- 当任务的状态处于“运行中”时，单击“操作”列的【关闭】，可以提前终止任务的执行
- 当任务的状态处于“已终止”或“已完成”时，单击“操作”列的【删除】，可以删除热点更新保护任务。
