# How to troubleshoot data integration problems? {#concept_bnt_kkb_r2b .concept}

If any problem arises during Data Integration operations, you must identify the relevant information, such as: on what server the tasks are run, the information on data sources, and the region in which the synchronization tasks are configured.

## The server performing the tasks {#section_bhk_mhb_r2b .section}

-   running on Alibaba's server:

      running in Pipeline\[basecommon\_ group\_xxxxxxxxx\]

-   running on your server:

    running in Pipeline\[basecommon\_xxxxxxxxx\]


## Information on data sources {#section_v3x_shb_r2b .section}

When Data Integration fails, you must review the information on data sources:

-   check the data sources among which the synchronization tasks are run.

-   check the environment of the data sources.

    For example: Alibaba Cloud database, data sources with/without public IPs or VPC network environment \(RDS and other sources\), Financial Cloud \(VPC and classic network\).

-   check if the connectivity test of data source is successful.

    Compare against the Data Source Configuration document: check if the information on data source is filled up incorrectly \(typical situations include mixing up multiple databases, adding spaces or special characters when filling up the information, or the connectivity test is not supported \(data source from database without public IPs or a VPC environment except RDS\)\).


## Check the region in which the synchronization tasks are configured {#section_dfl_snb_r2b .section}

You can see the related regions in the DataWorks console, such as East China 2, North China 1, Hong Kong, Southeast Asia Pacific 1, Central Europe 1, and Southeast Asia Pacific 2. Generally, the default region is the East China 2. You can see the corresponding region after purchasing the MaxCompute, as shown in the following figure:

![](images/8678_en-US.png)

## Copy the troubleshooting code when interface pattern errors are reported {#section_n3w_ynb_r2b .section}

When interface pattern errors are reported, copy the troubleshooting code for relevant personnel, as shown in the following figure:

![](images/8679_en-US.jpg)

## The log reports exceptions {#section_i2l_g4b_r2b .section}

**The log reports an error occurred while running the SQL statement \(the column contains the keyword\)**

```
2017-05-31 14:15:20.282 [33881049-0-0-reader] ERROR ReaderRunner - Reader runner Received Exceptions:com.alibaba.datax.common.exception.DataXException: Code:[DBUtilErrorCode-07]
```

Error Details:

Failed to read database data. Check your column/table/where/querySql configuration or ask DBA for help.

The executed SQL statement is as follows:

```
select **index**,plaid,plarm,fget,fot,havm,coer,ines,oumes from xxx
```

The error details are shown as follows:

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL Server version for the right syntax to use near Index, plaid, plarm, fget, fot, havm, coer, Ines, oums from XXX
```

Troubleshooting:

-   Then, run another SQL statement:

```
select **index**,plaid,plarm,fget,fot,havm,coer,ines,oumes from
          xxx
```

If you look at the results, there will also be corresponding errors.

-   If the field contains the keyword index, you can add single quotes or modify the field to resolve the problem.


**The log reports that an error occurred while running the SQL statement \(the table name is in single quotes within double quotes\)**

```
 
com.alibaba.datax.common.exception.DataXException: Code:[DBUtilErrorCode-07]
```

Error Details:

Failed to read database data. Check your column/table/where/querySql configuration or ask DBA for help.

The executed SQL statement is as follows:

```
select /_+read_consistency(weak) query_timeout(100000000)_/ _ from** 'ql_ddddd_[0-31]’ **where 1=2
```

The error details are shown as follows:

```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ‘‘ql_live_speaks[0-31]’ where 1=2’ at line 1 - com.mysql.jdbc.exceptions.jdbc4. Mysqlsyntaxerrorexception: You have an error in your SQL syntax; check the manual that corresponds to your MySQL Server version for the right syntax to use near **' 'ql _ ddddd _ [0-31] 'where 1 = 2 '**
```

Troubleshooting

If the table name is in single quotes within double quotes, you can delete the single quotes directly in the configuration constant "table":\["'ql*ddddd*\[0-31\]'"\].

**Connectivity test of data source fails \(The exception message "Access denied for..." is reported\)**

An error occurred while connecting to the database. Database connection string: jdbc:mysql://xx.xx.xx.x:3306/t\_demo. User name: fn\_test. Exception message: Access denied for user 'fn\_test’@’%’ to database 't\_demo'. Make sure you have added a whitelist in RDS.

Troubleshooting:

-   When the exception message Access denied for... is reported, it generally indicates certain problems of the information you entered. Check that information.

-   Check whether the whitelist or your account has the permission to access the database. You can add the required whitelist and permissions in the RDS console.


**The routing policy has some problems. The running pool are OXS and ECS clusters.**

```
 
