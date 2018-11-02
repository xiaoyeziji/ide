# Configure HBase Writer {#concept_nr5_w5l_q2b .concept}

In this article we will show you the data types and parameters supported by Stream Writer and how to configure Writer in script mode.

The HBase Writer plug-in provides the function to write data into HBase.  At the underlying implementation level, HBase Writer connects to a remote HBase service through the HBase Java client, and writes data into HBase in put mode.

## Supported features {#section_wv3_x5l_q2b .section}

-   **HBase0.94.x and HBase1.1.x versions are supported**
    -   If you use HBase 0.94.x, choose HBase094x as the Writer plug-in.For example:

        ```
        "writer": {
                "plugin": "hbase094x"
            }
        ```

    -   If you use HBase 1.1.x, choose HBase11x as the Writer plug-in.For example:

        ```
        "writer": {
                "plugin": "hbase11x"
            }
        ```

-   **Multiple fields in the source end can be concatenated into a rowkey**

    Currently, HBase Writer can concatenate multiple fields in the source end into the rowkey of an HBase table. For details, see the rowkeyColumn configuration.

-   **Support to versions of data written into HBase**

    Supported timestamps \(versions\) for data written into HBase include:

    -   Current time
    -   Specified source column
    -   Specified time

HBase Reader supports HBase data types and converts HBase data types as follows:

|Data integration internal types|Hbase Data Type|
|:------------------------------|:--------------|
|Long|int,short,long|
|float,double|float,double|
|String|String|
|Boolean|Boolean|

**Note:** Apart from the field types listed here, other types are not supported. 

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|haveKerberos|Description: If haveKerberos is True, the HBase cluster needs to be authenticated using kerberos.**Note:** 

-   NOTE: If this value is configured as true, the following five parameters related to kerberos authentication must be configured: kerberosKeytabFilePath、kerberosPrincipal、hbaseMasterKerberosPrincipal、hbaseRegionserverKerberosPrincipal and hbaseRpcProtection.
-   If the HBase cluster is not authenticated using kerberos, these six parameters are not required.

|No|false|
|hbaseConfig|Description: Configuration required for connecting to the HBase cluster, in JSON format. The required item is hbase.zookeeper.quorum, which indicates the URL of HBase ZK. In addition, more HBase client configurations can be added. For example, you can configure the cache and batch of scan to optimize the interaction with servers.|Yes|None |
|mode|Description: The mode in which data is written into HBase. Currently, only the normal mode is supported. The dynamic column mode will be available later.|Yes|None |
|table|Description: Name of the HBase table to be written. The name is case sensitive. |Yes|None |
|encoding|Description: The encoding method is UTF-8 or GBK, which is used when data in string is converted to HBase byte\[\].|No|UTF-8|
|column|Description: The HBase field to be written.-   index: Specify the index of the column that corresponds to the column of the Reader, starting from 0.
-   name: Specifies the column in the HBase table, which must be in column family:column name format.
-   type: Specifies the type of data to be written, which is used to convert HBase byte\[\].

|Yes|N/A|
|maxVersion|Description: Specify the number of versions of data to be read by HBase Reader in multi-version mode, which can only be -1 \(to read all versions\) or a number larger than 1.|The configuration format is as follows:|None |
|range|Specifies the rowkey range that the hbase reader reads.-   startRowkey: Specify start rowkey.
-   endRowkey: Specify end rowkey.
-   isBinaryRowkey: Specifies the way in which the configured startrowkey and endrowkey are converted to byte, the default is false. If it is true, Bytes.toBytesBinary\(rowkey\) is called for conversion. If it is false, Bytes.toBytes\(rowkey\) is called. The configuration format is as follows:

    ```
"range": {
"startRowkey":"aaa",
"endRowkey":"ccc",
"isBinaryRowkey":false
}
    ```

The format of the configuration file is as follows:

    ```
"column": [
        {
          "index":1,
          "name": "cf1:q1",
          "type": "string",
        },
        {
          "index":2,
          "name": "cf1:q2",
          "type": "string",
        }
     ］
    ```


