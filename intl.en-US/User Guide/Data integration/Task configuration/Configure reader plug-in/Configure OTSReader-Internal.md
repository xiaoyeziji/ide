# Configure OTSReader-Internal {#concept_cpg_nzq_p2b .concept}

This topic describes the data types and parameters supported by OTSReader-Internal and how to configure Reader in script mode.

Table Store \(originally known as OTS\) is a NoSQL database service built upon Alibaba Cloud's Apsara distributed system, enabling you to store and access massive structured data in real-time. Table Store organizes data into instances and tables. Using data partition and Server Load Balancing \(SLB\) technology, it provides seamless scaling.

OTSReader-Internal is used to export table data for Table Store Internal model, while OTS Reader is used to export data for OTS Public model.

Table Store Internal model supports multi-version columns, so OTSReader-Internal also provides two data export modes:

-   Multi-version mode: A version mode that exports data in multiple versions. Table Store supports multiple versions.

    Export solution: The Reader plug-in expands a cell of Table Store into a one-dimensional table consisting of four tuples: PrimaryKey \(column 1-4\), ColumnName, Timestamp, and Value \(the principle is similar to the multi-version mode of HBase Reader\). The four tuples are passed in to the Writer as four columns in Datax record.

-   Normal mode: Consistent with the normal mode of the hbase reader, simply export the latest version of each column in each row of data. For more information, see [Configure HBase reader](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure HBase reader.md#)the normal mode content that is supported by the hbase reader in.

In short, OTS Reader connects to Table Store's server and reads data through Table Store official Java SDK. OTS Reader optimizes the read process using features, such as read timeout retry and exceptional read retry.

Currently, OTS Reader supports all Table Store types. The conversion of Table Store types in the OTSReader-Internal is as follows:

|Data integration internal types|Table Store data model|
|:------------------------------|:---------------------|
|Long|Integer|
|Double|Double|
|String|String|
|Boolean|Boolean |
|Bytes|Binary|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|mode|The plug-in operation mode, supporting normal and multiVersion, which refers to normal mode and multi-version mode respectively.|Yes|N/A|
|endpoint|The EndPoint of Table Store Server.|Yes|N/A|
|accessId|The access ID for Table Store.|Yes|N/A|
|accessKey|The access key for Table Store.|Yes|N/A|
|Instance name|The Table Store instance name . The instance is an entity for using and managing Table Store service.After you enable the Table Store service, you can create an instance in the Console to create and manage tables. The instance is the basic unit for Table Store resource management. All access control and resource measurement performed by the Table Store for applications are completed at the instance level.

|Yes|N/A|
|table|The name of the table to be extracted. Only one table can be filled in. Multi-table synchronization is not required for Table Store.|Yes|N/A|
|Range|The export range: \[begin,end\).-   When begin is less than end, reads data in positive sequence.
-   When begin is greater than end, reads data in inverted sequence.
-   Begin and end cannot be equal.
-   The following types are supported: string, int, and binary. The binary data is passed in as Base64 strings in binary format. INF\_MIN represents an infinitely small value and INF\_MAX represents an infinitely large value.

|No|Reads from the beginning of the table to the end of the table|
|range: \{"begin "\}|The starting range that is exported. The value can be an empty array, a PK prefix, or a complete PK. When reading the data in positive order, the default fill PK suffix is inf\_min, and the reverse order is inf\_max, as shown in the example below.If your table has two PrimaryKeys in the type of string and int, the table data can be entered using the following three methods:

-   \[\] Indicates that it is read from the beginning of the table.
-   \[\{"type": "string", "value ": "a"\}\] means from \[\{"type": "string ", "value": "a"\}, \{"type": "INF\_MIN"\}\].
-   \[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\]

PrimaryKey column in binary type is special. JSON does not support directly passing in binary data, so the following rules are defined: To pass in binary data, you must use \(Java\) Base64.encodeBase64String method to convert binary data into a visualized string and then enter the string in value. The example is as follows \(Java\):

-   `byte[] bytes = "hello".getBytes();` :Create binary data. Here the byte value of string hello is used.
-   `String inputValue = Base64.encodeBase64String(bytes)`: Calls Base64 method to convert binary data into visualized strings.

Run the preceding code, and then the inputValue of "aGVsbG8=" can be obtained.

Finally, write the value into the configuration: \{"type":"binary","value" : "aGVsbG8="\}.

|No|Read data from the beginning of the table|
|range: \{"end "\}|The end range that is exported. The value can be an empty array, a PK prefix, or a complete PK. When reading data in positive order, the default population PK suffix is INF\_MAX, and the reverse order is INF\_MIN.If your table has two PKs in the type of string and int, the table data can be entered using the following three methods:

-   \[\] Indicates that it is read from the beginning of the table.
-   \[\{“type”:”string”, “value”:”a”\}\] means from \[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\].
-   \[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\].

PrimaryKey column in binary type is special. JSON does not support directly passing in binary data, so the following rules are defined: To pass in binary data, you must use \(Java\) Base64.encodeBase64String method to convert binary data into a visualized string and then enter the string in value. The example is as follows \(Java\):-   `byte[] bytes = "hello".getBytes();`: Create binary data. Here the byte value of string hello is used.
-   `String inputValue = Base64.encodeBase64String(bytes)`: Call Base64 method to convert binary data into visualized strings.

Run the preceding code, and then the inputValue of "aGVsbG8=" can be obtained.

Finally, write the value into the configuration: \{"type":"binary","value" : "aGVsbG8="\}.

|No|Read to end of table|
|range: \{"split "\}|If too much data needs to be exported, you can enable concurrent export. Split can split the data in the current range into multiple concurrent tasks according to split points.**Note:** 

-   The value entered in the split must be in the first column of PrimaryKey \(partition key\) and the value type must be consistent with that of the PartitionKey.
-   The values range must be between begin and end.
-   The value within the split must increase or decrease progressively depending on the positive and inverted relationship between begin and end.

|No|Empty cut point|
|column|Specifies the columns to export, supporting common and constant columns.Format \(multi-version mode is supported\)

Regular column format: \{"name":"\{your column name\}"\}

| | |
|timeRange \(only multi-version mode is supported\)|The time range of the request data. The read range is \[begin,end\).**Note:** Begin must be smaller than end.

|No|Read all versions by default|
|timeRange:\{"begin"\} \(only multi-version mode is supported\)|The start time of the time range of request data. The value range is 0-LONG\_MAX.|No|10 by default|
|timeRange:\{"end"\} \(only multi-version mode is supported\)|The end time of the time range of request data. The value range is 0-LONG\_MAX.|No|- Default value: Long Max\(9223372036854775806L\)|
|maxVersion \(only multi-version mode is supported\)|The request specified version. The value range is 1-INT32\_MAX.|No|Reads all versions by default|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_cp2_wsh_p2b .section}

