# Configure OSS Reader {#concept_d33_12q_p2b .concept}

The OSS Reader plug-in provides the ability to read data from OSS data storage. In terms of underlying implementation, OSS Reader acquires the OSS data using official OSS Java SDK, converts the data to the data synchronization protocol, and passes it to Writer.

-   If you want to learn more about OSS products, see the [OSS Product Overview](https://www.alibabacloud.com/help/doc-detail/31817.htm).
-   For details about OSS Java SDKs, see [Alibaba Cloud OSS Java SDK](http://oss.aliyuncs.com/aliyun_portal_storage/help/oss/OSS_Java_SDK_Dev_Guide_20141113.pdf).
-   For details on processing non-structured data such as the OSS data, see [Process Non-structured Data](https://www.alibabacloud.com/help/doc-detail/45389.htm).

OSS Reader provides the ability to read data from a remote OSS file and convert the data to the Data Integration/datax protocol. OSS file itself is a non-structured data storage. For Data Integration/datax, OSS Reader currently supports the following features:

-   Only supports reading TXT files and the shema in the TXT file must be a two-dimensional table.
-   Supports CSV‑like format files with custom delimiters.
-   Supports reading multiple types of data \(represented by String\) and supports column pruning and column constants.
-   Supports recursive reading and filtering by File Name.
-   Supports text compression. The available compression formats include gzip, bzip2, and zip.

    **Note:** Note: Multiple files cannot be compressed into one package.

-   Supports concurrent reading of multiple objects.

The following are not supported currently:

-   Multi-thread concurrent reading of a single object \(file\).
-   - Technically, the multi‑thread concurrent reading of a single compressed object is not supported.

OSS Reader supports the following data types of OSS: BIGINT, DOUBLE, STRING, DATATIME, and BOOLEAN.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|Object|The object information for the OSS, where you can support filling in multiple objects. For example, if the bucket of xxx contains yunshi folder which has ll.txt file, the object is directly specified as yunshi/ll.txt.-   If a single OSS object is specified, OSS Reader only supports single-threaded data extraction. We are planning to provide the function to concurrently read a single non-compressed object with multiple threads.
-   If multiple OSS objects are specified, OSS Reader can extract data with multiple threads. The number of concurrent threads is specified based on the number of channels.
-   - If a wildcard is specified, OSS Reader attempts to traverse multiple objects. For details, see [OSS Product Overview](https://www.alibabacloud.com/help/doc-detail/31827.htm).

**Note:** \> Data synchronization system identifies all objects synchronized in a job as a same data table. You must ensure that all objects are applicable to the same schema information.

|Yes|N/A|
|column|Description: It refers to the list of fields read, where the type indicates the type of source data, the index indicates the column in which the current column locates \(starts from 0\), and the value indicates that the current type is constant and the data is not read from the source file but the corresponding column is automatically generated according to the value.By default, you can read data by taking String as the only type. The configuration is as follows: 

```
json
"column": ["*"]
```

You can configure the column field as follows:

```
json
"column":
    {
       "type": "long",
       "index": 0 //Retrieves the int field from the first column of the local file text
    },
    {
       "type": "string",
       "value": "alibaba" // HDFS Reader internally generates the alibaba string field as the current field
    }
```

**Note:** For the specified column information, you must enter type and choose one from index/value.

|Yes|Read all according to string type|
|fieldDelimiter|The read field separator.**Note:** The OSS reader needs to specify a field partition when reading data, if you do not specify a default of ';', the interface configuration also defaults ','.

|Yes|,|
|compress|Compression type of files. It is left empty by default, which means no compression is performed. Supports the following compression types: gzip, bzip2, and zip.|No|Do not compress|
|encoding|Description: Encoding of the read files.|No|UTF-8|
|nullFormat|Description: Defining null \(null pointer\) with a standard string is not allowed in text files. Data Synchronization system provides nullFormat to define which strings can be expressed as null. For example, if the source data is "null", if you configure the `nullformat = "null "`, the data synchronization system is treated as a null field.|No|N/A|
|Skipheader|Description: The header of a file in CSV‑like format is skipped if it is a title. Headers are not skipped by default. skipHeader is not supported for file compression.|No |false|
|csvReaderConfig|Description: Reads the parameter configurations of CSV files. It is the Map type. This reading is performed by the CsvReader for reading CSV files and involves many configuration items, whose defaults are used if they are not configured.|No |N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15367217907815_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Object Prefix: Object in the preceding parameter description.

        **Note:** If your OSS file name has a section named according to the time of day, such as aaa/20171024abc.txt, about the object system parameters, aaa/$\{bdp.system.bizdate\}abc.txtcan be set.

    -   Column delimiter: fieldDelimiter in the preceding parameter description, which defaults to ",".
    -   Encoding format: encoding in the preceding parameter description, which defaults to utf-8.
    -   null Value: nullFormat in the preceding parameter description. Enter the field to be expressed as null into a text box. If source end exists, the corresponding field is converted to null.
    -   Compression Format: compress in the preceding parameter description, which defaults to "no compression".
    -   Whether to Include the Table Header: skipHeader in the preceding parameter description, which defaults to "No".
2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15367217907818_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367217907675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.

## Development in script mode {#section_cp2_wsh_p2b .section}

The following is a script configuration sample. For details about parameters, see the preceding Parameter Description.

```
{
    "type": "job",
    "version": "2.0",//Indicates the version.
    "steps":[
        {
            "stepType": "oss", // plug-in name
            "parameter": {
                "nullFormat": "", // nullformat defines which strings can be expressed as null?
                "compress": "", // text compression type
                "datasource": "", // Data Source
                "column": [// Field
                    {
                        "index": 0, // column sequence number
                        "type": "string" // data type
                    },
                    {
                        "index": 1,
                        "type": "long"
                    },
                    {
                        "index": 2,
                        "type": "double"
                    },
                    {
                        "index": 3,
                        "type": "boolean"
                    },
                    {
                        "format":"yyyy-MM-dd HH:mm:ss", // time format
                         "index":4,
                        "type": "date"
                    }
                ],
                 "skipHeader":"",// the class CSV format file may have a header as a header condition, need to skip
                "encoding":"",//encoding format
                "fieldDelimiter":",",//Separator
                "fileFormat": "",//File type
                "object": []//object prefix
            },
             "name":"Reader",
            "category":"reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream ",
            "parameter":{},
            "name": "Writer ",
            "category": "writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record": ""//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from": "Reader ",
                "to": "Writer"
            }
        ]
    }
}
```

