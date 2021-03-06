通过在 sql 语句前增加注释（hint）可以实现特定的功能，TDSQL 提供如下的 hint。

### 透传到指定物理分片
在分布式中，proxy 会对 sql 进行语法解析，会有比较严格的限制，如果用户想在某个物理分片（set）中执行 sql ，可以使用透传 sql 的功能。
透传 sql 的功能：支持透传 sql 到对应的一个或者多个物理分片（set），透传到分表键（shardkey）对应的 set ，示例如下：
```
	mysql> select * from test1 where a in (select a from test1);
	ERROR 7009 (HY000): Proxy ERROR:proxy do not support such use yet
	mysql> /*set_1*/select * from test1 where a in (select a from test1);
	Empty set (0.00 sec)
```
具体语法：
```
	/*sets:set_1*/
	/*sets:set_1,set_2*/  
	/*sets:allsets*/
	/*shardkey:10*/
```

### 强制 sql 走从机实现读写分离
TDSQL 支持多种读写分离方案，通过`/*slave*/`的方案常用于代码实现时，固化到业务逻辑中，方便开发中使用，示例如下：
```
	mysql> /*slave*/select * from test.ff limit 0;
```


>执行 TDSQL 特定的 sql 请参见 [控制指令](https://intl.cloud.tencent.com/document/product/1042/33371)。
