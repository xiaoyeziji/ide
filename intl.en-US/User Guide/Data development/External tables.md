# External tables {#concept_uyr_htl_gfb .concept}

## External table overview {#section_ynq_n5l_gfb .section}

Before you use external tables, you need to understand the following concepts.

|Name|Description|
|Object Storage Service（OSS）|OSS supports Standard, Infrequent Access, and Archive storage types. It is applicable to service scenarios that involve different requirements for data storage and access. Additionally, OSS supports seamless integration with Apache Hadoop, E-MapReduce, BatchCompute, MaxCompute, Machine Learning Platform for AI \(PAI\), Data Lake Analytics, Function Compute, and other Alibaba Cloud services.|
|MaxCompute|The big data computing service is a fast and fully-managed data warehousing solution. When used in conjunction with OSS, it enables you to effectively analyze and process large-scale data with reduced costs. Forrester names MaxCompute as one of the world's leading cloud-based data warehouses because of its processing performance.|
|External tables of MaxCompute|This function is based on the new generation of the computing framework of MaxCompute v2.0. It allows you to directly query data that is stored in OSS without loading data into the internal tables of MaxCompute. This not only saves time and effort for data migration but also saves costs for storage of duplicate data. You can use the external tables of MaxCompute to query data that is stored in Table Store in a similar way.|

The following figure shows the processing architecture of the external tables.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21811/155168397312702_en-US.png)

Currently, MaxCompute supports processing external tables in the storage of unstructured data such as OSS and Table Store. Based on the flow of data and the processing rules, you can understand that the main function of the unstructured data processing framework is to import and export data and connect the input and output of MaxCompute. The following example describes the processing rules applied to external tables in OSS.

1.  Data stored in OSS is converted through the unstructured data processing framework and passed to user-defined interfaces using the InputStream Java class. To implement the extracting rules, you need to read, parse, convert, and calculate the input streams. The data must be returned in the record format, which is the general format in MaxCompute.
2.  These records can be used in structured data processing based on the SQL engine built into MaxCompute to generate new records.
3.  You can perform further calculations before the data of records are output through the OutputStream Java class and are imported into OSS by MaxCompute.

You can create, search, query, configure, process, and analyze external tables in GUI through DataWorks, which is powered by MaxCompute.

## Network and access authorization {#section_jgw_5yl_gfb .section}

Since MaxCompute is separate from OSS, network connectivity between them on different clusters may affect the ability of MaxCompute to access the data stored in OSS. We recommend that you use the private endpoint \(it ends with -internal.aliyuncs.com\) to access the data stored in OSS through MaxCompute.

Authorization is required for MaxCompute to access data stored in OSS. MaxCompute guarantees secure access to data using Resource Access Management \(RAM\) and Security Token Service \(STS\) provided by Alibaba Cloud. You request the STS token for MaxCompute as the table creator. Therefore, MaxCompute and OSS must be under the same Alibaba Cloud account. A similar authorization process applies when accessing data stored in Table Store.

1.  STS authorization

    If MaxCompute requires direct access to data stored in OSS, you need to grant the OSS access to RAM users first. Security Token Service \(STS\) is a security token management service provided by Alibaba Cloud. It is a product based on Resource Access Management \(RAM\). Authorized RAM users can issue tokens with custom validity and access through STS. Applications can use tokens to directly call Alibaba Cloud APIs to manipulate resources. For more information, see [OSS STS mode authorization](../../../../../intl.en-US/User Guide/External table/OSS STS mode authorization.md#). You can choose either of the following methods to grant access.

    -   If MaxCompute and OSS are under the same Alibaba Cloud account, log on and perform Authorize. You can click Data Development and Create Table to jump to the Authorize page as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21811/155168397312719_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21811/155168397312720_en-US.png)

    -   Custom authorization. First, you need to grant MaxCompute access to OSS through RAM. Log on to the RAM console \(if MaxCompute and OSS are under different Alibaba Cloud accounts, use the account for OSS to log on\). Go to the **Role Management** page and click **Create Role**. Set the value of Role Name to AliyunODPSDefaultRole or AliyunODPSRoleForOtherUser.

        Configure **Role Details**.

        ```
        --When MaxCompute and OSS are under the same Alibaba Cloud account.
        {
        "Statement": [
        {
        "Action": "sts:AssumeRole",
        "Effect": "Allow",
        "Principal": {
        "Service": [
        "odps.aliyuncs.com"
              ]
            }
          }
        ],
        "Version": "1"
        }
        --When MaxCompute and OSS are under different Alibaba Cloud accounts.
        {
        "Statement": [
        {
        "Action": "sts:AssumeRole",
        "Effect": "Allow",
        "Principal": {
        "Service": [
        "Alibaba Cloud account for MaxCompute@odps.aliyuncs.com"
              ]
            }
          }
        ],
        "Version": "1"
        }
        
        ```

        Configure **Role Authorization Policies**. Search for the AliyunODPSRolePolicy policy that is required for granting OSS access. Attach the AliyunODPSRolePolicy policy to the role. If you can not find this policy through **Search and Attach**, authorize the role through **Input and Attach**. The policy content of the AliyunODPSRolePolicy policy is shown as follows.

        ```
        
        {
          "Version": "1",
          "Statement": [
            {
              "Action": [
                "oss:ListBuckets",
                "oss:GetObject",
                "oss:ListObjects",
                "oss:PutObject",
                "oss:DeleteObject",
                "oss:AbortMultipartUpload",
                "oss:ListParts"
                ],
                "Resource": "*",
                "Effect": "Allow"
          },
          {
              "Action": [
                "ots:ListTable",
                "ots:DescribeTable",
                "ots:GetRow",
                "ots:PutRow",
                "ots:UpdateRow",
                "ots:DeleteRow",
                "ots:GetRange",
                "ots:BatchGetRow",
                "ots:BatchWriteRow",
                "ots:ComputeSplitPointsBySize"
              ],
              "Resource": "*",
              "Effect": "Allow"
            }
          ]
        }
        ```

