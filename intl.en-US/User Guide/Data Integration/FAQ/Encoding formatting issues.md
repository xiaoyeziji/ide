# Encoding formatting issues {#concept_gcs_g4b_r2b .concept}

After the data integration synchronization task is formatted, synchronization failure may occur and result in dirty data, synchronization success, but the data is messy.

## Synchronization failed with dirty data generated {#section_n1g_44b_r2b .section}

**Issue Description**

The data integration task failed and dirty data is generated due to encoding problem. The error log is shown as follows:

```
016-11-18 14:50:50.766 13350975-0-0-writer ERROR StdoutPluginCollector - Dirty data:<br> 
{"exception":"Incorrect string value: '\\xF0\\x9F\\x98\\x82\\xE8\\xA2...' for column 'introduction' at row 1","record":[{"byteSize":8,"index":0,"rawData":9642,"type":"LONG"},
{"byteSize":33,"index":1,"rawData":" Hello world! (http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/56134/cn_zh/1498728641169/%E5%9B%BE%E7%89%877.png)
","type":"STRING"},
{"byteSize":8,"index":4,"rawData":0,"type":"LONG"}],"type":"writer"}
2016-11-18 14:50:51. 265 [13350975-0-0-writer] warn maid $ task-roll back this write, commit by writing one row at a time. Because: Java. SQL. batchupdateexception: incorrect string value: '\ xq0 \ x9f \ x88 \ xB6 \ XeF \ xb8... 'For column' introduction 'at Row 1
```

**Root cause**

The user does the appropriate encoding formatting for the database, or when adding a data source, no encoding is set to maid, because only chain encoding supports synchronous emotiffs.

**Solution**

-   When you add a data source in JDBC format, you need to modify the settings of the scanner, such as jdbc:mysql://xxx.x.x.x:3306/database? Com. mySQL. JDBC. faultinjection. servercharsetindex = 45, so that you can set the emotability on the data source to synchronize successfully.
-   Modify the data source encoding format to utf8mb4. For example, you can modify the database encoding format of the RDS on the RDS console.

## Synchronization succeeded with data garbled {#section_tjk_2pb_r2b .section}

**Issue Description**

The data synchronization task succeeded, but the data is garbled.

**Root cause**

Three reasons for garbled data:

-   Source-side data is already out of order.
-   The encoding for the database and the client is not the same;
-   Browser encoding is not the same, resulting in preview failure or garbled data.

**Solution**

You can select a solution for different reasons that cause chaos.

-   For the first reason, you must process the original data properly before starting the synchronization task.
-   For the second reason, you must modify the encoding format.
-   For the third reason, you must unify the encoding format before previewing the data.

