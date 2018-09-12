# Configure MySQL Reader {#concept_edt_bgp_p2b .concept}

MySQL Reader connects to a remote MySQL database through the JDBC connector. The SQL query statements are generated and sent to the remote MySQL database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data synchronization. Datasets are passed to the downstream writer for processing.

In short, with the JDBC connector, MySQL Reader connects to the remote MySQL database and runs SQL statements to select data from the MySQL database, achieving data reading from the MySQL database at the underlying level.

MySQL Reader supports table and view reading. In table field, you can specify all columns in sequence, specify certain columns, adjust the column order, specify constant fields, and configure MySQL functions, such as now\(\).

MySQL Reader supports the following MySQL data types.

|Type Classification|MySQL data type|
|-------------------|---------------|
|Integer|int，tinyint，smallint，mediumint，int，bigint|
|Floating point|float，double，decimal|
|String|varchar，char，tinytext，text，mediumtext，longtext|
|Date and time|date，datetime，timestamp，time，year|
|Boolean|bit，bool|
|Binary|tinyblob，mediumblob，blob，longblob，varbinary|

**Note:** 

-   Apart from the field types listed here, other types are not supported.
-   MySQL Reader classifies tinyint\(1\) as the integer type.

## Type conversion list {#section_f5g_g1n_q2b .section}

MySQL Writer converts the MySQL data types as follows:

|Type Classification|MySQL data type|
|:------------------|:--------------|
|Integer|Int、Tinyint、Smallint、Mediumint和Bigint|
|Float|Float、Double and Decimal|
|String type|Varchar, Char, Tinytext, Text, Mediumtext, and LongText|
|Date and time type| Date、Datetime、Timestamp、Time and Year|
|boolean|Bool|
|Binary|Tinyblob, Mediumblob, Blob, LongBlob and Varbinary|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table.|You select a table name that requires synchronization, and a data integration Job can only synchronize one table.|Yes|N/A|
|column|Description: The column name set to be synchronized in the configured table. Field information is described with arrays in JSON. \[ \* \] indicates all columns by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. You must follow the MySQL SQL syntax format, for example `["id", "table", "1", "'mingya.wmy'", "'null'", "to_char(a + 1)", "2.3" , "true"]`.
    -   ID is normal column name
    -   Table is a column name that contains Reserved Words
    -   1 for plastic digital Constants
    -   'mingya. wmy' is a String constant \(note that a pair of single quotes is required\)
    -   Null is a null pointer
    -   CHAR\_LENGTH\(s\) is the computed String Length Function
    -   2.3 is a floating point number
    -   true is a Boolean Value
-   column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|splitPk|| splitPk | If you specify the splitPk when using the MySQL Reader to extract data, it means that you want to use the fields represented by splitPk for data sharding. Then, the data synchronization starts concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   If you are using splitPk, we recommend that you use the primary keys of tables, because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   Currently, splitPk only supports data sharding for integer data types. Other types such as string, floating point, and date are not supported. If you specify an unsupported data type, the splitPk is ignored and the data is synchronized using a single channel.
-   If the splitPk is not specified \(for example, splitPk is not provided or splitPk value is null\), the table data is synchronized using a single channel.

|No |N/A|
|where|In actual business scenarios, the data on the current day is usually required to be synchronized. You can specify the WHERE condition as gmt\_create \> $bizdate.-   The where condition can be effectively used for incremental synchronization. If the where is not specified \(for example, the key or value of the where is not provided\), full synchronization is performed.
-   You cannot specify where condition as limit 10, which does not conform to requirements for MySQL SQL where clause.

|No|N/A|
|querySql\(advanced mode, wizard mode not available\)|In some business scenarios, the where condition is insufficient for filtration. In such cases, the user can customize a filter SQL using this configuration item. When this item is configured, the data synchronization system filters data using this configuration item directly instead of such configuration items as table and column. For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySql is configured, MySQL Reader directly ignores the configuration of table, column, where, and splitPk conditions. The priority of querySql is higher than table, column, WHERE, and splitPk. The datasource uses it to parse out information such as a user name and password.|No|N/A|
|Singleormulti \(applies only to split-up tables\)|Represents a sub-library table, and the wizard mode is converted into Script Mode to actively generate this configuration `"singleOrMulti": "multi"`, but the configuration script task template does not directly generate this configuration must be added manually, otherwise, only the first data source is recognized. Singleormulti is just a front-end, and the back-end does not use this as a split-Table judgment.|Yes|  multi|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15367217137781_en-US.png)

    Configurations:

    -   Data source: The data source in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    -   Data Filtering: you are about to synchronize the filtering criteria for the data, and the limit keyword filtering is not supported for the time being. The SQL syntax is consistent with the selected data source.
    -   Cut key: You can use a column in the source data table as a cut key, it is recommended that you use a primary key or an indexed column as a split key, and that only fields of type Integer be supported.

        During data reading, the data is split based on the configured fields to achieve concurrent reading, improving data synchronization efficiency.

        **Note:** The configuration of splitting key is related to the source selection in data synchronization. The splitting key configuration item is displayed only when you configure the data source.

2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15367217137782_en-US.png)

    -   Peer mapping: Click **peer mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Not identified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367217137675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

A script sample for a Single-library single-table, for example, can be found in the above parameter descriptions.

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
                    {
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

