# Configure POLARDB Reader {#concept_ud2_vzb_5fb .concept}

This topic describes the data types and parameters supported by POLARDB Reader and how to configure it in both the wizard and script modes.

POLARDB Reader connects to a remote POLARDB database through a JDBC connector, generates SELECT statements based on your configuration, and sends the statements to the remote database. Then, POLARDB Reader assembles SQL execution results into abstract datasets in custom data types of Data Integration, and passes the datasets to the downstream writer.

In short, POLARDB Reader connects to a remote POLARDB database through a JDBC connector and runs SELECT statements to extract data from the remote database at the underlying layer. POLARDB Reader can read tables and views. For table fields, you can specify all or some of the columns in sequence, adjust the column order, specify constant fields, and configure POLARDB functions, such as now\(\).

## Type conversion list {#section_ny3_4fr_5fb .section}

POLARDB Reader converts the data types in POLARDB as follows:

|Type classification|POLARDB data type|
|:------------------|:----------------|
|Integer|Int, Tinyint, Smallint, Mediumint, and Bigint|
|Float|Float, Double, and Decimal|
|String|Varchar, Char, Tinytext, Text, Mediumtext, and Longtext|
|Date and time|Date, Datetime, Timestamp, Time, and Year|
|Boolean|Bit and Boolean|
|Binary|Tinyblob, Mediumblob, Blob, Longblob, and Varbinary|

**Note:** 

-   Except the preceding field types, other types are not supported.
-   POLARDB Reader classifies tinyint\(1\) as the integer type.

## Parameter description {#section_vpb_wfr_5fb .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data sources is supported in script mode.|Yes|None|
|table|The name of the source table. A Data Integration job can synchronize only one table.|Yes|None|
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
|splitPk|The field used for data sharding when POLARDB Reader extracts data. If you specify splitPk, Data Integration initiates concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   We recommend that you set the splitPk attribute to the primary key of the table. Based on primary keys, data can be well distributed to different shards, but not intensively distributed to certain shards.
-   Currently, the splitPk attribute supports data sharding only for integers but not for other data types such as String, Float, and Date. If you specify an unsupported data type, Data Integration ignores the splitPk attribute and synchronizes data through a single task.
-   If you do not provide the splitPk attribute or leave it blank, Data Integration synchronizes the table data through a single task.

|No|None|
|where|The filter condition. In actual business scenarios, the data on the current day is usually synchronized. In this case, you can set the where attribute to gmt\_create\>$bizdate.-   The where attribute can be used to synchronize incremental business data effectively. If the where attribute is not specified \(for example, the key or value of the where attribute is not provided\), full synchronization is performed.
-   You cannot set the where attribute to limit 10, which does not conform to the constraints on the SQL WHERE clause.

|No|None|
|querySql \(an advanced attribute, which is not available in wizard mode\)|The custom filter SQL statement used in some business scenarios where the filter condition specified by the where attribute is insufficient. After this attribute is set, Data Integration ignores the table, column, and splitPk attributes, but directly filters data based on this attribute. For example, to synchronize data after joining multiple tables, set the querySql attribute to `select a,b from table_a join table_b on table_a.id = table_b.id`. The priority of querySql is higher than those of table, column, where, and splitPk. When querySql is set, POLARDB Reader directly ignores the configuration of the table, column, where, or splitPk attribute. The data source uses querySql to parse out information such as the username and password.|No|None|
|singleOrMulti \(applicable only to database and table sharding\)|Indicates whether to perform database and table sharding. When you switch from the wizard mode to script mode, the following configuration is automatically generated: `"singleOrMulti": "multi"`. However, the task script configuration template does not automatically generate this configuration. You need to add it manually. Otherwise, only the first data source is recognized. The singleOrMulti attribute is used only at the front end. The back end does not use this attribute to determine whether to perform database and table sharding.|Yes|multi|

## Development in wizard mode {#section_jvk_sgr_5fb .section}

1.  **Specify data sources.**

    Configure the source and destination of data for a synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62197/155177228032058_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**Data Source**|The datasource attribute in the preceding parameter description. Select the data source that you have configured.|
    |**Table**|The table attribute in the preceding parameter description. Select the source table.|
    |**Statements Run Before Import**|The preSql attribute in the preceding parameter description. Enter the SQL statement that is run before the data synchronization task is run.|
    |**Statements Run After Import**|The postSql attribute in the preceding parameter description. Enter the SQL statement that is run after the data synchronization task is run.|
    |**Primary Key Violation**|The writeMode attribute in the preceding parameter description. Select the expected write mode.|

2.  Configure mappings of fields \(the column attribute in the preceding parameter description\).

    Each source table field on the left maps a destination table field on the right. You can click **Add** to add a mapping or move the cursor over a line and click **Delete** to delete the current mapping.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/155177228032015_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**Map Fields with the Same Name**|Click **Map Fields with the Same Name** to establish a mapping between fields with the same name. Note that the data type must be consistent.|
    |**Map Fields in the Same Line**|Click **Map Fields in the Same Line** to establish a mapping for the same row. Note that the data type must be consistent.|
    |**Remove Mappings**|Click **Remove Mappings** to remove mappings that have been established.|
    |**Auto Layout**|The fields are automatically sorted based on specified rules.|

3.  **Configure channel control**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/155177228032018_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**DMU**|The unit that measures the resources \(including CPU, memory, and network resources\) consumed by Data Integration. A DMU represents the minimum operating capability of a Data Integration task, that is, the data synchronization processing capability given limited CPU, memory, and network resources.|
    |**Concurrent Jobs**|The maximum number of threads used to concurrently read data from the source or write data into the data storage media in a data synchronization task. In wizard mode, you can configure the concurrency for a task on the wizard page.|
    |**Dirty Data Records Allowed**|The maximum number of errors or dirty data records allowed.|
    |**Task Resource Group**|The machines on which tasks are run. If a large number of tasks are run on the default resource group, some tasks may be delayed due to insufficient resources. In this case, we recommend that you add a custom resource group. Currently, a custom resource group can be added only in China \(Hangzhou\) and China \(Shanghai\). For more information, see [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).|


## Development in script mode {#section_vw5_p3r_5fb .section}

The following code is an example of configuration for a single table in one database. For more information about attributes, see the preceding parameter description.

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {
                "datasource": "test_005",// The data source name.
                "column": [// The source table columns.
                    "id",
                    "name",
                    "age",
                    "sex",
                    "salary",
                    "interest"
                ],
                "where": "id=1001",// The filter condition.
                "splitPk": "id",// The shard key.
                "table": "polardb_person"// The source table name.
            },
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {}
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
            "dmu": 6// The DMU value.
        }
    }
}
```

