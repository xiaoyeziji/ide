# Configure HybridDB for MySQL Reader {#concept_bd5_szb_5fb .concept}

HybridDB for MySQL Reader can read tables and views. For table fields, you can specify all columns in sequence, specify certain columns, adjust the column order, specify constant fields, and configure HybridDB for MySQL functions such as now\(\).

HybridDB for MySQL Reader connects to a remote HybridDB for MySQL database through a JDBC connector, generates SELECT SQL statements based on your configuration, and sends the statements to the remote database. Then, HybridDB for MySQL Reader assembles SQL execution results into abstract datasets in custom data types of Data Integration, and passes the datasets to the downstream writer. At the same time, HybridDB for MySQL Reader runs a SELECT statement to read data from the HybridDB for MySQL database.

## Type conversion list {#section_s4y_fls_5fb .section}

HybridDB for MySQL Reader converts the data types in HybridDB for MySQL as follows:

|Type classification|HybridDB for MySQL data type|
|:------------------|:---------------------------|
|Integer|Int, Tinyint, Smallint, Mediumint, and Bigint|
|Float|Float, Double, and Decimal|
|String|Varchar, Char, Tinytext, Text, Mediumtext, and Longtext|
|Date and time|Date, Datetime, Timestamp, Time, and Year|
|Boolean|Bit and Boolean|
|Binary|Tinyblob, Mediumblob, Blob, Longblob, and Varbinary|

## Parameter description {#section_v33_lls_5fb .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|-------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data sources is supported in script mode.|Yes|None|
|table|The name of the source table. A Data Integration job can synchronize only one table.|Yes|None|
|column|An array of columns to be synchronized from the configured table, in JSON format. The default value is \[ \* \], which indicates all columns.-   Column pruning is supported, which means that you can select and export specific columns.
-   Change of column order is supported, which means that you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. Constants must be configured by using the SQL syntax, for example,

    ```
["id", "table","1", "'mingya.wmy'", "'null'",
                      "to_char(a+1)", "2.3" , "true"].
    ```

\[DO NOT TRANSLATE\]

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
|splitPk|The field used for data sharding when HybridDB for MySQL Reader extracts data. If you specify splitPk, Data Integration initiates concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   We recommend that you set the splitPk attribute to the primary key of the table. Based on primary keys, data can be well distributed to different shards, but not intensively distributed to certain shards.
-   Currently, the splitPk attribute supports data sharding only for integers but not for other data types such as String, Float, and Date. If you specify an unsupported data type, Data Integration ignores the splitPk attribute and synchronizes data through a single task.
-   If you do not provide the splitPk attribute or leave it blank, Data Integration synchronizes the table data through a single task.

|No|None|
|where|The filter condition. In actual business scenarios, the data on the current day is usually synchronized. In this case, you can set the where attribute to gmt\_create\>$bizdate.-   The where attribute can be used to synchronize incremental business data effectively. If the where attribute is not specified \(for example, the key or value of the where attribute is not provided\), full synchronization is performed.
-   You cannot set the where attribute to limit 10, which does not conform to the constraints of HybridDB for MySQL on the SQL WHERE clause.

|No|None|
|querySql \(an advanced attribute, which is not available in wizard mode\)|The custom filter SQL statement used in some business scenarios where the filter condition specified by the where attribute is insufficient. After this attribute is set, Data Integration ignores the table, column, and splitPk attributes, but directly filters data based on this attribute. For example, to synchronize data after joining multiple tables, set the querySql attribute to```
select a,b from table_a join table_b on table_a.id =
                table_b.id.
```

\[DO NOT TRANSLATE\] The priority of querySql is higher than those of table, column, where, and splitPk. When querySql is set, HybridDB for MySQL Reader directly ignores the configuration of the table, column, where, or splitPk attribute. The data source uses querySql to parse out information such as the username and password.|No|None|
|singleOrMulti \(applicable only to database and table sharding\)|Indicates whether to perform database and table sharding. When you switch from the wizard mode to script mode, the following configuration is automatically generated:```
"singleOrMulti":
                "multi"
```

