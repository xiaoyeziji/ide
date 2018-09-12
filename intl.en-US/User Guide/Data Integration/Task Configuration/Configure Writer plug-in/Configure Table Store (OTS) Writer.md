# Configure Table Store \(OTS\) Writer {#concept_sdt_fb4_q2b .concept}

Table Store \(originally known as OTS\) is a NoSQL database service built on the Alibaba Cloud Apsara distributed system, allowing storage of and real-time access to massive structured data.  Table Store organizes data into instances and tables. Using data partition and server load balancing technology, it provides seamless scaling.

In short, Table Store Writer-Internal connects to the Table Store server through official Table Store Java SDKs and writes data in the Table Store server through SDKs.  Table Store Writer has greatly optimized the write process, including retry upon write timeout, retry upon exception in writing, batch submission, and other features.

Currently, Table Store Writer-Internal supports all types of Table Store data and converts data types for Table Store as follows:

-   PutRow: PutRow for Table Store API, which is used to insert data to a specified row. If this row does not exist, a new row is added. Otherwise, the original row is overwritten.
-   UpdateRow: UpdateRow for Table Store API, which is used to update the data of a specified row. If the row does not exist, a new row is added. Otherwise, the values of the specified columns are added, modified, or deleted as request.

Currently, Table Store Writer supports all Table Store data types and converts the data types in Table Store as follows:

|Type Classification|Table store data type|
|:------------------|:--------------------|
|Integer|Integer|
|Float|Double|
|String|String|
|Boolean|Boolean|
|Binary|Binary|

**Note:** 

You must configure the Integer category to Int in script mode so that it can be converted to the Integer type for Table Store. If you directly configure it to the Integer type for Table Store, an error is reported in the log and causes task failure.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|None |
|endPoint|The endpoint for the table store server, see access control for details.|Yes|None |
|accessId|Description: accessId of a Table Store instance |Yes|None |
|accessKey|AccessKey required for accessing Table Store service|Yes|None |
|instanceName|Description: Name of the Table Store instanceAn instance is an object for using and managing Table Store. After activating Table Store, you need to create an instance through the console, and then create and manage tables in the instance. An instance is a basic unit for Table Store resource management. Table Store controls access to applications and measures resources on an instance basis. 

|Yes|None |
|table|Description: The name of the table to be extracted. You can enter only one table name.  Multi-table synchronization is not required for Table Store.|Yes|None |

-   primaryKey
    -   Primary key information of Table Store. Field information is described using JSON array. Table Store itself is a NoSQL system, so the corresponding field name must be specified when Table Store Writer imports data.
    -   Required: Yes.
    -   PrimaryKey of Table Store only supports STRING and INT types, so only these two types can be entered for Table Store Writer.

Data synchronization system supports data type conversion, so Table Store Writer can convert the non‑String and non-Int source data. Configuration example:

```
"primaryKey" : [
    {"name":"pk1", "type":"string"},
    
                    ],
```

-   column
    -   Description: The column name set to be synchronized in the configured table. Field information is described with arrays in JSON.
    -   Required: Yes.
    -   By default, this field is not specified.

The format is as follows:

```
{"name":"col2", "type":"INT"},
```

"name" specifies the name of Table Store's column to be written, and "type" specifies the type of data to be written. Data types supported by Table Store include STRING, INT, DOUBLE, BOOL, and BINARY.

Constants, functions, or custom statements are not supported during write process.

-   writeMode
    -   Description: Write mode. The following three modes are supported:
-   Single row operation

```
GetRow: Read data from a single row.
PutRow: PutRow for Table Store API, which is used to insert data to a specified row. If this row does not exist, a new row is added. Otherwise, the original row is overwritten.
UpdateRow: UpdateRow for Table Store API, which is used to update the data of a specified row. If the row does not exist, a new row is added. Otherwise, the values of the specified columns are added, modified, or deleted as request.
DeleteRow: Delete a row.
```

-   Batch Operation

```
BatchGetRow: Read data from multiple rows.
```

-   Read range

    ```
    GetRange: Read table data within a certain range.
    ```

    -   Required: Yes
    -   By default, this field is not specified.

## Development in wizard mode {#section_pjc_bh5_q2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_bp2_wsh_p2b .section}

Configure a job to write data to Table Store:

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

