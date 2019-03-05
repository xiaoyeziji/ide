# Configure HybridDB for PostgreSQL Reader {#concept_es2_5zb_5fb .concept}

This topic describes the data types and parameters supported by HybridDB for PostgreSQL Reader and how to configure it in both wizard and script modes.

HybridDB for PostgreSQL Reader reads data from a HybridDB for PostgreSQL database. At the underlying implementation level, HybridDB for PostgreSQL Reader connects to a remote HybridDB for PostgreSQL database through JDBC and runs SELECT statements to extract data from the database. On the public cloud, RDS provides the HybridDB for PostgreSQL storage engine.

In short, HybridDB for PostgreSQL Reader connects to a remote HybridDB for PostgreSQL database through a JDBC connector, generates SELECT statements based on your configuration, and sends the statements to the remote database. Then, HybridDB for PostgreSQL Reader assembles SQL execution results into abstract datasets in custom data types of Data Integration, and passes the datasets to the downstream writer.

-   HybridDB for PostgreSQL Reader concatenates the configured table, column, and where information into SQL statements and sends the statements to the HybridDB for PostgreSQL database.
-   HybridDB for PostgreSQL Reader sends the configured querySql information directly to the HybridDB for PostgreSQL database.

## Type conversion list {#section_ny3_4fr_5fb .section}

HybridDB for PostgreSQL Reader supports most data types in HybridDB for PostgreSQL. Check whether a data type is supported before configuring HybridDB for PostgreSQL Reader.

HybridDB for PostgreSQL Reader converts the data types in HybridDB for PostgreSQL as follows:

|Type classification|HybridDB for PostgreSQL data type|
|:------------------|:--------------------------------|
|Integer|Bigint, Bigserial, Integer, Smallint, and Serial|
|Float|Double precision, Money, Numeric, and Real|
|String|Varchar, Char, Text, Bit, and Inet|
|Date and time|Date, Time, and Timestamp|
|Boolean|Boolean|
|Binary|Bytea|

## Parameter description {#section_vpb_wfr_5fb .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data sources is supported in script mode.|Yes|None|
|table|The name of the source table.|Yes|None|
|column|An array of columns to be synchronized from the configured table, in JSON format. The default value is \[ \* \], which indicates all columns.-   Column pruning is supported, which means that you can select and export specific columns.
-   Change of column order is supported, which means that you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. Constants must be configured by using the SQL syntax, for example, `["id", "table","1", "'mingya.wmy'", "'null'", "to_char(a+1)", "2.3" , "true"]`.
    -   id: A common column name.
    -   table: A column name that contains a reserved word.
    -   1: An integer constant.
    -   'mingya.wmy': A string constant enclosed in single quotation marks.
    -   null: A null pointer.
    -   CHAR\_LENGTH\(s\): A function used to calculate the string length.
    -   2.3: A floating-point number.
    -   true: A Boolean value.
-   The column attribute must explicitly specify a set of columns to be synchronized. It cannot be left blank.

|Yes|None|
|splitPk|The field used for data sharding when HybridDB for PostgreSQL Reader extracts data. If you specify splitPk, Data Integration initiates concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   We recommend that you set the splitPk attribute to the primary key of the table. Based on primary keys, data can be well distributed to different shards, but not intensively distributed to certain shards.
-   Currently, the splitPk attribute supports data sharding only for integers but not for other data types such as String, Float, and Date. If you specify an unsupported data type, Data Integration ignores the splitPk attribute and synchronizes data through a single task.
-   If you do not provide the splitPk attribute or leave it blank, Data Integration synchronizes the table data through a single task.

|No|None|
|where|The filter condition. HybridDB for PostgreSQL Reader concatenates the specified column, table, and where information into an SQL statement and uses the SQL statement to extract data. For example, you can set the where attribute to id\>2 and sex=1 during a test. In actual business scenarios, the data on the current day is usually synchronized.-   The where attribute can be used to synchronize incremental business data effectively.
-   If you do not provide the where attribute or leave it blank, the data of the entire table is synchronized.

