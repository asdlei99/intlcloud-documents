

聚合函数是对一组值执行计算并返回单一的值的函数，它经常与 [SELECT](https://intl.cloud.tencent.com/document/product/614/38735) 语句的 [GROUP BY](https://intl.cloud.tencent.com/document/product/614/38732) 子句一同使用。

| 语句                 | 含义                                     | 示例                              | 示例 SQL                                                |
| -------------------- | ---------------------------------------- | --------------------------------- | ------------------------------------------------------- |
| AVG(KEY)             | 计算 KEY 列的算数平均值。                | 计算请求平均响应时间。              | `SELECT AVG(request_time) `                             |
| COUNT(\*)             | 表示所有的行数。                         | 计算状态码大于200的所有请求数 。    | `SELECT COUNT(*)  WHERE http_status  > 200`             |
| COUNT(KEY)           | 计算某一 KEY 列非 null 的行数。          | 计算请求时间大于5.0秒的请求的个数。 | `SELECT COUNT(request_time)  WHERE request_time  > 5.0` |
| COUNT(1)             | COUNT(1)等同于COUNT(\*)，表示所有的行数。 | -                                 | -                                                       |
| COUNT(DISTINCT(KEY)) | 统计 KEY 列去重后的行数。                | 计算独立客户端IP的个数（UV）。      | `SELECT COUNT(DISTINCT(remote_addr)) AS UV`             |
| MAX(KEY)             | 返回 KEY 列最大值。                      | 计算请求时间最大值。                | `SELECT MAX(request_time) AS max_request_time`          |
| MIN(KEY)             | 返回 KEY 列最小值。                      | 计算请求时间最大值。                | `SELECT MIN(request_time) AS   min_request_time`        |
| SUM(KEY)             | 返回 KEY 列的和。                        | 计算请求的总字节数。                | `SELECT SUM(body_bytes_sent) AS sum_bytes`              |
