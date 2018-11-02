# 配置HBase11xsql Writer {#concept_kfw_ryl_q2b .concept}

本文为您介绍HBase11xsql Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

HBase11xsql Writer实现了向Hbase中的SQL表（phoenix）批量导入数据的功能。Phoenix因为对rowkey做了数据编码，所以直接使用HBaseAPI进行写入会面临手工数据转换的问题，麻烦且易错。HBase11xsql Writer插件为您提供了单间的SQL表的数据导入方式。

在底层实现上，通过Phoenix的JDBC驱动，执行UPSERT语句向Hbase写入数据。

## 支持的功能 {#section_k4h_tyl_q2b .section}

支持带索引的表的数据导入，可以同步更新所有的索引表。

## 限制 {#section_tmw_tyl_q2b .section}

HBase11xsql Writer插件的限制如下所示。

-   仅支持1.x系列的Hbase。
-   仅支持通过phoenix创建的表，不支持原生HBase表。
-   不支持带时间戳的数据导入。

## 实现原理 {#section_n4h_tyl_q2b .section}

通过Phoenix的JDBC驱动，执行UPSERT语句向表中批量写入数据。因为使用上层接口，所以可以同步更新索引表。

## 参数说明 { .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|plugin|插件名字，必须是hbase11xsql。|是|无|
|table|要导入的表名，大小写敏感，通常phoenix表都是大写表名。|是|无|
|column|列名，大小写敏感。通常phoenix的列名都是大写。**说明：** 

-   列的顺序必须与Reader输出的列的顺序一一对应。
-   不需要填写数据类型，会自动从phoenix获取列的元数据。

|是|无|
|hbaseConfig|hbase集群地址，zk为必填项，格式为ip1, ip2, ip3。**说明：** 

-   多个IP之间使用英文的逗号分隔。
-   znode是可选的，默认值是/hbase。

|是|无|
|batchSize|批量写入的最大行数。|否|256|
|nullMode|读取到的列值为null时，您可以通过以下两种方式进行处理。-   skip：跳过这一列，即不插入这一列（如果该行的这一列之前已经存在，则会被删除）。
-   empty：插入空值，值类型的空值是0，varchar的空值是空字符串。

 |否|skip|

## 脚本开发介绍 {#section_nlj_scm_q2b .section}

脚本配置示例如下。

```
{
  "type": "job",
  "version": "1.0",
  "configuration": {
    "setting": {
      "errorLimit": {
        "record": "0"
      },
      "speed": {
        "mbps": "1",
        "concurrent": "1"
      }
    },
    "reader": {
      "plugin": "odps",
      "parameter": {
        "datasource": "",
        "table": "",
        "column": [],
        "partition": ""
      }
    },
    "plugin": "hbase11xsql",
    "parameter": {
      "table": "目标hbase表名，大小写有关",
      "hbaseConfig": {
        "hbase.zookeeper.quorum": "目标hbase集群的ZK服务器地址，向PE咨询",
        "zookeeper.znode.parent": "目标hbase集群的znode，向PE咨询"
      },
      "column": [
        "columnName"
      ],
      "batchSize": 256,
      "nullMode": "skip"
    }
  }
}
```

## 约束限制 {#section_iqp_xcm_q2b .section}

Writer中的列的定义顺序必须与Reader的列顺序匹配，Reader中的列顺序定义了输出的每一行中，列的组织顺序。而Writer的列顺序，定义的是在收到的数据中，Writer期待的列的顺序。示例如下：

Reader的列顺序为c1，c2，c3，c4。

Writer的列顺序为x1，x2，x3，x4。

则Reader输出的列c1就会赋值给Writer的列x1。如果Writer的列顺序是x1，x2，x4，x3，则c3会赋值给x4，c4会赋值给x3。

## 常见问题 {#section_jqp_xcm_q2b .section}

**Q：并发设置多少比较合适？速度慢时增加并发有用吗？**

A：数据导入进程默认JVM的堆大小是2GB，并发（channel数）是通过多线程实现的，开过多的线程有时并不能提高导入速度，反而可能因为过于频繁的GC导致性能下降。一般建议并发数（channel）为5-10。

**Q：batchSize设置多少比较合适？**

A：默认是256，但应根据每行的大小来计算最合适的batchSize。通常一次操作的数据量在2MB-4MB左右，用这个值除以行大小，即可得到batchSize。

