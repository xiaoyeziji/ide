# Configure HBase reader {#concept_i2s_xgj_p2b .concept}

The HBase Reader plug-in provides the capability to read data from HBase. At the underlying implementation level, the HBase Reader connects to the remote HBase service with HBase’s Java client. Reads data within the RowKey range specified by Scan, then assemble data into an abstract dataset using custom Data Integration data type, and pass dataset to the downstream Writer for processing.

## Supported features { .section}

-   **HBase0.94.x and HBase1.1.x versions**
    -   If you use HBase 0.94.x, select the HBase094x as the reader plug-in, as follows :

        ```
        "reader": {
                "plugin": "hbase094x"
            }
        ```

    -   If you use HBase 1.1.x, select HBase11x as the reader plug-in, as follows:

        ```
        "reader": {
                "plugin": "hbase11x"
            }
        ```

-   **Normal and multiVersionFixedColumn modes**
    -   normal mode: Read the latest data version from a HBase table, which is used as an ordinary two-dimensional table \(horizontal table\). For example:

        ```
        hbase(main):017:0 is greater than scan 'users'
        ROW COLUMN+CELL
        lisi column=address:city, timestamp=1457101972764, value=beijing
        lisi column=address:country, timestamp=1457102773908, value=china
        lisi column=address:province, timestamp=1457101972736, value=beijing
        lisi column=info:age, timestamp=1457101972548, value=27
        lisi column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi column=info:company, timestamp=1457101972653, value=baidu
        xiaoming column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming column=address:country, timestamp=1457082195729, value=china
        xiaoming column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming column=info:age, timestamp=1457082218735, value=29
        xiaoming column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0580 seconds }
        ```

        The data read from the table is shown as follows:

        |rowKey|address:city|address:country|address:province|info:age|info:birthday|info:company|
        |:-----|:-----------|:--------------|:---------------|:-------|:------------|:-----------|
        |lisi|beijing|china|beijing|27|1987-06-17|baidu|
        |xiaoming|hangzhou|china|zhejiang|29|1987-06-17|alibaba|

    -   multiVersionFixedColumn mode: Reads data from a HBase table, which is used as a vertical table. Each record read from the table is shown in the following four columns: rowKey, family:qualifier, timestamp, and value. You must specify the column when reading the data, where each cell value is a record. Multiple records are available if multiple data versions exist, see the following: 

        ```
        hbase(main):018:0 is greater than scan 'users',{VERSIONS=>5}
        ROW COLUMN+CELL
        lisi column=address:city, timestamp=1457101972764, value=beijing
        lisi column=address:country, timestamp=1457102773908, value=china
        lisi column=address:province, timestamp=1457101972736, value=beijing
        lisi column=info:age, timestamp=1457101972548, value=27
        lisi column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi column=info:company, timestamp=1457101972653, value=baidu
        xiaoming column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming column=address:country, timestamp=1457082195729, value=china
        xiaoming column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming column=info:age, timestamp=1457082218735, value=29
        xiaoming column=info:age, timestamp=1457082178630, value=24
        xiaoming column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0260 seconds }
        ```

        Data read from the table \(in four columns\):

        |rowKey|Column:qualifier|Timestamp|Value|
        |:-----|:---------------|:--------|:----|
        |lisi|address:city|1457101972764|beijing|
        |lisi|address:contry|1457102773908|china|
        |lisi|address:province|1457101972736|beijing|
        |lisi|info: age|1457101972548|27|
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

|Data integration internal types|HBase data type|
|:------------------------------|:--------------|
|Long|Int, short, and long|
|Double|Float and double|
|String|String and binarystring|
|Date|Date|
|Boolean |Boolean|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|haveKerberos|If haveKerberos is true, the HBase cluster must use Kerberos for authentication. **Note:** 

-   If the value is true, the following five parameters related to Kerberos authentication must be configured: kerberosKeytabFilePath, kerberosPrincipal, hbaseMasterKerberosPrincipal, hbaseRegionserverKerberosPrincipal, and hbaseRpcProtection.
-   If the HBase cluster is not authenticated with Kerberos, these six parameters are not required.

|No|False|
|hbaseConfig|The configuration information provided by each HBase cluster for the Data Integration client connection is stored in the hbase-site.xml. Contact your HBase PE for configuration information, and convert the configuration into JSON format. Multiple HBase client configurations can be added, for example, you can configure the cache and batch scan to optimize the servers interaction.|Yes|N/A|
|mode|Read modes of HBase. The “normal” and “multiVersionFixedColumn” are supported.|Yes|N/A|
|table|The HBase table name to be read and is case sensitive.|Yes|N/A|
|encoding|The encoding method is UTF-8 or GBK. This encoding is used when the HBase byte\[\] stored in binary form is converted into a String.|No|UTF-8|
|column|The read HBase field. This item is required for both normal and multiVersionFixedColumn modes.-   **In normal mode:**

The HBase columns specified by “name” for reading must be in the format of column family:column name except for RowKey. The “type” specifies the data source type. The “format” specifies the date format, and “value” specifies the current type as a constant. The system does not read HBase data, but generates corresponding columns based on “value”. The configuration format is shown as follows.

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

Under normal mode, you must enter the type and select an information from name or value for the specified Column information.

-   **In multiVersionFixedColumn mode**

The HBase columns specified by the item name for reading must be in the format of column family:column name except for RowKey. The constant column is not supported in multiVersionFixedColumn mode. The configuration is as follows:

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
|maxVersion|Specify the number of data versions read by the HBase Reader in multi-version mode. The specified number must be -1 to read all versions or a number larger than 1. |Required: This is required in multiVersionFixedColumn mode.|N/A|
|range|Specifies the read RowKey range of the HBase reader.-   startRowkey: Specifies the start RowKey.
-   endRowkey: Specifies the end RowKey.
-   sBinaryRowkey: Specifies the method for converting the configured startRowkey and endRowkey to byte\[\]. By default, this parameter is false. If the parameter is true, Bytes.toBytesBinary\(rowkey\) is called for conversion. If the parameter is false, Bytes.toBytes\(rowkey\) is called. The configuration format is shown as follows.

    ```
"range": {
"startRowkey":"aaa",
"endRowkey":"ccc",
"isBinaryRowkey":false
}
    ```


|No|N/A|
|scanCacheSize|The number of lines read by the HBase client from the server every time when the RPC is performed.|No|256|
|scanBatchSize|The number of columns read by the HBase client from the server every time when the RPC is performed. |No|1,000|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, development in Wizard Mode is not supported.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to extract data from HBase to the local machine under normal mode.

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

