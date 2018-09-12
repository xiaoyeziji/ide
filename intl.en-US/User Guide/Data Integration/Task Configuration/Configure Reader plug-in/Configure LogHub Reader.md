# Configure LogHub Reader {#concept_ihy_fvq_p2b .concept}

Honed originally by the Big Data demands of Alibaba Group, Log Service \(or "LOG" for short, formerly “SLS”\) is an all-in-one service for real-time data. With its capabilities to collect, consume, deliver, query, and analyze log-type data, Log Service allows you to process and analyze massive amounts of data much more efficiently. LogHub Reader uses the Java SDK of the Log Service to consume real-time log data in LogHub, and converts the log data to the Data Integration transfer protocol and sends the converted data to Writer.

## Implementation {#section_ig1_4vq_p2b .section}

LogHub Reader consumes real-time log data in LogHub by using the following version of Log Service Java SDK:

```
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>aliyun-log</artifactId>
    <version>0.6.7</version>
</dependency>
```

Logstore is a component of the Log Service for collecting, storing, and querying log data. Logstore read and write logs are stored on a shard. Each log library consists of several partitions, each of which consists of the left closed right open interval of MD5, each interval range is not covered by each other, and the range of all the intervals is the entire MD5 range of values, each partition can provide a certain level of service capability.

-   Writing: 5 MB/s, 2000 times/s.
-   Read: 10 MB/s, 100 times/s.

LogHub Reader consumes logs in shards, and the detailed consumption process \(GetCursor and BatchGetLog-related APIs\) is as follows:

-   Obtains a cursor based on the interval range.
-   Reads logs based on the cursor and step parameters and returns the next cursor.
-   Moves the cursor continuously to consume logs.
-   Splits tasks by shard for concurrent execution.

LogHub Reader supports LogHub type conversion, as shown in the following table:

|Datax internal type|Loghub Data Type|
|:------------------|:---------------|
|String|String|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|endpoint| Description: The Log Service endpoint is a URL for accessing a project and its internal log data. It is associated with the Alibaba Cloud region and name of the project. Service entry for each region, see [service entry](https://www.alibabacloud.com/help/zh/doc-detail/29008.htm).

 |Yes|N/A|
|accessId|Description: It refers to an AccessKey for accessing the Log Service, which is used to identify the accessing user.|Yes|N/A|
|accessKey|Description: It refers to another AccessKey for accessing the Log Service, which is used to verify the user's key.|Yes|N/A|
|project|Description: It refers to the project name of the target Log Service, which is the resource management component in the Log Service for isolating and controlling resources.|Yes|N/A|
|logstore|Description: It refers to the name of the target Logstore. Logstore is a component of the Log Service for collecting, storing, and querying log data.|Yes|N/A|
|batchSize|Description: It refers to the number of data entries queried from the Log Service at a time.|No |128|
|column|Description: Column names in each data entry. Here, you can set a metadata item in the Log Service as the synchronization column. Supported metadata items include "C\_Topic", "C\_MachineUUID", "C\_HostName", "C\_Path", and "C\_LogTime", which represent the log topic, unique identifier of the collection machine, host name, path, and log time, respectively.The sub-table represents the log theme, the acquisition machine uniquely identified, the host name, path, log time, and so on.

**Note:** The values of fields in the format are case insensitive.

|Yes|N/A|
|Begindatetime|Start time of data consumption. The parameter defines the left border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013000\) and can work with the scheduling time parameter in DataWorks.**Note:** The maid and enddatetime combinations are used together.

|Required: Select either this parameter or endTimestampMillis.|Blank|
|Enddatetime|End time of data consumption. The parameter defines the right border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013010\) and can work with the scheduling time parameter in DataWorks.**Note:** The combination of enddatetime and maid is used together.

|No |N/A|
|Begintimestampmillis|Description: It refers to the start time of data consumption in milliseconds and is the left boundary of the time range \(left-closed and right-open\).**Note:** Begintimestampmillis and endtimestampmillis combination for use.

1 represents the beginning of the log service cursor cursormode. Begin. The beginDateTime mode is recommended.

|Required: Select either this parameter or beginDateTime.|N/A|
|Endtimestampmillis|Description: It refers to the end time of data consumption in milliseconds and is the right boundary of the time range \(left-closed and right-open\).**Note:** Endtimestampmillis and begintimestampmillis combination for use.

-1 represents the last location of the log service cursor, cursormode.End. The endDateTime mode is recommended.

|Required: Select either this parameter or endDateTime.|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16233/15367219387878_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Log start time: Start time of data consumption: It defines the left border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013000\) and can work with the scheduling time parameter in DataWorks.
    -   Log end time: End time of data consumption. It defines the right border of a time range \(left closed and right open\) in the format of yyyyMMddHHmmss \(such as 20180111013010\) and can work with the scheduling time parameter in DataWorks.
2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16233/15367219387879_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Not identified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367219387675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

The following is a script configuration sample. For details about parameters, see the preceding Parameter Description.

```
{
 "type": "job",
 "version": "1.0"} //Indicates the version.
 "steps":[
     {
         "stepType": "loghub", // plug-in name
         "parameter": {
             "datasource": "", // Data Source
             "column": [// Field
                 "col0",
                 "col1",
                 "col2",
                 "col3",
                 "col4",
                 "C_topic", // log theme
                 "C_hostname", // host name
                 "C_path", // path
                 "C_logtime" // log time
             ],
             "beginDateTime":"", // start time of data consumption
             "batchSize": "", // number of data lines to query from the log service at once
             "endDateTime": ",",/end time of data consumption
             "fieldDelimiter": "," ,//Delimiter of each column
             "encoding": "UTF-8", // encoding format
             "logstore": "//: name of the target log Library
         },
         "name": "Reader ",
         "category": "Reader"
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
         "throttle":false,//false indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
         "concurrent": "1",//Number of concurrent tasks
         "dmu": 1 // DMU Value
     }
 },
 "order ":{
     "hops ":[
         {
             "from": "Reader ",
             "to": "Writer"
         }
     ]
 }
}
```

