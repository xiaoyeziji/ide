# Configure Table Store \(OTS\) Writer {#concept_sdt_fb4_q2b .concept}

This topic describes the data types and parameters supported by Table Store \(OTS\) Writer and how to configure Writer in both Wizard and Script mode.

Table Store \(formerly known as OTS\) is a NoSQL database service built-on Alibaba Cloud ApsaraDB Distributed Operating System that allows storage and real-time access of massive structured data. Table Store organizes data into instances and tables. Table Store provides seamless scaling by using data partition and Server Load Balancing \(SLB\) technology.

In short, the Table Store Writer-Internal connects to the Table Store server through the official Table Store Java Software Development Kits \(SDKs\), and writes data in the Table Store server through the SDKs. The Table Store Writer has optimized the writing process to include retry upon writing timeout, retry upon writing exception, batch submissions, and other features.

Currently, the Table Store Writer-Internal supports all types of Table Store data and converts data types for Table Store as follows:

-   PutRow: The PutRow for Table Store API, which is used to insert data in a specified row. If this row does not exist, a new row is added. Otherwise, the original row is overwritten.
-   UpdateRow: The UpdateRow for Table Store API, which is used to update the data of a specified row. If the row does not exist, a new row is added. Otherwise, the specified column values are added, modified, or deleted based on the request.

Currently, Table Store Writer supports all Table Store data types and converts the Table Store data types as follows:

|Type classification|Table store data type|
|:------------------|:--------------------|
|Integer|Integer|
|Float|Double|
|String|String|
|Boolean|Boolean|
|Binary|Binary|

**Note:** 

The Integer category must be configured to Int in Script Mode for it to be converted to Integer type in Table Store. You cannot configure the Integer type in Table Store, an error will occur in the log and lead to task failure.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Parameter|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. The name must be identical to the added data source name. Script Mode supports adding data source.|Yes|None |
|endPoint|The table store server endpoint. For more information, see Access control.|Yes|None |
|accessId|The AccessId of a Table Store instance.|Yes|None |
|accessKey|The AccessKey required for accessing Table Store service.|Yes|None |
|instanceName|The name of the Table Store instance.An instance is an object for using and managing the Table Store. After Table Store is activated, you need to create an instance through the console, and then create and manage tables in the instance. An instance is the basic unit for Table Store resource management. The Table Store controls access to applications and measures resources on an instance-level.

|Yes|None |
|table|Selects the table name for extraction. You can enter only one table name. Multi-table synchronization is not required for Table Store.|Yes|None |

-   primaryKey
    -   Primary key information of the Table Store. The field information is described with JSON arrays. The Table Store is a NoSQL system, so the corresponding field name must be specified when the Table Store Writer imports data.
    -   Required: Yes.
    -   The PrimaryKey of Table Store only supports String and Int data types, therefore only these two data types can be entered in Table Store Writer.

Data synchronization system supports data type conversions, so Table Store Writer can convert the non‑String and non-Int data source. Configuration example:

```
"primaryKey" : [
    {"name":"pk1", "type":"string"},
    
                    ],
```

-   column
    -   Description: The column name set for synchronization in the configured table. The field information is described with JSON arrays.
    -   Required: Yes.
    -   By default, this field is not specified.

The format is as follows:

```
{"name":"col2", "type":"INT"},
```

The parameter "name" specifies the Table Store column name to be written, and "type" specifies the data type to be written. The data types supported by Table Store, include String, Int, Double, Bool, and Binary.

Constants, functions, or custom statements are not supported during writing.

-   writeMode
    -   Description: The write mode. The following three modes are supported:
-   Single row operation

```
GetRow: Read data from a single row.
PutRow: PutRow for Table Store API, which is used to insert data to a specified row. If this row does not exist, a new row is added. Otherwise, the original row is overwritten.
UpdateRow: UpdateRow for Table Store API, which is used to update the data of a specified row. If the row does not exist, a new row is added. Otherwise, the values of the specified columns are added, modified, or deleted as request.
DeleteRow: Delete a row.
```

-   Batch operation

```
BatchGetRow: Read data from multiple rows.
```

-   Read range

    ```
    GetRange: Read table data within a certain range.
    ```

    -   Required: Yes
    -   By default, this field is not specified.

## Development in Wizard Mode {#section_pjc_bh5_q2b .section}

Currently, development in Wizard Mode is not supported.

## Development in Script Mode {#section_bp2_wsh_p2b .section}

Configure a job to write data to Table Store as follows:

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
            "stepType": "ots", //plug-in name
            "parameter":{
                "datasource": "", // Data Source
                "column": [// Field
                    {
                        "name": "columnname1", // field name
                        "type": "INT" // data type
                    },
                    {
                        "name": "columnname2 ",
                        "type": "STRING"
                    },
                    {
                        "name": "columnname3 ",
                        "type": "double"
                    },
                    {
                        "name": "columnname4 ",
                        "type": "BOOLEAN"
                    },
                    {
                        "name": "columnname5 ",
                        "type": "BINARY"
                    }
                ],
                "writeMode": "insert",//Write mode
                "table": ", // table name
                "primaryKey": primary key information for [// table store
                    {
                        "name":"pk1",
                        "type":"STRING"
                    },
                    {
                        "name":"pk2",
                        "type":"INT"
                    }
                ]
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
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
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