However, the task script configuration template does not automatically generate this configuration. You need to add it manually. Otherwise, only the first data source is recognized. The singleOrMulti attribute is used only at the front end. The back end does not use this attribute to determine whether to perform database and table sharding.|Yes|multi|

## Development in wizard mode {#section_trc_5ls_5fb .section}

Configure the source and destination of data for a synchronization task.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62192/155142993532098_en-US.png)

|Parameter|Description|
|:--------|:----------|
|Data Source|The datasource attribute in the preceding parameter description. Select the data source that you have configured.|
|Table|The table attribute in the preceding parameter description. Select the source table.|
|Filter|The filter condition for the data to be synchronized. Currently, filtering based on the limit keyword is not supported. SQL syntaxes vary with data sources.|
|Shard Key|The shard key. You can use a column in the source table as the shard key. We recommend that you use the primary key or an indexed column. Only integer fields are supported. If data sharding is performed based on the configured shard key, data can be read concurrently to improve data synchronization efficiency.**Note:** The Shard Key parameter is displayed only when you configure the source of data for a synchronization task.

 |

Configure mappings of fields \(the column attribute in the preceding parameter description\).

Each source table field on the left maps a destination table field on the right. You can click Add to add a mapping or move the cursor over a line and click Delete to delete the current mapping.

|Parameter|Description|
|---------|-----------|
|Map Fields with the Same Name|Click Map Fields with the Same Name to establish a mapping between fields with the same name. Note that the data type must be consistent.|
|Map Fields in the Same Line|Click Map Fields in the Same Line to establish a mapping for the same row. Note that the data type must be consistent.|
|Remove Mappings|Click Remove Mappings to remove mappings that have been established.|
|Auto Layout|The fields are automatically sorted based on specified rules.|
|Change Fields in Source Table![](images/32103_en-US.png)|You can manually edit fields in the source table. Each field occupies a row. The first and the last blank rows are included, while other blank rows are ignored.|
|Add|Click Add to add a mapping.-   You can enter constants. Each constant must be enclosed in single quotation marks, such as 'abc' and '123'.
-   You can use scheduling parameters, such as $\{bizdate\}.
-   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
-   If the value you entered cannot be parsed, the type is displayed as Unidentified.

|

**Configure channel control.**

|Parameter|Description|
|---------|-----------|
|DMU|The unit that measures the resources \(including CPU, memory, and network resources\) consumed by Data Integration. A DMU represents the minimum operating capability of a Data Integration task, that is, the data synchronization processing capability given limited CPU, memory, and network resources.|
|Concurrent Jobs|The maximum number of threads used to concurrently read data from the source or write data into the data storage media in a data synchronization task. In wizard mode, you can configure the concurrency for a task on the wizard page.|
|Dirty Data Records Allowed|The maximum number of errors or dirty data records allowed.|
|Task Resource Group|The machines on which tasks are run. If a large number of tasks are run on the default resource group, some tasks may be delayed due to insufficient resources. In this case, we recommend that you add a custom resource group. Currently, a custom resource group can be added only in China \(Hangzhou\) and China \(Shanghai\). For more information, see <xref\>.[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#)|

## Development in script mode {#section_pvs_k4s_5fb .section}

The following code is an example of configuration for a single table in one database. For more information about attributes, see the preceding parameter description.

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {
                "datasource": "px_aliyun_hymysql",// The data source name.
                "column": [// The source table columns.
                    "id",
                    "name",
                    "sex",
                    "salary",
                    "age",
                    "pt"
                ],
                "where": "id=10001",// The filter condition.
                "splitPk": "id",// The shard key.
                "table": "person"// The source table name.
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
            "concurrent": 7,// The number of concurrent threads.
            "throttle": true,// Indicates whether to throttle the transmission rate.
            "mbps": 1,// The maximum transmission rate.
            "dmu": 5// The DMU value.
        }
    }
}
```

