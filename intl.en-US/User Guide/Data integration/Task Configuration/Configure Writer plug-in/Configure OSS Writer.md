# Configure OSS Writer {#concept_fw1_2ln_q2b .concept}

In this article we will show you the data types and parameters supported by OSS Writer and how to configure Writer in both wizard mode and script mode.

OSS Writer provides the ability to write one or more table files in CSV-like format into OSS. 

**Note:** You must configure the data source before configuring the OSS Writer plug-in. For more information, see [Configure OSS data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure OSS data source.md#)Configure the OSS data source.

What is written and saved to the OSS file is a two-dimensional table in a logic sense, for example, text information in a CSV format.

-   If you want to learn more about OSS products, see the [OSS Product Overview](https://www.alibabacloud.com/help/doc-detail/31817.htm).

OSS Writer provides the ability to convert the data synchronization protocol to a text file in OSS, which itself is a non-structured data storage. Currently, OSS Writer supports the following features:

-   Only supports writing text files and the shema in the text file must be a two‑dimensional table.
-   Supports CSV‑like format files with custom delimiters.
-   Supports multi-thread writing, with different subfiles written using different threads.
-   Supports file rollover. A file exceeding a specific size value must be switched. A file that contains lines exceeding a specific number of lines must be switched.

OSS Writer does not support the following features temporarily:

-   Concurrent writing is not supported for a single file.
-   OSS itself does not provide data types. OSS Writer writes data of the String type to OSS.

OSS itself does not provide data types, which are defined by DataX OSS Writer.

|Type Classification|OSS data type|
|:------------------|:------------|
|Integer|Long|
|Float|Double|
|String|String|
|Boolean|bool|
|Date and time|Date|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|Description: Data source name. The name entered here must be same to the name of the added data source. You can add a data source in script mode.|Yes|None|
|Object|Description: The file name written by OSS Writer. It enables the simulation of directories with file names in OSS. If the bucket in the OSS data source for data synchronization is the test folder of test118, as shown in the following figure:![](images/8196_en-US.png)

only test needs to be specified for object, without the bucket name. The file name synchronized to the OSS end is identical to the one entered in the source end.

If "object": "test/DI" is specified, the object written in OSS begins with test/DI,  in which test is a folder, DI is the prefix of the file name \(suffix is a random string\), and a forward slash \(/\) is used as the delimiter of the simulated OSS directory.

|Yes|None|
|writeMode|Description: The mode in which OSS Writer clears the existing data before writing data. -   truncate: All objects with matched object name prefixes are cleared before writing.  For example, if`"object":"abc"` is specified, all objects beginning with abc are cleared.
-   append: No processing is done before writing. Data Integration OSS Writer writes data directly using the object name, and appends a random UUID suffix name to ensure no conflict of file names.  For example, if the object name you specified is Data Integration, the name is actually entered as DI\_xxxxxx\_xxxx\_xxxx.
-   nonConflict: If an object with matched prefix exists in a specified path, an error is reported directly.  For example, if `"object":"abc"`, is specified, when an object beginning with abc123 exists, an error is reported directly.

|Yes|None|
|fileFormat|The format written by the file, including both CSV and text. Description: The format in which a file is written. Supported formats are CSV and text. If the data to be written contains column delimiters, the column delimiters are escaped to double quotation marks \("\) in CSV escape syntax.  For text format, the data to be written is separated by column delimiters without being escaped.|No|text|
|fieldDelimiter|Description: The delimiter used to separate the read fields.|No|,|
|encoding|Description: Encoding of the written files.|No|UTF-8|
|nullFormat|Description: Defining null \(null pointer\) with a standard string is not allowed in text files. Data Synchronization system provides nullFormat to define which strings can be expressed as null. For example, when `nullFormat="null"` is configured, if the source data is null, it is considered as a null field in Data Synchronization.|No|None|
|header \(advanced configuration, which is not supported in wizard mode\)|Description: Header used when a file is written in OSS. For example, \['id', 'name', 'age'\].|No|None|
|maxFileSize \(advanced configuration, which is not supported in wizard mode\)|Description: The maximum size of a single object file written in OSS, which defaults to 10,000 x 10 MB. It is similar to the log rotation based on the log size in log4j log printing. For multipart upload in OSS, the size of each part is 10 MB \(which is the minimum file granularity for log rotation, and maxFileSize smaller than 10 MB is also taken as 10 MB\), and the maximum number of parts supported for each OSS InitiateMultipartUploadRequest is 10,000.When rotation occurs, the naming rule for object is the original object prefix + a random UUID + a suffix such as \_1, \_2, \_3.

|No|1,000 MB|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15413890817815_en-US.png)

    Parameters:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Object prefix: Object in the preceding parameter description.  Enter a path to the OSS folder without the bucket name.
    -   Column delimiter: fieldDelimiter in the preceding parameter description, which defaults to ",".
    -   Encoding format: encoding in the preceding parameter description, which defaults to utf-8.
    -   null value: nullFormat in the preceding parameter description, to define a string that represents the null value.
2.  Field mapping: The column in the preceding parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15413890817818_en-US.png)

    In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.

3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15413890817675_en-US.png)

    Parameters:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

The following is a script configuration sample. For details about parameters, see the preceding Parameter Description.

```
{
    "type": "job",
    "version": "2.0",
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "oss", // plug-in name
            "parameter":{
                "nullFormat":"", // The data synchronization system provides a nullformat to define which strings can be expressed as null.
                "dateFormat": "", // Date Format
                "datasource": "", // Data Source
                "writeMode": "",//Write mode
                "encoding": "UTF-8", // encoding format
                "fieldDelimiter":",",//Separator
                "fileFormat": "",//File type
                "object": "// object prefix
            },
            "name": "Writer ",
            "category":"writer"
        }
    ],
    "setting ":{
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1//DMU Value
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