2.  Using OSS data sources in Data Integration

    You can directly use the OSS data sources that have already been created in Data Integration.


## Create external tables {#section_rrw_t4m_gfb .section}

1.  Use DDL statements to create tables

    Go to the Data Development page. See [Table Management](intl.en-US/User Guide/Data development/Table Management.md#) and use DDL statements to create tables. You need to follow the ODPS syntax \(See [Table Operations](../../../../../intl.en-US/User Guide/SQL/DDL SQL/Table Operations.md#)\). If you have STS authorization, then you do not need to include the odps.properties.rolearn attribute. The following example shows how to use DDL statements to create a table. The EXTERNAL keyword in the statement indicates that this table is an external table.

    ```
    
    CREATE EXTERNAL TABLE IF NOT EXISTS ambulance_data_csv_external(
    vehicleId int,
    recordId int,
    patientId int,
    calls int,
    locationLatitute double,
    locationLongtitue double,
    recordTime string,
    direction string
    )
    
    STORED BY 'com.aliyun.odps.udf.example.text.TextStorageHandler' --The STORED BY clause specifies the StorageHandler for the corresponding file format. This clause is required.
    with SERDEPROPERTIES (
    'delimiter'='\\|', --The SERDEPROPERITES clause specifies the parameters used when serializing or deserializing data. These parameters are passed into the code of Extractor through DataAttributes. This clause is optional.
    'odps.properties.rolearn'='acs:ram::xxxxxxxxxxxxx:role/aliyunodpsdefaultrole'
    )
    LOCATION 'oss://oss-cn-shanghai-internal.aliyuncs.com/oss-odps-test/Demo/SampleData/CustomTxt/AmbulanceData/'             --The LOCATION clause specifies the location of the external tables. This clause is optional.
    USING 'odps-udf-example.jar'; --The USING clause specifies the Jar files that store the user-defined classes. This clause is optional, depending on whether you use user-defined classes. 
    ```

    The parameters following STORED BY that are corresponding to the built-in storage handlers for csv or tsv files are shown as follows:

    -   The `com.aliyun.odps.CsvStorageHandler` parameter is for CSV format. It defines how to read and write data in CSV format. The format has columns separated by the comma \(,\) and rows terminated by the newline character \(\\n\). For example, `STORED BY'com.aliyun.odps.CsvStorageHandler'` is a sample parameter.
    -   The `com.aliyun.odps.TsvStorageHandler` parameter is for TSV format. It defines how to read and write data in TSV format. The format has columns separated by the tab character \(\\t\) and rows terminated by the newline character \(\\n\).
    The parameters following STORED BY also support specifying the storage handlers for the **open-source file formats** such as TextFile, SequenceFile, RCFile, AVRO, ORC, and Parquet. For TextFile formats, you can specify the SerDe class. For example, `org.apache.hive.hcatalog.data.JsonSerDe`.

    -   org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe -\> stored as textfile
    -   org.apache.hadoop.hive.ql.io.orc.OrcSerde -\> stored as orc
    -   org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe -\> stored as parquet
    -   org.apache.hadoop.hive.serde2.avro.AvroSerDe -\> stored as avro
    -   org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe -\> stored as sequencefile
    For external tables that are in the open-source formats, the statements to create tables are as follows.

    ```
    
    CREATE EXTERNAL TABLE [IF NOT EXISTS] (<column schemas>)
    [PARTITIONED BY (partition column schemas)]
    [ROW FORMAT SERDE '']
    STORED AS 
    [WITH SERDEPROPERTIES ( 'odps.properties.rolearn'='${roleran}'
    [,'name2'='value2',...]
    ) ]
    LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
    ```

    Attributes of the **SERDEPROPERTIES** clause are shown in the following table. Currently, for gzip-compressed data from CSV and TST files in OSS, MaxCompute only supports reading through the built-in extractor. You can choose whether the file is gzip-compressed. Attribute settings are different based on file formats.

    |**Attribute**|**Value**|**Default value**|**Description**|
    |odps.text.option.gzip.input.enabled|true/false|false|Enables or disables the reading of compressed data.|
    |odps.text.option.gzip.output.enabled|true/false|false|Enables or disables the writing of compressed data.|
    |odps.text.option.header.lines.count|N \(a non-negative integer\)|0|Skip the first N lines of the file.|
    |odps.text.option.null.indicator|String|""|Replaces NULL with the value of the string.|
    |odps.text.option.ignore.empty.lines|true/false|true|Specifies whether to ignore blank lines.|
    |odps.text.option.encoding|UTF-8/UTF-16/US-ASCII|UTF-8|Specifies the encoding set of the file.|

    The LOCATION clause specifies the storage address of the external table in the format of oss://oss-cn-shanghai-internal.aliyuncs.com/BucketName/DirectoryName. You can select the directory in OSS through the dialog boxes. Do not select the files.

    You can find tables that are created using DDL statements in the node directories in the Tables tab. You can modify Level 1 Topic or Level 2 Topic to change the directories for the tables.

2.  External tables in Table Store

    The statements to create external tables in Table Store are as follows.

    ```
    
    CREATE EXTERNAL TABLE IF NOT EXISTS ots_table_external(
    odps_orderkey bigint,
    odps_orderdate string,
    odps_custkey bigint,
    odps_orderstatus string,
    odps_totalprice double
    )
    STORED BY 'com.aliyun.odps.TableStoreStorageHandler' 
    WITH SERDEPROPERTIES (
    'tablestore.columns.mapping'=':o_orderkey,:o_orderdate,o_custkey, o_orderstatus,o_totalprice', -- (3)
    'tablestore.table.name'='ots_tpch_orders'
    'odps.properties.rolearn'='acs:ram::xxxxx:role/aliyunodpsdefaultrole'
    )
    LOCATION 'tablestore://odps-ots-dev.cn-shanghai.ots-internal.aliyuncs.com'; 
    ```

    Description:

    -   com.aliyun.odps.TableStoreStorageHandler is the MaxCompute built-in storage handler to process data in Table Store.
    -   SERDEPROPERITES provides options for parameters. You must specify tablestore.columns.mapping and tablestore.table.name when using TableStoreStorageHandler.
        -   tablestore.columns.mapping: This parameter is required. It describes the columns of the table in Table Store that MaxCompute accesses, including the primary key columns and property columns. A primary key column is indicated with the colon sign \(:\) at the beginning of the column name. In this example, primary key columns are p:o\_orderkey and :o\_orderdate. The others are property columns. Table Store supports up to four primary key columns. The data types include String, Integer and Binary. The first column of the primary key is the partition key. You must specify all primary key columns of the table in Table Store when specifying the mapping. You only need to specify the property columns that MaxCompute accesses instead of specifying all property columns..
        -   tablestore.table.name: The name of the table to access in Table Store. If the table name does not exist in Table Store, an error is reported. MaxCompute does not create a table in Table Store.
    -   LOCATION: Specifies the name and the endpoint of the Table Store instance.
3.  Create a table in GUI

    Go to the Data Development page, see [Table Management](intl.en-US/User Guide/Data development/Table Management.md#) to create a table in GUI. An external table has the following attributes.

    -   Basic attributes
        -   Table name \(**Create a table** and enter the name\)
        -   Table alias
        -   Level 1 Topic and Level 2 Topic
        -   Description
    -   Physical model
        -   **Table type**: Select External table.
        -   **Partition**: External tables in Table Store do not support partitioning.
        -   **Select the memory address**: Specify the LOCATION clause. You can specify the LOCATION clause in the **Physical model** section. **Select an option** the storage location of the external table in the dialog box. Then you can perform **Authorize**.
        -   **Select storage format**: Select the file format as required. CSV, TSV, TextFile, SequenceFile, RCFile, AVRO, ORC, and Parquet, and custom file formats are supported. If you select a custom file format, you need to select the corresponding resource. The classes are parsed from the resources automatically when you submit the resources. You can select the class name.
        -   rolearn: If you have STS authorization, you do not need to specify the rolearn attribute.
    -   Table structure design

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21811/155168397312750_en-US.png)

        -   **Data type**: MaxCompute 2.0 supports INYINT, SMALLINT, INT, BIGINT, VARCHAR and STRING types for fields.
        -   **Actions**: You can create, modify, and delete the fields.
        -   **Length/Set**: You can set the maximum length of the VARCHAR type columns. For composite data types, you can fill in the definitions for them.

## Supported data type {#section_s3k_zvq_gfb .section}

Basic data types that are supported by external tables are shown in the following table.

|Data type|New|Examples|Description|
|TINYINT|Yes|1Y，-127Y|A signed eight-bit integer in the range -128 to 127.|
|SMALLINT|Yes|32767S， -100S|A signed 16-bit integer in the range -32,768 to 32,767.|
|INT|Yes|1000, -15645787|A signed 32-bit integer in the range -231 to 231-1.|
|BIGINT|No|100000000000L, -1L|A signed 64-bit integer in the range -263 + 1 to 263 - 1.|
|FLOAT|Yes|None|A 32-bit binary floating point number.|
|DOUBLE|No|3.1415926 1E+7|An eight-byte double precision floating-point number \(a 64-bit binary floating point number\).|
|DECIMAL|No|3.5BD， 99999999999.9999999BD|A decimal exact numeric. Precision can range from -1036 + 1 to 1036 -1, scale from 10 to 18.|
|VARCHAR\(n\)|Yes|None|A variable-length character string. The length is n that is in the range 1 to 65535.|
|STRING|No|“abc”，’bcd’，”alibaba”|A string. Currently, the maximum length is 8M.|
|BINARY|Yes|None|A binary number. Currently, the maximum length is 8M.|
|DATETIME|No|DATETIME ‘2017-11-11 00:00:00’|The data type for dates and times. UTC–8 is used as the standard time of the system. The range is from 0000-01- 01 to 9999-12-31, accurate to a millisecond.|
|TIMESTAMP|Yes|TIMESTAMP ‘2017-11-11 00:00:00.123456789’|TIMESTAMP data type, which is independent of time zones. The range is from 0000-01- 01 to 9999-12-31, accurate to a nanosecond.|
|BOOLEAN|No|TRUE，FALSE|Logical Boolean \(TRUE/FALSE\)|

Composite data types supported by external tables are shown in the following tables.

|Type|Definition|Constructor|
|ARRAY|array< int \>; array< struct< a:int, b:string \>\>|array\(1, 2, 3\); array\(array\(1, 2\); array\(3, 4\)\)|
|MAP|map< string, string \>; map< smallint, array< string\>\>|map\(“k1”, “v1”, “k2”, “v2”\); map\(1S, array\(‘a’, ‘b’\), 2S, array\(‘x’, ‘y\)\)|
|STRUCT|struct< x:int, y:int\>; struct< field1:bigint, field2:array< int\>, field3:map< int, int\>\>|named\_struct\(‘x’, 1, ‘y’, 2\); named\_struct\(‘field1’, 100L, ‘field2’, array\(1, 2\), ‘field3’, map\(1, 100, 2, 200\)|

If you need to use data types newly supported by MaxCompute 2.0 \(TINYINT、SMALLINT、 INT、 FLOAT、VARCHAR、TIMESTAMP 、BINARY or composite data types\), you need to include `set odps.sql.type.system.odps2=true;` before the statements to create a table. Submit and execute the statements to create a table with the set statement. If compatibility with HIVE is required, we recommend that you include the `odps.sql.hive.compatible=true;` statement.

## View and process external tables {#section_tjs_1hs_gfb .section}

You can find the external tables in the [Tables](intl.en-US/User Guide/Data development/Table Management.md#) view.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21811/155168397312827_en-US.png)

The processing of external tables is similar to that of internal tables. For more information about external tables, see [Table Management](intl.en-US/User Guide/Data development/Table Management.md#) and [All data](intl.en-US/User Guide/Data management/All data.md#).

