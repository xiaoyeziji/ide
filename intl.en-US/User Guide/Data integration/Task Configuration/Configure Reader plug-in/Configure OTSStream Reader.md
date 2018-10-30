# Configure OTSStream Reader {#concept_m4h_5gr_p2b .concept}

In this article we will show you the data types and parameters supported by OTSStream Reader and how to configure Reader in both wizard mode and script mode.

OTSStream Reader plug-in is mainly used for exporting Table Store incremental data. Incremental data can be seen as operation logs which include data and operation information.

Different from full export plug-in, incremental export plug-in only has multi-version mode and it doesn't support specified columns. This is related to the principle of incremental export. See the following for more information about export format.

Before using the plug-in, ensure that the Stream feature is enabled. You can enable the feature when creating the table or enable it using SDK UpdateTable API.

How to enable Stream:

```
Syncclient client = new syncclient ("","","","");
Enable Stream when you create the table:
CreateTableRequest createTableRequest = new CreateTableRequest(tableMeta);
createTableRequest.setStreamSpecification(new StreamSpecification(true, 24)); // 24 means that the incremental data is retained for 24 hours
client.createTable(createTableRequest);
If Stream is not enabled when the table is created, you can enable it with UpdateTable:
UpdateTableRequest updateTableRequest = new UpdateTableRequest("tableName");
createTableRequest.setStreamSpecification(new StreamSpecification(true, 24)); // 24 means that the incremental data is retained for 24 hours 
client.updateTable(updateTableRequest);
```

## Implementation {#section_ngl_bhr_p2b .section}

You can enable Stream and set expiration time by using SDK UpdateTable feature to enable incremental feature. When incremental feature is enabled, Table Store server saves your operation logs additionally. Each partition has a sequential operation log queue. Each operation log is moved by garbage collection after a period of time which is the expiration time you specified.

Table Store SDK provides several Stream-related APIs for reading these operation logs. The incremental plug-in also gets incremental data with Table Store SDK API, transforms incremental data into multiple 6-tuples \(pk, colName, version, colValue, opType, sequenceInfo\), and imports them into MaxCompute.

## The format of the export data {#section_n3v_khr_p2b .section}

In Table Store multi-version mode, the format of table data is in three-level mode, namely row \> column \> version. One row can have multiple columns. The column name is not fixed, and each column can have multiple versions. Each version has a specific timestamp \(version number\).

You can perform read/write operations with Table Store API. Table Store records incremental data by recording your recent write operations to the table \(or data change operation\). Therefore, incremental data can also be seen as a series of operation records.

Table Store has three types of data change operations: PutRow, UpdateRow, and DeleteRow: 

-   PutRow: write a row. If the row already exists, it is overwritten.
-   UpdateRow: Updates a row without changing other data of the original row. Update may include adding or overwriting \(if the corresponding version of the corresponding column already exists\) some column values, deleting all the versions of a column, and deleting a version of a column.
-   DeleteRow: Delete a row.

Table Store generates corresponding incremental data records according to each type of operation. Reader plug-in reads the records and exports the data in the format of Datax.

Because Table Store has the feature of dynamic column and multi-version, a row exported by Reader plug-in doesn't correspond to a row in Table Store but a version of a column in Table Store. A row in Table Store can be exported as multiple rows. Each row includes primary key value, the name of the column, the timestamp of the version under the column \(version number\), the value of the version, and operation type. If isExportSequenceInfo is set as true, time sequence information is also included.

When the data is transformed into Datax format, we define four types of operations as follows: 

-   U \(UPDATE\): Writes a version of a column.
-   DO \(DELETE\_ONE\_VERSION\): Deletes a version of a column.
-   DA \(DELETE\_ALL\_VERSION\): Deletes all the versions of a column. Delete all the versions of the corresponding column according to primary key and column name.
-   DR \(DELETE\_ROW\): Deletes a row. Delete all the data of the row according to primary key.

Assuming that the table has two primary key columns. The names of the two primary key columns are pkName1 and pkName2. The example is as follows:

| pkName1| pkName2|columnName|timestamp|columnValue| opType|
|--------|--------|----------|---------|-----------|-------|
|pk1\_V1|pk2\_V1| col\_a| 1441803688001|col\_val1|U|
|pk1\_V1|pk2\_V1|col\_a|1441803688002|col\_val2|U|
|pk1\_V1|pk2\_V1|col\_b|1441803688003|col\_val3|U|
|pk1\_V2|pk2\_V2|col\_a| 1441803688000|—|Do|
|pk1\_V2|pk2\_V2|col\_b|—|—|Da|
|pk1\_V3|pk2\_V3|—|—|—|Dr|
|pk1\_V3|pk2\_V3|col\_a|1441803688005|col\_val1|U|

Assuming that the export data has seven rows as in the shown preceding example, corresponding to three rows in Table Store table. The primary keys are \(pk1\_V1, pk2\_V1\), \(pk1\_V2, pk2\_V2\), and \(pk1\_V3, pk2\_V3\).

