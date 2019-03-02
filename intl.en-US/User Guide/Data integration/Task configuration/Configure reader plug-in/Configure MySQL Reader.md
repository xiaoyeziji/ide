# Configure MySQL Reader {#concept_edt_bgp_p2b .concept}

This topic describes how to configure a MySQL Reader. The MySQL Reader connects to a remote MySQL database through the JDBC connector. The SQL query statements are generated and sent to the remote MySQL database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data synchronization. Datasets are then passed to the downstream writer for processing.

In short, MySQL Reader reads data from the MySQL database underlying level by using the JDBC connector to connect the MySQL Reader to the remote MySQL database, and runs SQL statements to select data from the MySQL database.

MySQL Reader supports table and view reading. In the table field, you can specify all columns in sequence, specify certain columns, adjust column order, specify constant fields, and configure MySQL functions, such as now\(\).

MySQL Reader supports the following MySQL data types.

|Type classification|MySQL data type|
|:------------------|:--------------|
|Integer|int, tinyint, smallint, mediumint, int, bigint|
|Floating point|float, double, decimal|
|String|varchar, char, tinytext, text, mediumtext, longtext|
|Date and time|date, datetime, timestamp, time, year|
|Boolean|bit, bool|
|Binary|tinyblob, mediumblob, blob, longblob, varbinary|

**Note:** 

-   Only the field types listed in the preceding table are supported.
-   MySQL Reader classifies tinyint\(1\) as the integer type.

## Type conversion list {#section_f5g_g1n_q2b .section}

MySQL Writer converts the MySQL data types as follows:

|Type classification|MySQL data type|
|:------------------|:--------------|
|Integer|Int, Tinyint, Smallint, Mediumint, Bigint|
|Float|Float, Double, Decimal|
|String type|Varchar, Char, Tinytext, Text, Mediumtext, LongText|
|Date and time type| Date, Datetime, Timestamp, Time, Year|
|boolean|Bool|
|Binary|Tinyblob, Mediumblob, Blob, LongBlob, Varbinary|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the added data source name . Adding data source is supported in script mode.|Yes|N/A|
|table.|You select a table name that requires synchronization, and a data integration Job can only synchronize one table.|Yes|N/A|
|column|The column name set to be synchronized in the configured table. Field information is described with JSON arrays . \[ \* \] indicates all columns by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. You must follow the MySQL SQL syntax format, for example `["id", "table", "1", "'mingya.wmy'", "'null'", "to_char(a + 1)", "2.3", "true"]`.
    -   ID is a normal column name
    -   Table is a column name that contains Reserved Words
    -   1 for plastic digital Constants
    -   'mingya. wmy' is a String constant \(note that a pair of single quotes is required\)
    -   Null is a null pointer
    -   CHAR\_LENGTH\(s\) is the computed String Length Function
    -   2.3 is a floating point number
    -   true is a Boolean Value
-   The column must contain the specified column set for synchronization and it cannot be blank.

|Yes|N/A|
|SplitPk|If SplitPk is specified when using MySQL Reader to extract data, it means the fields are represented by SplitPk for data sharding. Data synchronization starts using concurrent tasks to synchronize data, which greatly improves data synchronization efficiency.-   We recommend you use the table primary keys for SplitPk because the primary keys are usually even and less likely to generate data hot spots during data sharding.
-   Currently, SplitPk only supports data sharding for integer data types. Other types such as string, floating point, and date are not supported. If you specify an unsupported data type, the SplitPk is ignored and the data is synchronized using a single channel.
-   If the SplitPk is unspecified the table data is synchronized using a single channel. For example, when SplitPk is not provided or when the SplitPk value is null.

|No |N/A|
|WHERE|In actual business scenarios, the current day data is usually required for synchronization. You can specify the WHERE clause as gmt\_create \> $bizdate.-   The WHERE clause can be effectively used for incremental synchronization. Full synchronization is performed when the WHERE clause is not specified, for example, when the WHERE key or value is not provided.
-   You cannot specify limit 10 as the WHERE clause, because it does not conform to MySQL WHERE clause requirements.

|No|N/A|
|querySQL\(only available in advanced mode\)|querySQL is used for customizing a filter SQL in business scenarios, where the WHERE clause is an insufficient filter. When this item is configured, the data synchronization system filters data with this configuration item directly, instead of configuration items, such as tables and columns. For example, for data synchronization after multi-table join, use `select a, b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, MySQL Reader directly ignores the configuration of table, column, WHERE, and SplitPk conditions. The querySQL priority is higher than the table, column, WHERE, and SplitPk. The datasource uses querySQL to parse information, such as a user name and password.|No|N/A|
|singleOrMulti \(applies only to hardedsharded tables and sharded databases\)|Represents a sharded table or sharded databases, and the wizard mode is converted into Script Mode to actively generate this configuration `"singleOrMulti": "multi"`.This configuration is not automatically generated by the script task template, and must be added manually, or only the first data source is recognized. singleOrMulti is just the frontend, and the back-end does not use this for sharded table judgment.|Yes|  multi|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the data source and destination of the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15515083437781_en-US.png)

    Configurations:

    -   Data source: The data source in the preceding parameter description. Enter the configured data source name .
    -   Table: The table in the preceding parameter description. Select the table for synchronization.
    -   Data filtering: The data synchronization filtering criteria. Currently, keyword filtering limits are not supported. The SQL syntax is consistent with the selected data source.
    -   Shard keys: You can use a column in the source data table as a shard key. We recommend that you use a primary key or an indexed column as the shard key, and only Integer type fields are supported.

        The data shard is based on configured fields during data reading to achieve concurrent reading, and improve data synchronization efficiency.

        **Note:** The shard key configuration is related to the source selection in data synchronization. The shard key configuration item is displayed only when you configure the data source.

2.  The field mapping is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one correspondence, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15515083437782_en-US.png)

    -   Peer mapping: Click **peer mapping** to establish a corresponding mapping relationship in the peer, and take special note of the data type match.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit fields, where each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value entered cannot be parsed, the type is displayed as unidentified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15515083437675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources including CPU, memory, and network bandwidth consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: The maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: The machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource. We recommend that you add a Custom Resource Group. Currently, only East China 1 and East China 2 supports adding custom resource groups. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

A script sample for a single-library and single-table, for example, can be found in the above parameter descriptions.

```
{
    "type": "job",
    "version": "1.0"} //Indicates the version.
    "steps":[
        {
            "stepType": "mysql", // plug-in name
            "parameter": {
                "Column": [// column name
                    "id",
                ],
                "connection": [
                    {   "querysql":["select a,b from join1 c join join2 d on c.id = d.id;"],
                        "datasource": "", // Data Source
                        "table": [// table name
                            "xxx"
                        ]
                    }
                ],
                "where": "", //Filtering condition
                "Splitpk": "ID", // cut key
                "encoding": "UTF-8", // encoding format
            },
            "name": "Reader ",
            "category": "reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream ",
            "parameter":{}
            "name": "Writer ",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "throttle": false, //false stands for open current, the speed of the lower limit does not work, and true stands for current limit
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 //DMU Value
        }
    },
    "order":{
        "hops":[
            {
                 "from":"Reader",
                "to": "Writer"
            }
        ]
    }
}
```

