# 配置HDFS Writer {#concept_q3n_hdm_q2b .concept}

本文为您介绍HDFS Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

HDFS Writer提供向HDFS文件系统指定路径中写入TextFile文件、 ORCFile文件以及ParquetFile格式文件，文件内容可与 Hive 中表关联。在开始配置 HDFS Writer 插件前，请首先配置好数据源，详情请参见 [配置FTP数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置FTP数据源.md#)。

## 实现过程 {#section_dvy_43m_q2b .section}

HDFS Writer的实现过程，如下所示。

1.  根据您指定的path，创建一个HDFS文件系统上不存在的临时目录。

    创建规则：path\_随机。

2.  将读取的文件写入这个临时目录。
3.  全部写入后将这个临时目录下的文件移动到您指定的目录（在创建文件时保证文件名不重复）。
4.  删除临时目录。如果在此过程中，发生网络中断等情况造成无法与HDFS建立连接，需要您手动删除已经写入的文件和临时目录。

**说明：** 数据同步需要使用Admin账号，并且有访问相应文件的读写权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16224/15519393277725_zh-CN.png)

命令说明如下：

-   创建admin用户，并创建主目录，指定hadoop用户组，并指定supergroup附加组。

    ```
    useradd -m -G supergroup -g hadoop -p admin admin 
    ```

    -   `-G supergroup`：指定用户所属附加组。
    -   `-g hadoop`：指定用户所属的用户组。
    -   `-p admin admin`：给admin用户添加密码。
-   查看该目录下的文件内容。

    ```
    hadoop fs -ls /user/hive/warehouse/hive_p_partner_native
    ```

    使用hadoop命令时，格式为`hadoop fs -command`，其中command表示命令。

-   将文件part-00000拷贝到本地文件系统上。

    ```
    hadoop fs -get /user/hive/warehouse/hive_p_partner_native/part-00000
    ```

-   编辑刚刚拷贝的文件。

    ```
    vim part-00000
    ```

-   退出当前用户。

    ```
    exit
    ```

-   通过清单连接主机并在每台连接的主机上创建admin账号。

    ```
    pssh -h /home/hadoop/slave4pssh useradd -m -G supergroup -g hadoop -p admin admin
    ```

    -   `pssh -h /home/hadoop/slave4pssh`：通过清单文件连接主机。
    -   `useradd -m -G supergroup -g hadoop -p admin admin`：创建admin账号。

## 功能限制 {#section_fbh_pqj_p2b .section}

-   目前HDFS Writer仅支持TextFile、ORCFile和ParquetFile三种格式的文件，且文件内容存放的必须是一张逻辑意义上的二维表。
-   由于HDFS是文件系统，不存在schema的概念，因此不支持对部分列写入。
-   目前仅支持以下Hive数据类型。
    -   数值型：Tinyint、Smallint、Int、Bigint、Float和Double。
    -   字符串类型：String、Varchar和Char。
    -   布尔类型：Boolean。
    -   时间类型：Date、Timestamp。
-   目前不支持Decimal、Binary、Arrays、Maps、Structs和Union等Hive数据类型。
-   对于Hive分区表目前仅支持一次写入单个分区。
-   对于TextFile需您保证写入HDFS文件的分隔符与在Hive上创建表时的分隔符一致，从而实现写入HDFS数据与Hive表字段关联。
-   目前插件中的Hive版本为1.1.1，Hadoop版本为2.7.1（Apache为适配JDK1.7）。在Hadoop2.5.0、Hadoop2.6.0和Hive1.2.0测试环境中写入正常。

## 数据类型转换 {#section_acv_mlm_q2b .section}

目前HDFS Writer支持大部分Hive类型，请注意检查您的类型。

HDFS Writer针对Hive数据类型的转换列表，如下所示。