-   For the row whose primary key is \(pk1\_V1, pk2\_V1\), three operations are required, respectively writing two versions of col\_a column and one version of col\_b column.
-   For the row whose primary key is \(pk1\_V2, pk2\_V2\), two operations are required, respectively deleting one version of col\_a column and all versions of col\_b column.
-   For the row whose primary key is \(pk1\_V3, pk2\_V3\), two operations are required, respectively deleting the whole row and writing one version of col\_a column.


Currently OTSStream Reader supports all OTS types. The conversion list for OTS types is as follows:

|Type Classification|Otsstream Data Type|
|:------------------|:------------------|
|Integer|Integer|
|Float|Double|
|String type|String-|
|Boolean|Boolean|
|Binary|Binary|

## Parameter description​ {#section_nc1_jjr_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|dataSource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|dataTable|The name of the table from which the incremental data is exported.  The table needs to enable the Stream feature. You can enable the feature when creating the table or enable it using UpdateTable API.|Yes|N/A|
|statusTable|The name of the table used by the Reader plug-in to record the status, these States can be used to reduce scanning of data in non-target ranges to speed up export. statusTable is the table for recording status in Reader. If the table doesn't exist, Reader creates the table automatically. When an offline export task is completed, you must not delete the table. The statuses recorded in the table can be used for the next export task.-   You don't need to create the table and you only need to provide a name for the table. Reader plug-in tries to create the table under your instance. If the table doesn't exist, it is created. If the table already exists, it judges whether the Meta of the table is consistent with expectation. If it is not consistent, an exception is thrown.
-   When an export is completed, you must not delete the table. The statuses of the table can be used for the next export task.
-   The table enables TTL and data expire automatically, therefore we can consider that the data volume is small.
-   For the Reader configurations of different dataTables under one instance, you can use the same statusTable. The status messages recorded are independent of each other. 

In conclusion, you must configure a name such as TableStoreStreamReaderStatusTable. Note that the name must not be duplicate with that of business-related tables.|Yes|N/A|
|startTimestampMillis|The left boundary of the time range of the incremental data \(left closed right\), in milliseconds.-   Reader finds the point corresponding to startTimestampMillis in statusTable, and reads and exports data from that point.
-   If the corresponding point is not found in statusTable, the system reads from the first entry of the incremental data retained in the system and skips the data whose write time is earlier than startTimestampMillis.

|No |N/A|
|endTimestampMillis|The right border of the time range \(left closed and right open\) of incremental data, in milliseconds.-   After exporting data from the point of startTimestampMillis, Reader finishes data export at the first entry of data whose timestamp is later than endTimestampMillis.
-   When all the incremental data are read, the read is completed, even if endTimestampMillis is not reached.

|No|N/A|
|date| The data format is yyyyMMdd, for example 20151111, which means exporting the data of the date. If you do not specify a date, you must specify a maid and a maid, and vice versa. For example, Alibaba Cloud Data Process Center scheduling only supports day level. Therefore, the function of the configuration is similar to startTimestampMillis and endTimestampMillis.|No|N/A|
|isExportSequenceInfo|Whether to export time sequence information. Time sequence information includes the write time of data.  The default value is false which means not to export data.|No|N/A|
|maxRetries|The maximum number of retries of each request when incremental data is read from TableStore. The default value is 30. There are intervals between retries. The total time of 30 retries is approximately 5 minutes which generally doesn't require changes.|No|N/A|
|startTimeString|The left border of the time range \(left closed and right open\) of incremental data, in milliseconds \(in the format of yyyymmddhh24miss\).|No|N/A|
|endTimeString|The right border of the time range \(left closed and right open\) of incremental data, in millisecond \(in the format of yyyymmddhh24miss\). |No |N/A|

## Development in wizard mode {#section_q1t_vkr_p2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_ewq_xkr_p2b .section}

The following is a script configuration sample. For details about parameters, see the preceding Parameter Description.

```
{
    "type": "job",
    "version": "2.0"} //Indicates the version.
    "steps":[
        {
            "stepType": "otdsstream", // plug-in name
            "parameter": {
                "statusTable": "TableStoreStreamReaderStatusTable",//The name of the table for recording the status.
                "maxRetries": 30, // when you read incremental data from the tablestore, maximum number of retries per request, by default 30
                "isExportSequenceInfo": false, // do you want to export timing information?
                "datasource": "$ srcdatasource", // Data Source
                "startTimeString": "$ {starttime }", // The left boundary of the time range of the incremental data (left closed right on)
                "table": "ok",//Target table name
                "endTimeString ": "$ {endtime}" // time range of incremental data (left closed right) right Border
            },
            "name": "Reader ",
            "category": "Reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream ",
            "parameter":{}
            "name": "Writer ",
            "category": "Writer"
        }
    ],
    "setting":{
        "errorLimit": {
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
                "from": "Reader ",
                "to": "Writer"
            }
        ]
    }
}
```

