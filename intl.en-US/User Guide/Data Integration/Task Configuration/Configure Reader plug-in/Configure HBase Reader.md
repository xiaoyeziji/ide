# Configure HBase Reader {#concept_i2s_xgj_p2b .concept}

The HBase Reader plug‑in provides the ability to read data from HBase. At the underlying implementation level, HBase Reader connects to remote HBase service with HBase's Java client, reads data within the rowkey range you specified by means of Scan, then assembles data into an abstract dataset using custom data type for Data Integration, and passes the dataset to downstream Writer for processing.

## Supported features { .section}

-   **HBase 0.94.x and HBase 1.1.x versions are supported**
    -   If you use HBase 0.94.x, choose HBase094x as the reader plug-in, as shown in the following figure:

        ```
        "reader": {
                "plugin": "hbase094x"
            }
        ```

    -   If you use HBase 1.1.x, choose HBase11x as the reader plug-in, as shown in the following figure:

        ```
        "reader": {
                "plugin": "hbase11x"
            }
        ```

-   **The normal and multiVersionFixedColumn modes are supported**
    -   normal mode: Read the latest version of data from an HBase table which is used as an ordinary two-dimensional table \(horizontal table\). For example:

        ```
        hbase(main):017:0 is greater than scan 'users'
        ROW COLUMN+CELL
        lisi column=address:city, timestamp=1457101972764, value=beijing
        lisi column=address:contry, timestamp=1457102773908, value=china
        lisi column=address:province, timestamp=1457101972736, value=beijing
        lisi column=info:age, timestamp=1457101972548, value=27
        lisi column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi column=info:company, timestamp=1457101972653, value=baidu
        xiaoming column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming column=address:contry, timestamp=1457082195729, value=china
        xiaoming column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming column=info:age, timestamp=1457082218735, value=29
        xiaoming column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0580 seconds }
        ```

        The data read from the table is shown as follows:

        |rowKey|addres:city|address:contry|address:province|info:age|info:birthday|info:company|
        |:-----|:----------|:-------------|:---------------|:-------|:------------|:-----------|
        |Lisi|beijing|china|beijing|27|1987-06-17|baidu|
        |xiaoming|hangzhou|china|zhejiang|29|1987-06-17|alibaba|

    -   multiVersionFixedColumn mode: Read data from an HBase table which is used as an vertical table. Each record read from the table is shown in the form of the following four columns: rowKey, family:qualifier, timestamp, and value. You must specify the column to be read when reading data. The value of each cell is a record. 

        ```
        hbase(main):018:0 is greater than scan 'users',{VERSIONS=>5}
        ROW COLUMN+CELL
        lisi column=address:city, timestamp=1457101972764, value=beijing
        lisi column=address:contry, timestamp=1457102773908, value=china
        lisi column=address:province, timestamp=1457101972736, value=beijing
        lisi column=info:age, timestamp=1457101972548, value=27
        lisi column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi column=info:company, timestamp=1457101972653, value=baidu
        xiaoming column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming column=address:contry, timestamp=1457082195729, value=china
        xiaoming column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming column=info:age, timestamp=1457082218735, value=29
        xiaoming column=info:age, timestamp=1457082178630, value=24
        xiaoming column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0260 seconds }
        ```

        The data read from the table \(in four columns\)

        |rowKey|column:qualifier|timestamp|value|
        |:-----|:---------------|:--------|:----|
        |lisi|address:city|1457101972764|beijing|
        |Lisi|address:contry|1457102773908|china|
        |lisi|address:province|1457101972736|beijing|
        |lisi|Info: Age|1457101972548|27|
        |lisi|info:birthday|1457101972604|1987-06-17|
        |lisi|info:company|1457101972653|beijing|
        |Aging|address:city|1457082196082|hangzhou|
        |xiaoming|address:contry|1457082195729|china|
        |xiaoming|address:province|1457082195773|zhejiang|
        |xiaoming|info:age|1457082218735|29|
        |xiaoming|info:age|1457082178630|24|
        |xiaoming|info:birthday|1457082186830|1987-06-17|
        |xiaoming|info:company|1457082189826|alibaba|


HBase Reader supports HBase data types and converts HBase data types as follows:

|Data integration internal types| HBase data type|
|:------------------------------|:---------------|
|Long|Int, short, and long|
|Double|Float and double|
|String|String and binarystring|
|Date|Date|
|Boolean |Boolean|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|haveKerberos|Description: If haveKerberos is True, the HBase cluster needs to be authenticated using kerberos. **Note:** 

-   NOTE: If this value is configured as true, the following five parameters related to kerberos authentication must be configured:  Kerberoskeytabfilepath, kerberosprincipal, hbasemasterkerberosprincipal, hbaseregionserverkerberosprincipal and hbaserpcprotection.
-   If the HBase cluster is not authenticated using kerberos, these six parameters are not required.

