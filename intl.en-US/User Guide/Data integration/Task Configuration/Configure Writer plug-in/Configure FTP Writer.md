# Configure FTP Writer {#concept_mrn_q4l_q2b .concept}

In this article we will show you the data types and parameters supported by FTP Writer and how to configure Writer in both wizard mode and script mode.

FTP Writer is used to write one or more files in CSV format to a remote FTP file.  At the underlying implementation level, FTP Writer converts the data under the Data Integration transfer protocol to CSV files and writes these files to the remote FTP server using FTP-related network protocols. You must configure the data source before configuring the FTP Writer plug-in. 

**Note:** For more information, see[Configure the FTP data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure the FTP data source.md#)Configure the FTP data source.

What is written and saved to the FTP file is a two-dimensional table in a logic sense, for example, text information in CSV format.

FTP Writer provides the function to convert the Data Integration protocol to a FTP file. The FTp file is a non-structured data storage file. FTP Writer supports the following features:

-   Only supports writing text files \(BLOB, for example, video data, is not supported\) and schema in the text file must be a two-dimensional table.
-   Supports CSV and text files with custom delimiters.
-   Does not support text compression during writing.
-   Supports multi-thread writing, with different subfiles written using different threads.

The following two features are not supported for the time being.

-   FTP Writer does not support the following features currently:
-   FTP itself does not provide data types. FTP Writer writes data of String type to FTP file.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.| Yes|None|
|timeout |Description: Time-out period \(in milliseconds\) of the connection to the FTP server. | No|60000 \(1 minute\)|
|path|Description: The path of the FTP file system. FTP Writer writes multiple files under the path directory. | Yes|None|
|FileName|Description: The name of the file written by FTP Writer. A random suffix is appended to the file name to form the actual name of the file written with each thread. | Yes|None|
|writeMode|Description: The mode in which FTP Writer clears existing data before writing data. Options include: -   truncate: Clear all the files prefixed by fileName in the directory before writing.
-   append: The file is not processed before writing, and Data Integration FTP Writer writes data directly using fileName without conflict of file names.
-   nonConflict: An error is reported if a file prefixed by fileName exists under the path directory.

| Yes|None|
|fieldDelimiter|Description: The delimiter used to separate the written fields. |Yes.A single character is used. |None |
|compress|Description: The gzip and bzip2 compression modes are supported. | No|Do Compress|
|encoding|Description: Encoding of the read files.|No|UTF-8|
|nullFormat|Description: Defining null \(null pointer\) with a standard string is not allowed in text files. Data Integration provides nullFormat to define which strings can be expressed as null.For example, if you configure`nullFormat="null"`, then if the source data is null, data integration is considered a null field.

| No|None|
|dateFormat|Description: The format in which data of Date type is serialized into file, for example, "dateFormat": "yyyy-MM-dd".| No|None|
|fileFormat|The format written by the file includes both CSV and text, and the CSV is a strict CSV format, if you want to write the data that includes the column separator, It is escaped in the escape syntax of the CSV, the escape symbol is double quotes. The text format is a simple division of the data to be written using the column separator, do not escape for data to be written, including column separator.|No|text|
|header|Description: The header used when a txt file is written, for example, 'id', 'name', 'age'\].|No|None|
|Markdonefilename|Description: The name of the file marked as "done". After a synchronization task is completed, a MarkDoneFile is generated, based on which whether the task is executed successfully is determined.| No|None|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15411404327697_en-US.png)

    Parameters:

    -   Data Source: datasource in the preceding parameter description. Select the FTP data source.
    -   File Path: path in the preceding parameter description.
    -   Column delimiter: fieldDelimiter in the preceding parameter description, which defaults to ",".
    -   Encoding format: encoding in the preceding parameter description, which defaults to utf-8.
    -   null Value: nullFormat in the preceding parameter description, which is used to define a string that represents the null value.
    -   Compression Format: compress in the preceding parameter description, which defaults to "no compression".
    -   Whether to Include the Table Header: \*\*skipHeader\*\* in the preceding parameter description, which defaults to "No".
    -   Prefix Conflict: The writemode in the above parameter description defines a string that represents a null value.
2.  Field mapping: The column in the preceding parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add Line** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15411404327701_en-US.png)

    In-row mapping: You can click In-row Mapping to create a mapping for the same row. Note that the data type must be consistent.

3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15411404327704_en-US.png)

    Parameters:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure synchronization jobs written to the FTP database.

```
{
    "type":"job",
    "version":"2.0", //version number
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "ftp", // plug-in name
            "parameter":{
                "path":""//File path
                "fileName": "",//File name
                "nullFormat": "null", // Null Value
                "dateFormat":"yyyy-MM-dd HH:mm:ss", // time format
                "datasource": "", // Data Source
                "writeMode": "",//Write mode
                "fieldDelimiter": "," //Delimiter of each column
                "encoding": "UTF-8", // encoding format
                "fileFormat": "",//File type
            },
            "name": "Writer ",
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