Multi-version mode

```
{
    "type": "job",
    "version": "1.0",
    "configuration": {
        "reader": {
            "plugin": "otsreader-internalreader ",
            "parameter": {
                "mode": "multiversion ",
                "endpoint": "",
                "accessId": "",
                "accessKey": "",
                "instanceName":"",
                "table":"",
                "range":{
                    "begin":[
                        {
                            "type": "string",
                            "value": "a"
                        },
                        {
                            "type": "INF_MIN"
                        }
                    ],
                    "end":[
                        {
                            "type": "string",
                            "value": "g"
                        },
                        {
                            "type": "INF_MAX"
                        }
                    ],
                    "split":[
                        {
                            "type": "string",
                             "value": "b"
                        },
                        {
                            "type": "string",
                            "value": "c"
                        }
                    ]
                },
                "column": [
                    {
                        "name": "attr1"
                    }
                ],
                "timeRange": {
                    "begin": 1400000000,
                    "end": 1600000000
                },
                "maxVersion": 10
            }
        }
    },
    "writer": {
}
```

Normal mode

```
{
    "type": "job",
    "version": "1.0",
    "configuration": {
        "reader": {
            "plugin": "otsreader-internalreader ",
            "parameter": {
                "mode": "normal",
                "endpoint": "",
                "accessId": "",
                "accessKey": "",
                "instanceName":"",
                "table":"",
                "range":{
                    "begin":[
                        {
                            "type": "string",
                            "value": "a"
                        },
                        {
                            "type": "INF_MIN"
                        }
                    ],
                    "end":[
                        {
                            "type": "string",
                             "value": "g"
                        },
                        {
                            "type": "INF_MAX"
                        }
                    ],
                    "split":[
                        {
                            "type": "string",
                             "value": "b"
                        },
                        {
                            "type": "string",
                             "value": "c"
                        }
                    ]
                },
                "column": [
                    {
                        "name": "pk1"
                    },
                    {
                        "name": "pk2"
                    },
                    {
                         "name": "attr1"
                    },
                    {
                        "type": "string",
                        "value":""
                    },
                    {
                        "type": "int",
                        "value": ""
                    },
                    {
                        "type": "double",
                        "value":""
                    },
                    {
                        "type": "binary",
                         "value": "aGVsbG8="
                    }
                ]
            }
        }
    },
     "writer": {}
}
```