|No|false|
|hbaseConfig|Description: Configuration required for connecting to the HBase cluster, in JSON format. The required item is hbase.zookeeper.quorum, which indicates the URL of HBase ZK.  In addition, more HBase client configurations can be added. For example, you can configure the cache and batch of scan to optimize the interaction with servers.|Yes|N/A|
|mode|Description: Read mode of HBase. The normal and multiVersionFixedColumn modes are supported.|Yes|N/A|
|table.|Name of HBase table to be read \(case-sensitive\).|Yes|N/A|
|encoding|Description: Encoding method \(UTF-8 or GBK\). This is used when HBase byte\[\] stored in binary form is converted into String.|No|UTF-8|
|column|Description: HBase field to be read. This item is required in both normal and multiVersionFixedColumn modes.-   **In normal mode:**

In normal mode: Except for rowkey, the HBase columns specified by the item name for reading must be in the format of column family:column name. The item type specifies the type of source data. The item format specifies the format of date. The item value specifies the current type as a constant. The system does not read data from HBase, but generates corresponding columns based on the value. The configuration format is as follows:

    ```
"column": 
[
{
  "name": "rowkey",
  "type": "string",
},
{
  "value": "test",
  "type": "string",
}
]
    ```

In normal mode, for the specified Column information, you must enter type and choose one from name/value.

-   **In multiVersionFixedColumn mode**

Except for rowkey, the HBase columns specified by the item name for reading must be in the format of column family:column name.  The constant column is not supported in multiVersionFixedColumn mode. The configuration is as follows:

    ```
"column": 
[
{
  "Name": "rowkey ",
  "type": "string",
},
{
  "name": "info: age",
  "type": "string",
}
]
    ```


|Yes|N/A|
|maxVersion|Description: Specify the number of versions of data to be read by HBase Reader in multi-version mode, which can only be -1 \(to read all versions\) or a number larger than 1. |Required: This is required in multiVersionFixedColumn mode.|N/A|
|range|Specifies the rowkey range that the hbase reader reads.-   startRowkey: Specify start rowkey.
-   endRowkey: Specify end rowkey.
-   isBinaryRowkey: Specify the method for converting configured startRowkey and endRowkey to byte\[\]. The default value is false. If it is true, Bytes.toBytesBinary\(rowkey\) is called for conversion. If it is false, Bytes.toBytes\(rowkey\) is called. If it is true, Bytes.toBytesBinary\(rowkey\) is called for conversion.   If it is false, Bytes.toBytes\(rowkey\) is called. The configuration format is as follows:

    ```
"range": {
"startRowkey":"aaa",
"endRowkey":"ccc",
"isBinaryRowkey":false
}
    ```


|No|N/A|
|scanCacheSize|Description: Number of lines read by the HBase client from the server every time when RPC is performed.|No|256|
|scanBatchSize|Description: Number of columns read by the HBase client from the server every time when RPC is performed. |No|1,000|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is not supported currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to extract data from HBase to local machine: \(normal mode\).

```
{
    "type":"job",
    "version":"2.0",//Indicates the version.
    "steps":[
        {
            "stepType":"hbase",plug-in name
            "parameter":{
                "mode": "normal", //read HBase mode, supports normal mode, multiVersionFixedColumn Mode
                "scanCacheSize": 256,//Number of lines read by the HBase client from the server every time when RPC is performed.
                "scanBatchSize": 100 ", //The number of columns that the HBase client reads per rpc from the server. 
                "hbaseVersion": "9.4x/11x", //hbase version
                "column":[//Field
                    {
                        "name":"rowkey", //field name
                        "type":"string" //data type
                    },
                    {
                        "name":"columnFamilyName1: columnname1 ",
                        "type":"string",
                    },
                    {
                        "name":"columnFamilyName2:columnName2",
                        "format":"yyyy-MM-dd",
                        "type":"date",
                    },
                    {
                        "name":"columnFamilyName3:columnName3",
                        "type":"long"
                    }
                ],
                "range":{//specify the rowkey range that the HBase Reader reads.
                    "endRowkey":"", //specify end rowkey.
                    "isBinaryRowkey":true,//Specify the method for converting configured startRowkey and endRowkey to byte[]. The default value is false. If it is true, Bytes.toBytesBinary(rowkey) is called for conversion. If it is false, Bytes.toBytes(rowkey) is called.
                    "startRowkey":"//specify the start rowkey.
                },
                "maxVersion":"", //specify the number of versions read by hbase reader in Multi-version Mode
                "encoding":"UTF-8", //encoding format
                "table":"ok",//The name of the target table.
                "hbaseConfig":{// configuration information required to connect to the hbase cluster, JSON format.
                    "hbase.zookeeper.quorum":"hostname",
                    "hbase.rootdir":"hdfs://ip:port/database",
                    "hbase.cluster.distributed":"true"
                }
            },
            "name":"Reader",
            "category":"reader"
        },
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record":"0"//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent":"1",//Number of concurrent tasks
            "dmu":1//DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