|No|None|
|querySql \(an advanced attribute, which is not available in wizard mode\)|The custom filter SQL statement used in some business scenarios where the filter condition specified by the where attribute is insufficient. After this attribute is set, Data Integration ignores the table, column, and splitPk attributes, but directly filters data based on this attribute. For example, to synchronize data after joining multiple tables, set the querySql attribute to `select a,b from table_a join table_b on table_a.id = table_b.id`. \[DO NOT TRANSLATE\]|No|None|
|fetchSize|The number of data records that the plug-in can fetch from the database server each time. The value determines the frequency of interaction between Data Integration and the server on the network, and therefore can be used to greatly improve data extraction performance.**Note:** A value greater than 2048 may lead to OOM during the data synchronization process.

 |No|512|

## Development in wizard mode {#section_jvk_sgr_5fb .section}

1.  **Specify data sources.**

    Configure the source and destination of data for a synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175235732069_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**Data Source**|The datasource attribute in the preceding parameter description. Select the data source that you have configured.|
    |**Table**|The table attribute in the preceding parameter description. Select the source table.|
    |**Filter**|The filter condition for the data to be synchronized. Currently, filtering based on the limit keyword is not supported. SQL syntaxes vary with data sources.|
    |**Shard Key**|The shard key. You can use a column in the source table as the shard key. We recommend that you use the primary key or an indexed column. Only integer fields are supported. If data sharding is performed based on the configured shard key, data can be read concurrently to improve data synchronization efficiency.**Note:** The Shard Key parameter is displayed only when you configure the source of data for a synchronization task.

|

2.  Configure mappings of fields \(the column attribute in the preceding parameter description\).

    Each source table field on the left maps a destination table field on the right. You can click **Add** to add a mapping or move the cursor over a line and click **Delete** to delete the current mapping.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175235732070_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**Map Fields with the Same Name**|Click **Map Fields with the Same Name** to establish a mapping between fields with the same name. Note that the data type must be consistent.|
    |**Map Fields in the Same Line**|Click **Map Fields in the Same Line** to establish a mapping for the same row. Note that the data type must be consistent.|
    |**Remove Mappings**|Click **Remove Mappings** to remove mappings that have been established.|
    |**Auto Layout**|The fields are automatically sorted based on specified rules.|
    |**Change Fields in Source Table**|You can manually edit fields in the source table. Each field occupies a row. The first and the last blank rows are included, while other blank rows are ignored.|
    |**Add**|     -   You can enter constants. Each constant must be enclosed in single quotation marks, such as 'abc' and '123'.
    -   You can use scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Unidentified.
 |

3.  **Configure channel control**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175235739902_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**DMU**|The unit that measures the resources \(including CPU, memory, and network resources\) consumed by Data Integration. A DMU represents the minimum operating capability of a Data Integration task, that is, the data synchronization processing capability given limited CPU, memory, and network resources.|
    |**Concurrent Jobs**|The maximum number of threads used to concurrently read data from the source or write data into the data storage media in a data synchronization task. In wizard mode, you can configure the concurrency for a task on the wizard page.|
    |**Dirty Data Records Allowed**|The maximum number of errors or dirty data records allowed.|
    |**Task Resource Group**|The machines on which tasks are run. If a large number of tasks are run on the default resource group, some tasks may be delayed due to insufficient resources. In this case, we recommend that you add a custom resource group. Currently, a custom resource group can be added only in China \(Hangzhou\) and China \(Shanghai\). For more information, see [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).|


## Development in script mode {#section_xqz_qyr_5fb .section}

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {
                "datasource": "test_004",// The data source name.
                "column": [// The source table columns.
                    "id",
                    "name",
                    "sex",
                    "salary",
                    "age",
                ],
                "where": "id=1001",// The filter condition.
                "splitPk": "id",// The shard key.
                "table": "public.person"// The source table name.
            },
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {},
            "name": "Writer",
            "category": "writer"
        }
    ],
    "version": "2.0",// The version number.
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    },
    "setting": {
        "errorLimit": {// The maximum number of errors allowed.
            "record": ""
        },
        "speed": {
            "concurrent": 6,// The number of concurrent threads.
            "throttle": false,// Indicates whether to throttle the transmission rate.
            "dmu": 7// The DMU value.
        }
    }
}
```

