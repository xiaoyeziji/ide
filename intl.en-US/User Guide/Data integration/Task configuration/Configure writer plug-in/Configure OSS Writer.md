# Configure OSS Writer {#concept_fw1_2ln_q2b .concept}

This topic describes the data types and parameters supported by OSS Writer and how to configure Writer in both Wizard and Script mode.

OSS Writer allows you to write one or more table files in formats similar to CSV into OSS. 

**Note:** You must configure the data source before configuring the OSS Writer plug-in. For more information, see [Configure OSS data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure OSS data source.md#).

What is written and saved to the OSS file is a two-dimensional table in a logic sense. For example, the text information can be written in CSV format.

-   For more information about OSS products, see[OSS Product Overview](https://www.alibabacloud.com/help/doc-detail/31817.htm).

OSS Writer allows you to convert the data synchronization protocol to a text file in OSS, which is a non-structured data storage. Currently, OSS Writer supports the following features:

-   Only writing text files is supported. The schema in the text file must be a two‑dimensional table.
-   File formats similar to CSV with custom delimiters is supported.
-   Multi-thread writing with different subfiles written using different threads is supported.
-   File rollover is supported. A file exceeding a specific size value must be switched. A file that contains lines that exceed a specific number of lines must be switched.

Currently, OSS Writer does not support the following features:

-   Concurrent writing for a single file is not supported.
-   OSS does not provide data types, but OSS Writer writes String type data to OSS.

OSS does not provide data types, which are defined by DataX OSS Writer.

|Type classification|OSS data type|
|:------------------|:------------|
|Integer|Long|
|Float|Double|
|String|String|
|Boolean|bool|
|Date and time|Date|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. The name entered here must be the same name as the added data source. You can add a data source in Script Mode.|Yes|None|
|Object|The file name written by OSS Writer. The object enables the simulation of directories with file names in OSS. For example, if the data is synchronized into the OSS bucket is the test folder of test118, you only need to specify test for the object, and you do not have to specify the bucket name. The file name synchronized to the OSS end must be identical with the source end. If the value "object": "test/DI" is used to specify the object, the object written in OSS starts with test/DI. In this command, test is the folder, DI is the file name prefix \(the suffix is a random string\), and the forward slash \(/\) is the delimiter of the simulated OSS directory.

|Yes|None|
|writeMode|The write mode in which the OSS Writer clears existing data before writing data. -   truncate: Clears all objects with the specified Object prefix before writing. For example, all objects with the prefix abc will be cleared, if the specified object prefix is`"object":"abc"`
-   append: This parameter setting will not run any processes before writing data. Data Integration OSS Writer writes data directly with the object name, and appends a random UUID suffix name to ensure there are no conflicts in the file names. For example, if the object name you specified is Data Integration, the name entered is DI\_xxxxxx\_xxxx\_xxxx.
-   nonConflict: This parameter will report an error, if an object with the specified prefix exists in the specified path. For example, if the specified prefix is `"object":"abc"`, an error is reported when an object starts with abc123.

|Yes|None|
|fileFormat|The written file format, includes CSV and text. If the written data in CSV format contains column delimiters, the column delimiters are escaped into double quotation marks \("\) by the CSV escape syntax. For text format, the data written is separated by column delimiters without being escaped.|No|Text|
|fieldDelimiter|The delimiter for separating read fields.|No|,|
|encoding|Encodes written files.|No|UTF-8|
|nullFormat|You cannot define null \(null pointer\) with a standard string in text files. The Data Synchronization system provides nullFormat to define strings that can be expressed as null. For example,if `nullFormat="null"` is configured, and the source data is null, the Data Syncrhonization system will classify it as a null field.|No|None|
|header \(only available in advanced configuration\)|The header used when a file is written in OSS. For example: \['id', 'name', 'age'\].|No|None|
|maxFileSize \(only available in advanced configuration\)|By default, the maximum size of an object file written in OSS is 10,000 x 10 MB. This parameter is similar to log rotation based on the log size in log4j log printing. Each part of a multipart upload in OSS is 10 MB in size, which is the minimum file granularity for log rotation. For maxFileSize with a size smaller than 10 MB are classified as 10 MB, and the maximum number of parts supported for each OSS InitiateMultipartUploadRequest is 10,000.When rotation occurs, the object naming rule is the original object prefix + a random UUID + a suffix, such as \_1, \_2, \_3.

|No|100,000 MB|

## Development in Wizard Mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    The following is a configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15514335237815_en-US.png)

    Parameters:

    -   Data source: The datasource from the parameter description section. Enter the configured data source name.
    -   Object prefix: The object from the parameter description section. Enter a path to the OSS folder without the bucket name.
    -   Column delimiter: The fieldDelimiter in the preceding parameter description section. By default, the delimiter are commas \(,\).
    -   Encoding format: The encoding in the preceding parameter description section. By default, the encoding format is UTF-8.
    -   null value: The nullFormat in the preceding parameter description section. Defines a string that represents the null value.
2.  Field mapping: The column in the preceding parameter description section.

    The source table field on the left and the target table field on the right have a one-to-one relationship. To add a single filed, click **Add Row**. To delete the current field, click **Delete**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15514335237818_en-US.png)

    In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.

3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15514335237675_en-US.png)

    Parameters:

    -   DMU: A unit that measures resources consumed during data integration, including CPU, memory, and network bandwidth. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: The maximum number of threads used to concurrently read/write data into the data storage media in a data synchronization task. You can configure a concurrency for the specified task under Wizard Mode.
    -   The maximum number of errors, which means the maximum number of dirty data records.
    -   Task resource group: The machine on which the task runs. If the number of tasks is large, the default Resource Group is used to wait for a resource. We recommend that you add a Custom Resource Group \(currently only East China 1 and East China 2 supports adding custom resource groups\). For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in Script Mode {#section_cp2_wsh_p2b .section}

The following is an example of script configuration. For details about the parameters, see the preceding Parameter Description section.

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

