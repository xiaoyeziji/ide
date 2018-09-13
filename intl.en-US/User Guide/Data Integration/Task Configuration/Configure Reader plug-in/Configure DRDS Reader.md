# Configure DRDS Reader {#concept_kvr_2xh_p2b .concept}

The DRDS Reader plug-in allows your to read data from DRDS \(distributed RDS\). At the underlying implementation level, DRDS Reader connects to a remote DRDS database through JDBC and runs corresponding SQL statements to SELECT data from the DRDS database.

Currently, the DRDS plug-in is only adapted to the MySQL engine. DRDS is a distributed MySQL database, and most of the communication protocols are applicable to MySQL use cases.

Specifically, DRDS Reader connects to a remote DRDS database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote DRDS database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data synchronization. Datasets are passed to the downstream writer for processing.

DRDS Reader concatenates the table, column, and WHERE information you configured into SQL statements and sends them to the DRDS database. Unlike the MySQL database, DRDS as a distributed database is unable to adapt to all MySQL protocols, and does not support complex clauses such as Join.

DRDS Reader supports most data types in MySQL. Check whether your data type is supported.

DRDS Reader converts MySQL data types as follows:

|MySQL data type|DRDS Data Management|
|:--------------|:-------------------|
|Integer|Int, tinyint, smallint, mediumint, and bigint|
|Floating point|Float, double, decimal|
|String|varchar, char, tinytext, text, mediumtext, or longtext|
|Date and time|date, datetime, timestamp, time, or year|
|Boolean| bit or bool|
|Binary|tinyblob, mediumblob, blob, longblob, or varbinary|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table|The table selected for extraction.|Yes|N/A|
|column|The column name set to be synchronized in the configured table. Field information is described with arrays in JSON. \['\*'\] indicates all columns by default.-   - Column pruning is supported, which means you can select some columns to export. 
-    - Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   - Constant configuration is supported. You must follow the MySQL SQL syntax format, for example \[“id”, “\`table\`“, “1”, “‘bazhen.csy’”, “null”, “to\_char\(a + 1\)”, “2.3” , “true”\], 
    -   where id refers to the ordinary column name, 
    -   \`table\` is the name of the column containing reserved words,
    -    1 is an integer constant,
    -    'bazhen.csy' is a string constant, 
    -   null refers to null pointer, 
    -   CHARLENGTH\(s\) is the function expression to calculate the string length, 
    -   2.3 is a floating point, 
    -   and true is a boolean value.
-   Column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|where|Filtering condition. DRDS Reader concatenates an SQL command based on specified column, table, and WHERE conditions and extracts data according to the SQL statement. For example, you can set the WHERE condition during a test. In actual business scenarios, the data on the current day is usually required to be synchronized, in which case you can set the WHERE condition to STRTODATE\(‘$\{bdp.system.bizdate\}’, ‘%Y%m%d’\) <= taday AND taday < DATEADD\(STRTODATE\(‘$\{bdp.system.bizdate\}’, ‘%Y%m%d’\), interval 1 day\).-   - The where condition can be effectively used for incremental synchronization.
-   - If the where condition is not set or is left null, full table data synchronization is applied.

|No|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368022117672_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: The table in the preceding parameter description. Select the table to be synchronized.
    -   Data filtering: You should synchronize the filter for the data. Limit keyword filter is not supported yet.  SQL syntaxes vary with data sources.
    -   Splitting key: You can use a column in the source table as the splitting key. It is recommended to use a primary key or an indexed column as the splitting key. Only integer fields are supported.

         During data reading, the data is split based on the configured fields to achieve concurrent reading, improving data synchronization efficiency. The configuration of splitting key is related to the source selection in data synchronization. 

        **Note:** The splitting key configuration item is displayed only when you configure the data source.

2.  Field mapping: The column in the preceding parameter description.

    The Source Table Field on the left maps with the Target Table Field on the right. Click **Add Line**, and then a field is added. Hover the cursor over a line, click **Delete**, and then the line is deleted.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368022117673_en-US.png)

    -   In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    By clicking Add Row,

    -    you can enter constants. Each constant must be enclosed in a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   Enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as 'Not Identified'.
3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368022117675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   Number of error records: The maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\), see [Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from an RDBMS database:

```
{
    "type": "job",
    "version": "2.0",//Indicates the version.
    "steps":[
        {
            "stepType": "drds",//plug-in name
            "parameter":{
                "datasource": "", //Name of the data source
                "column": [//column name
                    "id",
                    "name"
                ],
                "where": "", //Filtering condition
                "table": "",//The name of the target table.
                "splitPk": "", //Splitting key
            },
            "Name": "Reader ",
            "category":"reader"
        },
        {//You can locate the corresponding writer plug-in documentation among the following documentations.
            "stepType": "stream", //plug-in name
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//Number of error records
        },
        "speed":{
            "throttle":false, // do you want to limit the flow?
            "concurrent": "1", // Number of concurrency
            "DMU": 1 // DMU Value
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
}:"Writer"
            }
        ]
    }
}
```

## Additional information {#section_htd_3th_p2b .section}

**Consistency view**

As a distributed database, DRDS cannot provide a consistency view of multiple tables in multiple databases. Different from MySQL where data is synchronized in a single table of one database, DRDS Reader cannot extract the snapshot of database/table sharding at the same time slice, that is to say, DRDS Reader obtains different snapshots of table shards when extracting data from different underlying table shards. Therefore, strong consistency cannot be ensured.

**Database coding**

Drds provides flexible encoding options, including database‑level, table‑level, and field‑level encoding. Different encodings can also be configured. The priority \(from high to low\) is field, table, database, and instance.  We recommend that you use UTF-8 for database encoding at the database level.

DRDS Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete transcoding at the underlying level.  Therefore, DRDS Reader can identify the encoding and complete transcoding automatically without the need to specify the encoding.

DRDS Reader cannot identify the inconsistency between the encoding written to the underlying layer of DRDS and the configured encoding, nor provide a solution. Due to this issue, the exported codes may contain garbage codes.

**Incremental Synchronization**

Since DRDS Reader extracts data using JDBC SELECT statements, you can extract incremental data using the SELECT and WHERE conditions  in the following ways:

-   When database online applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\).  For this type of applications, DRDS Reader only requires the WHERE condition followed by the timestamp of the last synchronization phase.
-   For new streamline data, DRDS Reader requires the WHERE condition followed by the maximum auto-increment ID of the last synchronization phase.

In case that no field is provided for the business to identify the addition or modification of data, DRDS Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL security**

DRDS Reader providesquerySQL statements for you to SELECT data by yourself. DRDS Reader conducts no security verification onquerySQL. The security during use is ensured by the data synchronization users.