|数据集成类型分类|HDFS/Hive数据类型|
|:-------|:------------|
|long|Tinyint、Smallint、Int和Bigint。|
|double|Float和Double|
|string|String、Varchar和Char|
|boolean|Boolean|
|date|Date和Timestamp|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|defaultFS|Hadoop HDFS文件系统namenode节点地址，例如`hdfs://127.0.0.1:9000`。默认资源组不支持Hadoop高级参数HA的配置，请[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。|是|无|
|fileType|文件的类型，目前仅支持您配置为text，orc或parquet。-   text：表示TextFile文件格式。
-   orc：表示ORCFile文件格式。
-   parquet：表示普通parquet file文件格式。

|是|无|
|path| 存储到Hadoop HDFS文件系统的路径信息，HDFS Writer会根据并发配置在path目录下写入多个文件。

 为了与Hive表关联，请填写Hive表在HDFS上的存储路径。例如Hive上设置的数据仓库的存储路径为`/user/hive/warehouse/`，已建立数据库test表hello，则对应的存储路径为`/user/hive/warehouse/test.db/hello` 。

 |是|无|
|fileName|HDFS Writer写入时的文件名，实际执行时会在该文件名后添加随机的后缀作为每个线程写入实际文件名。|是|无|
|column|写入数据的字段，不支持对部分列写入。为了与Hive中的表关联，需要指定表中所有字段名和字段类型，其中name指定字段名，type指定字段类型。

您可以指定column字段信息，配置如下：

```
"column": 
[
    {
        "name": "userName",
        "type": "string"
    },
    {
        "name": "age",
        "type": "long"
    }
]
```

|是（如果filetype为parquet，此项无需填写）|无|
|writeMode|HDFS Writer写入前数据清理处理模式。-   append：写入前不做任何处理，数据集成HDFS Writer直接使用filename写入，并保证文件名不冲突。
-   nonConflict：如果目录下有fileName前缀的文件，直接报错。

**说明：** Parquet格式文件不支持Append，所以只能是noConflict。

|是|无|
|fieldDelimiter|HDFS Writer写入时的字段分隔符，需要您保证与创建的Hive表的字段分隔符一致，否则无法在Hive表中查到数据。|是（如果filetype为parquet，此项无需填写）|无|
|compress|HDFS文件压缩类型，默认不填写，则表示没有压缩。其中text类型文件支持gzip和bzip2压缩类型，orc类型文件支持SNAPPY压缩类型（需要您安装SnappyCodec）。

|否|无|
|encoding|写文件的编码配置。|否|无压缩|
|parquetSchema|写Parquet格式文件时的必填项，用来描述目标文件的结构，所以此项当且仅当fileType为parquet时生效 。格式如下：```
message MessageType名 {
是否必填, 数据类型, 列名;
......................;
}
```

配置项说明如下：

-   MessageType名：填写名称。
-   是否必填：required表示非空，optional表示可为空。推荐全填optional。
-   数据类型：Parquet文件支持Boolean、Int32、Int64、Int96、Float、Double、Binary（如果是字符串类型，请填Binary）和fixed\_len\_byte\_array等类型。

**说明：** 每行列设置必须以分号结尾，最后一行也要写上分号。

示例如下：

```
message m {
optional int64 id;
optional int64 date_id;
optional binary datetimestring;
optional int32 dspId;
optional int32 advertiserId;
optional int32 status;
optional int64 bidding_req_num;
optional int64 imp;
optional int64 click_num;
}
```

|否|无|
|hadoopConfig|hadoopConfig中可以配置与Hadoop相关的一些高级参数，例如HA的配置。默认资源组不支持Hadoop高级参数HA的配置，请[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。```
"hadoopConfig":{
"dfs.nameservices": "testDfs",
"dfs.ha.namenodes.testDfs": "namenode1,namenode2",
"dfs.namenode.rpc-address.youkuDfs.namenode1": "",
"dfs.namenode.rpc-address.youkuDfs.namenode2": "",
"dfs.client.failover.proxy.provider.testDfs": "org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider"
}
```

|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导开发模式开发。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

脚本配置示例如下，详情请参见上述参数说明。

```
{
    "type": "job",
    "version": "2.0",//版本号
    "steps": [
        { //下面是关于Reader的模板，可以找相应的读插件文档
            "stepType": "stream",
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "stepType": "hdfs",//插件名
            "parameter": {
                "path": "",//存储到 Hadoop HDFS 文件系统的路径信息
                "fileName": "",//HDFS Writer 写入时的文件名
                "compress": "",//HDFS 文件压缩类型
                "datasource": "",//数据源
                "column": [
                    {
                        "name": "col1",//字段名
                        "type": "string"//字段类型
                    },
                    {
                        "name": "col2",
                        "type": "int"
                    },
                    {
                        "name": "col3",
                        "type": "double"
                    },
                    {
                        "name": "col4",
                        "type": "boolean"
                    },
                    {
                        "name": "col5",
                        "type": "date"
                    }
                ],
                "writeMode": "",//写入模式
                "fieldDelimiter": ",",//列分隔符
                "encoding": "",//编码格式
                "fileType": "text"//文本类型
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": ""//错误记录数
        },
        "speed": {
            "concurrent": 3,//作业并发数
            "throttle": false,////false代表不限流，下面的限流的速度不生效，true代表限流
            "dmu": 1//DMU值
        }
    },
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    }
}
```

