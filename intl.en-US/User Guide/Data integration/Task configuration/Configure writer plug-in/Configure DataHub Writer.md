# Configure DataHub Writer {#concept_kyh_52l_q2b .concept}

This topic describes the data types and parameters supported by DataHub Writer and how to configure Writer in script mode.

DataHub is a real-time data distribution and streaming data processing platform. It can publish, subscribe, and distribute streaming data. It allows you to easily create analysis programs and applications based on streaming data.

Based on Alibaba Cloud's Apsara platform, DataHub delivers high availability, low latency, high scalability, and high throughput. Seamlessly connect to Alibaba Cloud's stream computing engine, StreamCompute, DataHub allows you to easily use SQL statements to analyze streaming data. DataHub provides the function to distribute streaming data to cloud products, currently including MaxComputer and Object Storage System \(OSS\).

**Note:** The string can only be UTF-8 encoded and the maximum length of a single string column is 1 MB.

## Parameter configuration {#section_mdl_dfl_q2b .section}

The source is connected to the sink through a channel. The channel type at the writer must be consistent with that at the Reader. Two types of channels are provided generally: memory channel and file channel. The following example describes how to configure a file channel.

```
"agent.sinks.dataXSinkWrapper.channel": "file"
```

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|accessId|The accessId of the Datahub.|Yes|N/A|
|accessKey|The accessKey of the DataHub.|Yes|N/A|
|endpoint| For an access request to a DataHub resource, select the correct domain name based on the service that the resource belongs.

 |Yes|N/A|
|maxRetryCount|The maximum number of retries for task failure.|No|N/A|
|mode|The write mode when the value type is string.|Yes|N/A|
|parseContent|Parses the content.|Yes|N/A|
|project|Project is the basic unit of DataHub data that contains multiple topics.**Note:** DataHub projects are independent from MaxCompute projects. Projects you created in MaxCompute cannot be used in DataHub.

|Yes|N/A|
|topic| Topic is the smallest unit of the DataHub subscription and publication, you can use topic to represent one type or one type of streaming data.

 |Yes|N/A|
|maxCommitSize|To improve writing efficiency, DataX-On-Flume collects the buffer data and submits it to the target end in batches when the collected data size reaches maxCommitSize \(in MB\). The maxCommitSize is 1 MB by default.| No|1 MB|
|batchSize|To improve writing efficiency, DataX-On-Flume collects the buffer data and submits it to the target end in batches when the number of collected data entries reaches batchSize \(in entry\). The batchSize is 1024 entries by default.|No|1,024|
|maxCommitInterval|To improve writing efficiency, DataX-On-Flume collects buffer data and submits it to the target end in batches when the number of collected data entries reaches the limit of maxCommitSize and batchSize. If the data collection source does not produce data for extensive periods, the maxCommitInterval parameter \(the maximum time allowed for the buffer data preservation, beyond which the data is compulsively delivered in milliseconds\) is increased to ensure the timely delivery of data\( . The maxCommitInterval is 30000 \(30 seconds\) by default.| No|30|
|parseMode|Log parsing mode includes non-parsing default mode and CSV mode. In the non-parsing mode, one collected log line is written directly as a column of DataX Record. The CSV mode supports configuring one column separator, which separates one log line into multiple columns of DataX Record.| No|default|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is not supported currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a synchronization job to read data from memory:

```
{
    "type": "job",
    "version": "2.0",//version size
    "steps": [
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader ",
            "category":"reader"
        },
        {
            "stepType":"datahub", //plug-in name
            "parameter":{
                "datasource":"", //Name of the data source
                "topic": "", //Topic is the smallest unit of DataHub subscription and publishing. You can use Topic to represent a class or a kind of streaming data.
                "maxRetryCount":500,//Number of retries
                "maxCommitSize": 1048576//data to be saved to buffer size reaches maxrefersize size (in MB) when, batch submitted to the destination
            },
            "name": "Writer ",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": ""//Number of error records
        },
        "speed": {
            "concurrent": 20,// Number of concurrent jobs
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "dmu": 20 //DMU values
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