2017-08-08 15:58:55 : Start Job[xxxxxxx], traceId **running in Pipeline[basecommon_group_xxx_cdp_oxs]**ErrorMessage:Code:[DBUtilErrorCode-10]
```

Error Details:

An error occurred while connecting to the database. Check your account, password, database name, IP address and port or ask DBA for help \(note the network environment\). An error occurred while connecting to the database, because no connecting JDBC URL can be found from jdbc:oracle:thin:@xxx.xxxxx.x.xx:xxxx:prod. Check and modify your configurations. Check your configurations and make changes.

The error message "java.lang.Exception: DataX" indicates that the corresponding database cannot be connected for the following reasons:

-   the IP/port/database/JDBC you configured is incorrect and cannot be connected.

-   the user name/password you configured is incorrect, and authentication is unsuccessful. Confirm with DBA whether the connection information of the database is correct.


Troubleshooting:

Scenario 1:

-   To synchronize RDS-PostgreSQL data sources from Oracle, you can click **Run** directly. The tasks cannot be performed by the scheduler, because different pools are required.

-   You can add data sources in the form of JDBC to RDS, then the RDS-PostgreSQL data sources can be synchronized from Oracle.


Scenario 2:

-   RDS-PostgreSQL data sources in VPC environment cannot run on a custom source group. The RDS in VPC environment provides reverse proxy capability, leading to network problems for the custom resource group. Therefore, RDS in VPC environment can directly run on Alibaba's server. If our server cannot meet your requirements, and you want to run tasks on your server, you must add data source in the form of JDBC to RDS in VPC environment and purchase the ECS in the same network segment.

-   - The "jdbc:mysql://100.100.70.1:4309/xxx,100" mapped out by the RDS in VPC environment often begins with an IP mapped out by the background. If it begins with an domain, the RDS is not in a VPC environment.

HBase Writer does not support the Date type

Hbase synchronization to hbase: 2017-08-15 11: 19: 29: State: 4 \(fail\) | Total: 0r 0b | speed: 0r/s 0b/S | error: 0r 0b | stage: 0.0% errormessage: Code: \[fig\]

Error Details:

The value of the parameter you entered is invalid.

**Hbase writer does not support this type: Date**. The types currently supported are: \[string，boolean，short，int，long，float，double\].

Troubleshooting:

-   HBase writer does not support the Date type. You cannot configure any data in the type of Date in the writer.

-   You can directly configure the data in string type, because HBase has no limit in terms of data type. The bottom layer of the HBase is generally the byte array.


**JSON format configuration error**

Column configuration error

Based on the analysis by DataX, the most likely cause of this error is as follows:

`com.alibaba.datax.common.exception.DataXException: Code:[Framework-02]`

Error Details:

The DataX engine encountered an error when running. For details, see the error diagnostic information after DataX stops running

java.lang.ClassCastException：com.alibaba.fastjson. Jsonobject cannot be cast to  java.lang.String

Troubleshooting:

JSON is configured improperly.

```

