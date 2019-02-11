# Configuring FTP Reader {#concept_zzv_ww3_p2b .concept}

FTP Reader provides the capability to read data from a remote FTP file system. At the underlying implementation level, FTP Reader acquires the remote FTP file data, converts data to the data synchronization and transmission protocol, and transmits it to Writer.

What is saved to the local file is a two-dimensional table in a logic sense, for example, text information in a CSV format.

FTP Reader allows you to read data from a remote FTP file and convert the data to the data synchronization protocol. Remote FTP file itself is a non-structured data storage file. For data synchronization, FTP Reader currently supports the following features:

-   Only supports reading TXT files and the schema in the TXT file must be a two-dimensional table.
-   Supports CSV-like format files with custom delimiters.
-   Supports reading multiple types of data \(represented by String\) and supports column pruning and column constants.
-   Supports recursive reading and filtering by File Name.
-   Supports text compression. The available compression formats, include gzip, bzip2, zip, lzo, and lzo\_deflate.
-   Supports concurrent reading of multiple files.

The following two features are not supported currently:

-   Multi-thread concurrent reading of a single file. This feature involves the internal splitting algorithm of a single file \(under planning\).
-   Technically, the multi-thread concurrent reading of a single compressed file is not supported.

The remote FTP file itself does not provide data types, which are defined by DataX FtpReader:

|Internal DataX type|Data type of a remote FTP file|
|:------------------|:-----------------------------|
|Long|Long|
|Double|Double|
|String|String|
|Boolean|Boolean|
|Date|Date|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|path|The path of the remote FTP file system. Multiple paths can be specified.-   If a single remote FTP file is specified, FTP Reader only supports single-threaded data extraction. We are planning to provide the function to concurrently read a single non-compressed file with multiple threads.
-   If multiple remote FTP files are specified, FTP Reader can extract data with multiple threads. The number of concurrent threads is specified based on the number of channels.
-   If a wildcard is specified, FTP Reader attempts to traverse multiple files. For example, when / is specified, FTP Reader reads all the files under the / directory. When /bazhen/ is specified, FTP Reader reads all the files under the bazhen directory.  Currently, FTP Reader only supports \* as the file wildcard.

**Note:** 

-   The data synchronization system identifies all text files synchronized in a job as a same data table. You must ensure that all files are applicable to the same schema information.
-   You must ensure that the file to be read is in CSV‑like format, and the read permission must be granted to the data synchronization system.
-   If no matching file exists for extraction in the path specified by Path, an error may occur in the synchronization task.

|Yes-|N/A|
|column| It refers to the list of fields read, where the type indicates the type of source data. The index indicates the column in which the current column locates \(starts from 0\), and the value indicates that the current type is constant. The data is not read from the source file, but the corresponding column is automatically generated according to the value. 

 By default, you can read data by taking String as the only type. The configuration is as follows: `"column": ["*"]`. You can configure the column field as follows:

```
{
    "type": "long",
    "index": 0 //Read the int field from the first column of the remote FTP file text
  },
  {
    "type": "string",
    "value": "alibaba" //FtpReader internally generates the alibaba string field as the current field
  }
```

 For the specified column information, you must enter type and choose one from index/value.

 |Yes|Read all according to string type|
|fieldDelimiter|The delimiter used to separate the read fields.**Note:** Note that a field delimiter must be specified when FTP Reader reads data. By default, if commas \(,\) are not specified, it is entered in the interface configuration.

|Yes|,|
|Skipheader|The header of a file in CSV-like format is skipped if it is a title. Headers are not skipped by default. skipHeader is not supported for file compression.|No|False|
|encoding|Encoding of the read files.|No |utf-8|
|nullFormat|Defining null \(null pointer\) with a standard string is not allowed in text files. Data synchronization provides nullFormat to define which strings can be expressed as null.For example, when `nullFormat:“null”`, is configured, if the source data is "null", it is considered as a null field in data synchronization.

|No|N/A|
|markDoneFileName|The name of the file marked as "done". Check MarkDoneFile before data synchronization. If the file does not exist, wait for a while and check again. If the file exists, start the data synchronization task.|No|N/A|
|MaxRetryTime|The number of attempts made to check MarkDoneFile. The default value is 60. Try every minute for a duration of 60 minutes.|No|600|
|csvReaderConfig|Reads the CSV files parameter configurations. It is the Map type. This reading is performed by the CsvReader for reading CSV files and involves many configuration items. Not configured items will use default settings.|No|N/A|
|fileFormat|The read file type. By default, the file is read as a CVS file and the file content is parsed to a logical two-dimensional table for processing. If you set this file to binary, the file is copied and transmitted in the binary format. Such setting is applicable for peer-to-peer copy of directories between FTP and OSS files. Generally, you do not need to configure this item.|No|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the data source and destination for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15498534477697_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the configured data source name.
    -   File path: The path in the above parameter description.
    -   Column delimiter: The fieldDelimiter in the preceding parameter description, which defaults to a comma \(,\).
    -   Encoding format: Encoding in the preceding parameter description, which defaults to utf-8.
    -   null value: nullFormat in the preceding parameter description to define a string that represents the null value.
    -   Compression format: Compress in the preceding parameter description, which defaults to "no compression".
    -   Whether to include the table header: skipHeader in the preceding parameter description, which defaults to "No".
2.  Field mapping: The column in the preceding parameter description.

    The source table field on the left and the target table field on the right are one-to-one correspondences, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15498534477701_en-US.png)

    -   In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Manually edit source table field: Manually edit the fields, and each line indicates a field. The first and end blank lines are ignored.
3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15498534487704_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources, including CPU, memory, and network bandwidth consumed during data integration. It represents a unit of data synchronization processing capability given limited CPU, memory, and network resources.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task resource group: The machine on which the task runs. If the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group currently only East China 1 and East China 2 supports adding custom resource groups. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common Configuration/Add task resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a synchronous Extraction Data job from the FTP database.

```
{
    "type": "job",
    "version": "2.0"} //Indicates the version.
    "steps":[
        {
            "stepType": "ftp", // plug-in name
            "parameter": {
                "path":[],//File path
                "nullFormat": "", // Null Value
                "compress": "", // compression format
                "datasource": "", // Data Source
                "column": [// Field
                    {
                        "index": 0, // serial number
                        "type": "// Field Type
                    }
                ],
                "skipHeader": "", // contains a header?
                "fieldDelimiter": "," //Delimiter of each column
                "encoding": "UTF-8", // encoding format
                "fileFormat": "csv"//File type
            },
            "name": "Reader ",
            "category": "reader"
        },
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType": "stream ",
            "parameter":{}
            "name": "Writer ",
            "category": "writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record": "0"//Number of error records
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