|No|N/A|
|rowkeyColumn|Rowkey column of the hbase to write.-   index: Specify the index of the column that corresponds to the column of the Reader, starting from 0. If it is a constant, index is-1.
-   type: Specifies the type of data to be written, which is used to convert HBase byte\[\].
-   value: A configuration constant, which is usually used as the concatenation operator of multiple fields. HBase Writer concatenates all columns of the rowkeyColumn into a rowkey in the configuration sequence to write data into HBase. The rowkey cannot contain constants only.

The format of the configuration file is as follows:

```
"rowkeyColumn": [
          {
            "index":0,
            "type":"string"
          },
          {
            "index":-1,
            "type":"string",
            "value":"_"
          }
      ]
```

|Yes|None |
|versionColumn|Description: Specifies the timestamp when data is written into HBase. You can choose any one of three options: current time, specified time column, and specified time. The current time is used if this parameter is not configured. Supports the current time, the specified time column, or the specified time \(one of the three choices \), indicates that the current time is used if it is not configured.-   index: Specify the index that corresponds to the column of the Reader, starting from 0. The index must be able to be converted to long.
-   type: If the type is Date, HBase Writer parses the data in the formats of yyyy-MM-dd HH:mm:ss and yyyy-MM-dd HH:mm:ss SSS. The index is –1 in case of a specified time.
-   value: The long value of the specified time.

The format of the configuration file is as follows:

-   ```
"versionColumn":{
"index":1
}
```

-   ```
"versionColumn":{
"index":－1,
"value":123456789
}
```


 |No|None |
|nullMode|Description: Processing mode when a null value is read. The following two methods are supported:-   skip: This column is not written into HBase.
-   empty: HConstants.EMPTY\_BYTE\_ARRAY is written into HBase, that is, new byte \[0\].

| No|skip|
|walFlag|Description: When committing data to the RegionServer in the cluster \(Put/Delete operation\), the HBase client writes the WAL \(Write Ahead Log, which is an HLog shared by all Regions on a RegionServer\). The HBase client writes data into MemStore only after it successfully writes data into WAL. In this case, the client is notified that the data is successfully committed. In case of failure to write the WAL, HBase Client is notified that the commit is failed. Disable walFlag \(false\) to stop writing the WAL so as to improve the performance of data writing.|No|false|
|writeBufferSize|Description: Set the buffer size \(in byte\) of the HBase client. Use it with autoflush.autoflush：

-   autoflush: If it is set to true, the HBase client performs an update operation for each put request. I
-   If it is set to false, the HBase client initiates a write request to the HBase server only when the client write buffer is filled up with the put requests.

|No|8 MB|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is not supported currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to write data from a local machine into hbase1.1.x:

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"hbase",plug-in name
            "parameter":{
                "mode":"normal", //mode written to hbase
                "walFlag":"false", //close (false) give up writing Wal log
                "hbaseVersion": "094x", //Hbase version
                "rowkeyColumn": [// The rowkey column of the hbase to write.
                    {
                        "index": 0, //serial number
                        "type": "string" // data type
                    },
                    {
                        "index":"-1",
                        "type":"string",
                        "value":"_"
                    }
                ],
                 
"nullMode":"skip",//How do I handle null values read by "Skip?
                "column": [// The hbase field to write.
                    {
                        "name": "columnFamilyName1:columnName1", // field name
                        "index": "0", // Index Number
                        "type": "string" // data type
                    },
                    {
                        "name":"columnFamilyName2:columnName2",
                        "index":"1",
                        "type":"string"
                    },
                    {
                        "name":"columnFamilyName3:columnName3",
                        "index":"2",
                        "type": "string",
                    }
                ],
                "writeMode": "api", // write mode is
                "encoding": "utf-8", // encoding format
                "table": ", // table name
                "hbaseConfig":{// configuration information required to connect to the hbase cluster, JSON format.
                    "hbase.zookeeper.quorum":"hostname",
                    "hbase.rootdir":"hdfs: //ip:port/database",
                    "hbase.cluster.distributed":"true"
                }
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record": "0"//Number of error records
        },
        "speed":{
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 // DMU Value
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