Writer: 
"column":[ 
{ 
"name":"busino", 
"type"": "string" 
} 
] 
Write the statement as follows: 
"column":[ 
{ 
"Busino" 
} 
]
```

-   The JSON list is written less \[\]


In using smart analysis of DataX, the most likely reason for error is:

`com.alibaba.datax.common.exception.DataXException: Code:[Framework-02]`

Error Details:

The DataX engine encountered an error when running. For details, see the error diagnostic information after DataX stops running

```
java.lang.String cannot be cast to java.util.List - java.lang.String cannot be cast to java.util.List  
at com.alibaba.datax.common.exception.DataXException.asDataXException(DataXException.java:41)
```

Troubleshooting:

When \[\] is missing, the list type is changed. You can resolve this by finding where the is missing and adding the.

**Permission issues**

-   Permission issues \(no permission for "delete" operation\)

For synchronization from MaxCompute to RDS-MySQL, the error message is: Code:DBUtilErrorCode-07

Error Details:

Failed to read database data. Check your column/table/where/querySql configuration or ask DBA for help.

The executed SQL statement is as follows:

`delete from fact_xxx_d where sy_date=20170903`

The error details are shown as follows:

```
**DELETE command denied** to user ‘xxx_odps’@‘[xx.xxx.xxx.xxx](http://xx.xxx.xxx.xxx)’ for table ‘fact_xxx_d’ - com.mysql.jdbc.exceptions.jdbc4. MySQLSyntaxErrorException: DELETE command denied to user ‘xxx_odps’@‘[xx.xxx.xxx.xxx](http://xx.xxx.xxx.xxx)’ for table 'fact_xxx_d’
```

-   Troubleshooting:

    The error message "DELETE command denied to" indicates that you have no permission to delete the table, and you must grant the permission required in the corresponding database.

-   Permission issues \(no permission for "drop" operation\)

    Code:DBUtilErrorCode-07

    Error Details:

    Failed to read database data. Check your column/table/where/querySql configuration or ask DBA for help.

    The SQL you run is: truncate table be\_xx\_ch

    The error details are shown as follows:

    ```
    **DROP command denied to user** ‘xxx’@‘[xxx.xx.xxx.xxx](http://xxx.xx.xxx.xxx)’ for table ‘be_xx_ch’ - com.mysql.jdbc.exceptions.jdbc4. MySQLSyntaxErrorException: DROP command denied to user ‘xxx’@‘[xxx.xx.xxx.xxx](http://xxx.xx.xxx.xxx)’ for table 'be_xx_ch’
    ```


Troubleshooting:

The preceding error is reported when the prepared statement "truncate" before MySQLWriter configuration execution is performed to delete the table data, because you have no permission for "drop" operation.

**ADS permission issues**

```
2016-11-04 19:49:11.504 [job-12485292] INFO OriginalConfPretreatmentUtil - Available jdbcUrl:jdbc:mysql://100.98.249.103:3306/ads_rdb? yearIsDateType=false&zeroDateTimeBehavior=convertToNull&tinyInt1isBit=false&rewriteBatchedStatements=true.  
2016-11-04 19:49:11. 505 [job-12485292] warn maid
```

There is a certain risk of column configuration in your configuration file. Because you do not have columns configured to read database tables, when there is a change in the number and type of your table fields, may affect task correctness or even run errors. Check your configurations and make changes.

`2016-11-04 19:49:11.528 [job-12485292] INFO Writer$Job`

If it is MaxCompute \> ADS data synchronization, you must complete the following authorizations:

-   The ADS official account must have at least the "describe" and "select" permissions for the tables to be synchronized, because the ADS system requires the structure and data information of the table to be synchronized from MaxCompute.

-   The account AK you configured to access the ADS data source must have the permission to initiate a request to load data to the specified ADS database. You can add the authorization in the ADS system.


`2016-11-04 19:49:11.528 [job-12485292] INFO Writer$Job`

If it is the data synchronization between RDS \(or other non-MaxCompute data sources\) and ADS, the implementation logic is to first load the data to the MaxCompute temporary table, and then synchronize data from MaxCompute temporary table to ADS \( set temporary MaxCompute project as cdp\_ads\_project, and set the temporary project account as cloud- data-pipeline@aliyun-inner.com\).

Permissions:

-   The ADS official account must have at least the "describe" and "select" permissions for the tables \(MaxCompute temporary table\) to be synchronized, because the ADS system requires the structure and data information of the table to be synchronized from MaxCompute \(the authorization has been completed at deployment\).

-   The account cloud-data-pipeline@aliyun-inner.com of temporary MaxCompute must have the permission to initiate a request to load data to the specified ADS database. You can add the authorization in the ADS system.


Troubleshooting:

This problem is due to the lack of permission to load data.

The temporary project account is cloud-data-pipeline@aliyun-inner.com. ADS official account must have at least the "describe" and "select" permissions for the tables \(MaxCompute temporary table\) to be synchronized, because the ADS system requires the structure and data information of the table to be synchronized from MaxCompute \(the authorization has been completed at deployment\). Log on to the ADS console and grant the "load data" permission to the ADS.

**Whitelist issues**

-   The whitelist has not been added and the connectivity test of data source fails.

Test connection failed. Connectivity test of data source failed:

```
error message: Timed out after 5000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}. Client view of cluster state is {type=UNKNOWN, servers=[{[address:3717=dds-bp1afbf47fc7e8e41.mongodb.rds.aliyuncs.com](http://address:3717=dds-bp1afbf47fc7e8e41.mongodb.rds.aliyuncs.com), type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}, {[address:3717=dds-bp1afbf47fc7e8e42.mongodb.rds.aliyuncs.com](http://address:3717=dds-bp1afbf47fc7e8e42.mongodb.rds.aliyuncs.com), type=UNKNOWN, state=CONNECTING,** exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream**}}]
```

Troubleshooting

When adding data source to MongoDB in non-VPC environment, if the error message Timed out after 5000 is reported, it means that the whitelist has a problem.

**Note:** If you are using ApsaraDB for MongoDB, a root account is provided by default. To ensure security, Data Integration only supports using the relevant account of MongoDB for connection. Avoid using root account as the access account when adding and using the MongoDB data source.

-   White List not complete

for Code:\[DBUtilErrorCode-10\]

Error Details:

An error occurred while connecting to the database. Check your account, password, database name, IP address and port or ask DBA for help \(note the network environment\).

The error details are shown as follows:

```
java.sql.SQLException: Invalid authorization specification, message from server: "#**28000ip not in whitelist, client ip is xx.xx.xx.xx". **  
2017-10-18 11:03:00. 673 [job-Newfoundland] Error retryutil-exception when calling callable
```

Troubleshooting:

The whitelist you added is incomplete. You has not added your server into the whitelist.

**The data source information is incorrect**

-   When configuring the script mode, the corresponding data source information \(could not be blank\) is missing.

    ```
    2017-09-06 12:47:05 INFO Success to fetch meta data for table with projectId 43501 project ID and instance ID mongodbdata source name. **  
     2017-09-06 12:47:05 [INFO] Data transport tunnel is CDP.  
    2017-09-06 12:47:05 [INFO] Begin to fetch alisa account info for 3DES encrypt with parameter account: [zz_683cdbcefba143b7b709067b362d4385].  
     
    2017-09-06 12:47:05 [INFO] Begin to fetch alisa account info for 3DES encrypt with parameter account: [zz_683cdbcefba143b7b709067b362d4385].  
    [Error] exception when running task, message: ** configuration property [adord] is generally the information to be filled in by ODPS data source could not be blank! **
    ```

    Troubleshooting:

    The error message shows that the corresponding accessId information is blank. This is generally due to script mode issues. Check the JSON code you configured to see whether the corresponding data source name is missing.

-   Data source is not configured

    ```
    
     
    2017-10-10 10:30:08 INFO =================================================== 
     
    File “/home/admin/synccenter/src/Validate.py”, line 16, in notNone 
    raise Exception(“Configuration property [%s] could not be blank!” % (Context )) 
    ** Exception: configuration property [username] could not be blank! **
    ```

    Troubleshooting:

    -   Check with the normal logs:

        ```
        
        [56810] and instanceId(instanceName) [spfee_test_mysql]… 
        2017-10-09 21:09:44 [INFO] Success to fetch meta data for table with projectId [56810] and instanceId [spfee_test_mysql].
        ```

    -   Generally, such information shows that an error occurred while calling the data source. If the empty user name is reported, it shows that the data source has not been configured or the location of data source has not been configured correctly. In this case, the user has configured an incorrect position of the data source.
-   DRDS data connection time-out

    When synchronizing data from MaxCompute to DRDS, the following errors often appear:

    ```
    [2017-09-11 16:17:01. 729 [49892464-0-0-writer] warn maid $ task
    ```

    Roll back the data written this time and write a single row of data each time and submit again. The reasons are as follows:

    ```
     
    com.mysql.jdbc.exceptions.jdbc4.  CommunicationsException: **Communications link failure **   
    The last packet successfully received from the server was 529 milliseconds ago.   
    The last packet sent successfully to the server was** 528 milliseconds ago**.
    ```

    ![](images/8686_en-US.png)

    Troubleshooting:

    Datax client timeouts can be added when adding DRDs data sources `? useUnicode=true&characterEncoding=utf-8&socketTimeout=3600000` timeout Parameter

    Example:

    ```
    jdbc:mysql://10.183.80.46:3307/ae_coupon? useUnicode=true&characterEncoding=utf-8&socketTimeout=3600000
    ```

-   System internal problems

    ![](images/8687_en-US.png)

    Troubleshooting:

    Generally, system internal problems are reported when the data source in JSON format is mistakenly modified and saved in the development environment. When the page is blank, you can directly provide the project name and the node name to us for background processing.


**Dirty data**

-   Dirty data \(the string \[""\] cannot be converted to long\)

    ```
    2017-09-21 16:25:46.125 [51659198-0-26-writer] ERROR WriterRunner - Writer Runner Received Exceptions:  
    com.alibaba.datax.common.exception.DataXException: Code:[Common-01]
    ```

    Error Details:

    The business dirty data generated during data synchronization is caused by incorrect data type conversion. The string \[""\] cannot be converted to long.

    Troubleshooting:

    The String \[""\] cannot be converted to long: The statements for table creation in two tables are the same. The preceding error is reported because the field type empty cannot be converted to long. You can directly configure it as a string.

-   Dirty data \(out of range value\)

    ```
    2017-11-07 13:58:33.897 [503-0-0-writer] ERROR StdoutPluginCollector 
    Dirty data: 
    {"exception":"Data truncation:Out of range value for column 'id' at row 1","record":{"byteSize":2,"index":0,"rawData":-3,"type":"LONG"},{"byteSize":2,"index":1,"rawData":-2,"type":"LONG"},{"byteSize":2,"index":2,"rawData":"other","type":"STRING"},{"byteSize":2,"index":3,"rawData":"other","type":"STRING"},"type":"writer"}
    ```

    Troubleshooting:

    The source data type of mysql2mysql is set as smallint\(5\) and the target data type is int\(11\) unsigned. Because the data in the type of smallint\(5\) contains negative number, and the data in the type of unsigned cannot be negative, the dirty data is generated.

-   Dirty data \(storing emoj\)

    The data table is configured to store emoj, and dirty data is reported during data synchronization.

    Troubleshooting:

    Data integration is supported by default by utt 8, so when you add a data source in JDBC format, you need to modify your settings, such jdbc:mysql://xxx.x.x.x:3306/database? characterEncoding=utf8&com.mysql.jdbc.faultInjection.serverCharsetIndex=45, so that you can set the emotability on the data source to synchronize successfully.

-   Dirty data caused by empty fields

    ```
     {“exception”:“Column ‘xxx_id’ cannot be null”,“record”:[{“byteSize”:0,“index”:0,“type”:“LONG”},{“byteSize”:8,“index”:1,“rawData”:-1,“type”:“LONG”},{“byteSize”:8,“index”:2,“rawData”:641,“type”:“LONG”}
    ```

    Based on the analysis by DataX, the most likely cause of this error is as follows:

    com.alibaba.datax.common.exception.DataXException: Code:\[Framework-14\]

    Error Details:

    The dirty data transmitted by DataX exceeds user expectations. This error often occurs when a lot of dirty business data exists within the source data. Please check carefully the dirty data log information reported by DataX, or adjust the dirty data threshold accordingly.

    The check on the number of dirty data entries failed. The number of dirty data entries is limited to 1, but seven are captured.

    Troubleshooting:

    The dirty data is generated because the field "column 'xxx\_id' cannot be null" cannot be empty, and empty data is used during data synchronization. You can modify those empty data, or modify the field.

-   The field "data too long for column 'flash'" is too short and the dirty data is generated.

    ```
     2017-01-02 17:01:19.308 [16963484-0-0-writer] ERROR StdoutPluginCollector 
    Dirty data:  
    {"exception": "Data updatation: data Too long for column 'Flash 'at Row 1, "record ": [{"bytesize": 8, "Index": 0, "rawdata": 1, "type": "long"}, {"bytesize": 8, "Index ": 3, "rawdata": 2, "type": "long "}, {"bytesize": 8, "Index": 4, "rawdata": 1, "type": "long"}, {"bytesize": 8, "Index ": 5, "rawdata": 1, "type": "long "}, {"bytesize": 8, "Index": 6, "rawdata": 1, type: "Long "}
    ```

    Troubleshooting:

    The field "data too long for column 'flash'" is too short, but the data that you synchronized is too long. Therefore, the dirty data is generated. You can modify the data, or the field.

-   Read-only permission to database settings

    ```
    2017-11-07 13:58:33.897 503-0-0-writer ERROR StdoutPluginCollector 
    Dirty data:  
    {"exception": "the MySQL server is running with the -- read-only option so it cannot execute this statement", "record": [{"bytesize": 3, "Index": 0, rawdata: 201, type: Long}, {bytesize ": 8, "Index": 1, "rawdata": 1474603200000, "type ": "date"}, {"bytesize": 8, "Index": 2, rawdata: September 23, "12", "type": "string "}, {"bytesize": 5, "Index": 3, "rawdata ": "12", "type": "string"}
    ```

    Troubleshooting:

    When read-only mode is set, if all the data to be synchronized is dirty data, you can change the "read-only" mode of the database into "writable" mode.

-   Logs generated when partition error occurs

    An error message is reported when the parameter is configured as $yyyymm. The log is generated as follows:

    `[2016-09-13 17:00:43] 2016-09-13 16:21:35. 689 [job-10055875] Error Engine`

    Based on the analysis by DataX, the most likely cause of this error is as follows:

    `com.alibaba.datax.common.exception.DataXException: Code:[OdpsWriter-13]`

    Error Details:

    If an exception occurs while running MaxCompute SQL, you can try again. If the MaxCompute target table throws an exception when executing MaxCompute SQL, contact the MaxCompute administrator. The content of SQL is as follows:

    ```
    alter table db_rich_gift_record add IF NOT EXISTS
          partition(pt=’${thismonth}’);
    ```

    Troubleshooting:

    The single quotes added leads to invalid scheduling parameter replacing. Solution: remove the single quotes of '$\{thismonth\}'.

-   column is not configured as the array form

    ```
    Run Command failed.  
    com.alibaba.cdp.sdk.exception.CDPException: com.alibaba.fastjson.JSONException: syntax error, **expect {,** actual error, pos 0  
    at com.alibaba.cdp.sdk.exception.CDPException.asCDPException(CDPException.java:23)
    ```

    Troubleshooting:

    The JSON has the following problem:

    ```
    
    "plugin": “mysql”,** 
    "parameter":{ 
    "Datasource": "XXXXX", 
    ** “column”: “uid”,** 
     “where”: “”, 
    “splitPk”: “”, 
     “table”: “xxx” 
    } 
    “column”: “uid”,-----has not been configured as the array form
    ```

-   JDBC formatting error

    ![](images/8688_en-US.png)

    Troubleshooting:

    The JDBC format is incorrect. The correct format is: jdbc:mysql://ServerIP:Port/Database.

-   Test connectivity failed

    ![](images/8692_en-US.png)

    Troubleshooting:

    -   Check whether the firewall limits the IP and port used by your account.

    -   Check the port development of the security group.

-   uid\[xxxxxxxx\] is reported in the logs

    ```
    Run Command failed.  
    com.alibaba.cdp.sdk.exception.CDPException: RequestId[F9FD049B-xxxx-xxxx-xxx-xxxx] Error: there was an exception in the network information for the obtained instance, please check the RDS buyer ID and the RDS Instance name, UID [Newfoundland], instance [rm-bp1cwz5886rmzio92] serviceunavailable: the request has failed due to a maid failure of the server.  
    RequestIdF9FD049B-xxxx-xxxx-xxx-xxxx Error:
    ```

    Troubleshooting:

    Generally, when synchronizing data from RDS to MaxCompute, if the preceding error is reported, you can directly copy the RequestId:F9FD049B-xxxx-xxxx-xxx-xxxx to the RDS personnel.

-   - The query parameter in MongoDB is incorrect

    When the following error is reported as synchronizing data from MongoDB to MySQL, if you find that it is caused by incorrect JSON, it means that the JSON query parameter is not configured properly.

    ```
    Exception in thread “taskGroup-0” com.alibaba.datax.common.exception.DataXException: Code:[Framework-13]
    ```

    Error Details:

    The DataX plug-in encountered an error while running. For the specific causes, refer to the error diagnostic information after DataX stops running.

    ```
     org.bson.json.JsonParseException: Invalid JSON input. Position: 34. Character :'.'.
    ```

    Troubleshooting:

    -   Negative example: "query":"\{‘update\_date’:\{’$gte’:new Date\(\).valueOf\(\)/1000\}\}". The parameter in the form of "new Date\(\) " is not supported.

    -   Correct example: "query":"\{‘operationTime’\{’$gte’:ISODate\(’$\{last\_day\}T00:00:00.424+0800’\)\}\}"

-   Cannot allocate memory

    ```
     
    2017-10-11 20:45:46.544 [taskGroup-0] INFO TaskGroupContainer - taskGroup[0] taskId[358] attemptCount[1] is started   
    Java HotSpot™ 64-Bit Server VM warning: INFO: os::commit_memory(0x00007f15ceaeb000, 12288, 0) failed; error=‘**Cannot allocate memory’** (errno=12)
    ```

    Troubleshooting:

    The memory is insufficient. If it occurs on your server, you must add extra memory; if it occurs on Alibaba's server, directly contact the technical support personnel.

-   max\_allowed\_packet parameter

    The error details are shown as follows:

    ```
    Packet for query is too large (70>-1 ). You can change this value on the server by setting the max_allowed_packet’ variable. **com.mysql.jdbc.PacketTooBigException: Packet for query is too large (70 > -1). You can change this value on the server by setting the max_allowed_packet’ variable. **
    ```

    Troubleshooting:

    -   The max\_allowed\_packet parameter is used to define the maximum length of the communication buffer. MySQL may limit the size of the data packets received by the server based on the configuration file. Sometimes, insertions and updates in large size may fail due to the limitation of the max\_allowed\_packet parameter.
-   If the value of Max\_allowed\_packet parameter is too large, you can change it into a smaller one. 10 MB = 10\_1024\_1024.
-   "HTTP Status 500" is reported and an error occurred while reading the logs.

    ```
    Unexpected Error:  
    Response is com.alibaba.cdp.sdk.util.http.Response@382db087[proxy=HTTP/1.1 500 Internal Server Error [Server: Tengine, Date: Fri, 27 Oct 2017 16:43:34 GMT, Content-Type: text/html;charset=utf-8, Transfer-Encoding: chunked, Connection: close,  
    **HTTP Status 500** - Read timed out**type** Exception report**message**++Read timed out++**description**++The server encountered an internal error that prevented it from fulfilling this request.++**exception**  
    java.net.SocketTimeoutException: Read timed out
    ```

    Troubleshooting:

    When "HTTP Status 500" is reported while your tasks are running, if an error occurred during log reading of the tasks running on Alibaba's server, contact technical support personnel. If you are running on tasks on your own server, restart the Alisa.

    **Note:** If the service status remains Stopped after the refreshing, restart the following alisa command to switch to the admin account: /home/admin/alisatatasknode/target/alisatatasknode/bin/serverct1 restart.

-   hbasewriter parameter: hbase.zookeeper.quorum configuration error

    ```
    2017-11-08 09:29:28.173 [61401062-0-0-writer] INFO ZooKeeper - Initiating client connection, connectString=xxx-2:2181,xxx-4:2181,xxx-5:2181,xxxx-3:2181,xxx-6:2181 sessionTimeout=90000 watcher=hconnection-0x528825f50x0, quorum=node-2:2181,node-4:2181,node-5:2181,node-3:2181,node-6:2181, baseZNode=/hbase  
    Nov 08, 2017 9:29:28 AM org.apache.hadoop.hbase.zookeeper.RecoverableZooKeeper checkZk  
    WARNING: **Unable to create ZooKeeper Connection**
    ```

    Troubleshooting:

    -   Error example: "hbase. zoolokeeper. quorum: "xxx-2, xxx-4, xxx-5, xxxx-3, xxx-6"

    -   "Hbase.zookeeper.quorum":"your zookeeper IP address"

-   No relevant files are found

    Based on the analysis by DataX, the most likely cause of this error is as follows:

    com.alibaba.datax.common.exception.DataXException: Code:\[HdfsReader-08\]

    Error Details:

    The directory of the file you are trying to read is empty. Failed to locate the file to be read, check your configuration items.

    ```
    Path:/user/hive/warehouse/maid /*  
    at com.alibaba.datax.common.exception.DataXException.asDataXException(DataXException.java:41)
    ```

    Troubleshooting:

    Find the corresponding location using the path to check the corresponding file. If the file is not found, perform the necessary operations on the file.

-   Table doesn't exist

    Based on the analysis by DataX, the most likely cause of this error is as follows:

    com.alibaba.datax.common.exception.DataXException: Code:\[MYSQLErrCode-04\]

    Error Details:

    The table does not exist. Check the table name or contact DBA to confirm whether the table exists.

    Table name: xxxx.

    The SQL executed is: `Select * from Newfoundland where 1 = 2;`

    The error details are shown as follows:

    ```
    Table ‘darkseer-test.xxxx’ doesn’t exist - com.mysql.jdbc.exceptions.jdbc4. MySQLSyntaxErrorException: Table ‘darkseer-test.xxxx’ doesn’t exist
    ```

    Troubleshooting:

    select \* from xxxx where 1=2 and check if the table xxxx has a problem. Take appropriate actions if any problem exists.


