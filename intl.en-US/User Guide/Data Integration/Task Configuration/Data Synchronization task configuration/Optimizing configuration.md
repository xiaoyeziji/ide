# Optimizing configuration {#concept_erz_fwc_p2b .concept}

Influence factors of the data synchronization speed, differences between speed-limited jobs and speed-not-limited jobs, precautions for custom resource groups, and how to adjust the DMU configuration and concurrent configuration of synchronization jobs for the maximum synchronization speed.

DataWorks Data Integration supports real-time, offline data interconnection between any data sources in any location and any network environment. It is a comprehensive full-stack data synchronization platform that allows you to copy dozens of TBs of data between various cloud and local data storage media.

The super fast data transmission performance and interconnection between over 400 pairs of heterogeneous data sources are the crucial factor that helps users focus on core big data issues only. The service can be used to design advanced analysis solutions with deep insight into all data.

## Factors affecting the speed of data synchronization {#section_rqx_rwc_p2b .section}

The factors that affect the speed of data synchronization are as follows.

-   Source-side data sources
    -   Database performance: The performance of the CPU, memory module, SSD, network, and hard disk.
    -   Concurrency: A high data source concurrency results in a high database workload.
    -   Network: The bandwidth \(throughput\) and speed of the network. Generally, a database with better performance can bear a higher concurrency. Therefore, the data synchronization job can be configured for high-concurrency data extraction.
-   Synchronous task configuration for data integration
    -   Synchronization speed: whether a limit is set for the synchronization speed.
    -   DMU: the amount of resources used for running the synchronization task.
    -   Concurrency: the maximum number of threads that can be used to read data from the data source, or write data to the target data source at the same time in one synchronization task.
    -   Wait resource.
    -   Bytes setting. If Bytes is set to 1048576, and the network is slow, the data transmission is timed out before it completes. We recommend that you set Bytes to a lower value.
    -   Whether to create an index for query statements.
-   Objective To end Data Source
    -   Performance: The performance of the CPU, memory module, SSD, network, and hard disk.
    -   Load: High database load, affecting data write efficiency.
    -   Network: The bandwidth \(throughput\) and speed of the network.

You need to monitor and optimize the performance, load, and network of the originating data source and destination databases. The following mainly describes how to set core configurations of a synchronization task on Data Integration.

## DMU {#section_rwn_lxc_p2b .section}

-   Configuration

    A data synchronization task can run using single or multiple DMUs. In Wizard mode, you can configure a maximum of 20 DMUs for a task. The following is an example of how to set the number of DMUs in Script mode:

    ```
    "Setting ":{
          "Speed ":{
            "dmu": 10
          }
        }
    ```

    **Note:** Note: If system performance is good, you can set the number of DMUs to more than 20 using a script. However, this may not improve system performance. Do not assign too many DMUs to a task.

-   Relationship between DMU and operation speed

    The DMU represents the resource capability, and the synchronization task is configured with a higher DMU, you can allocate more resources, but it does not mean that the speed of the synchronization task must be improved. Speed tuning requires combining concurrent, DMU ratio tuning between the two. For example, a synchronization task that configures 3 concurrency, requires 3dmu, and the synchronization speed is 10 Mb/s. At this time, the number of 3 Concurrent resources required is 3dmu, and the task does not need to use more resources, increasing the DMU does not, therefore, increase the speed of the synchronization task.


## Concurrency {#section_vw4_wxc_p2b .section}

-   Configuration

    In wizard mode, configure a concurrency for the specified task on the wizard page. The following is an example of configuring the number of concurrency with Script Mode.

    ```
    "Setting ":{
          "Speed ":{
            "concurrent": 10
          }
        }   }
    ```

-   Concurrent relationship with DMU

    A higher concurrency requires more DMUs. When network conditions and performance of data sources are good, more DMUs and higher concurrency will lead to better synchronization speed.

    -   To ensure that a task can be successfully executed at high concurrency, in Wizard mode, the highest concurrency allowed must not exceed the number of DMUs you set. For example, do not configure more than 10 concurrent threads when the number of DMUs is set to 10.
    -   When a high concurrency is set, you need to consider data source capabilities of reading and writing ends. Excessive concurrency may affect the performance of source database. Therefore, you need to tune the database.
    -   In Script mode you can set a high concurrency. However, the number of DMUs that can be provided for a task are limited. Do not set an excessively high concurrency.

## Speed Limit {#section_wc5_4yc_p2b .section}

After the beta phase of Data Integration has ended, throttling is disabled by default. In a synchronization task, data is synchronized at the maximum speed supported by the concurrency and DMUs configured for that task. Considering that excessively fast synchronization may overstress the database and thus affect the production, Data Integration allows you to limit the synchronization speed and optimize the configuration as required. It is recommended that the maximum speed should not exceed 30 MB/s when this option is enabled. The following is a sample example for configuring the speed limit in script mode, in which the transmission bandwidth is 1 MB/s:

```
"Setting ":{
      "Speed ":{
         "throttle": true // Throttling enabled.
        "mbps": 1,　// Synchronization speed
      }
    }
```

**Note:** 

-   Note: When the throttling parameter is set to false, throttling is disabled, and you do not need to configure the mbps parameter.
-   The traffic measured value is a Data Integration metric and does not represent the actual NIC traffic. Generally, the NIC traffic is two to three times of the channel traffic, which depends on the serialization of the data storage system.
-   A semi-structured Single file does not have the concept of cutting keys, multiple files can set the maximum job rate to increase the speed of synchronization, however, the maximum job rate is related to the number of files. For example, there are n files with maximum job rate limit set to nmb/s, if you set n + 1 Mb/s or sync at nmb/s speed, if set to n-1mb/s, synchronization is performed at n-1mb/s speed.
-   Only when a maximum job rate and a splitting key are configured for a relational database, table splitting can be performed according to the set maximum job rate. Relational databases only support numeric splitting keys, but Oracle databases support both numeric and string splitting keys.

## Cases of slow data synchronization {#section_tx3_zyc_p2b .section}

**Synchronization tasks remain in the waiting status when using public scheduling \(WAIT\) resources**

-   Related examples are as follows

    When you test the synchronization tasks in DataWorks, multiple tasks remain in the waiting status and an internal system error occurs.

    It takes 800 seconds to synchronize a task from RDS to MaxCompute using the default resource group. But the log shows that the task runs for only 18 seconds and stops. Other synchronization tasks with hundreds of data entries also remain in the waiting status.

    The waiting log is displayed as follows:

    ```
    2017-01-03 07: 16: 54: State: 2 (wait) | Total: 0r 0b | speed: 0r/s 0b/S | error: 0r 0b | stage: 0.0%
    ```

-   Solution

    In this case, public scheduling resources are used, whose capability is limited because they are shared by many projects but not only two or three tasks of a single user. A 10-second task was extended to 800 seconds because the required resources were insufficient and must be waited for when you ran the task.

    If you have strict requirements on the synchronization speed and the waiting time, we recommend starting the synchronization tasks in non-rush hours. Typically, synchronization tasks are concentrated between 00:00 and 03:00. You can perform synchronization tasks in other time apart from the aforesaid period to avoid resource waiting.


**Accelerate the tasks of synchronizing the data in multiple tables to the same table**

-   Related examples are as follows

    Synchronization tasks are serialized to synchronize the tables of multiple data sources to the same table, but the synchronization duration turns out to be a long one.

-   Solution

    To launch multiple tasks to write data to the same database at the same time, pay attention to the followings:

    -   Ensure that the load capacity of the destination database is sufficient to prevent improper running.
    -   When you configure workflow tasks, select a single task node and configure database or table sharding tasks, or set multiple nodes to run concurrently in the same workflow.
    -   If the synchronization tasks encounter resource waiting \(WAIT\) during running, run them in non-rush hours for a high execution priority.

**No index added while using the SQL WHERE clause**

-   Related examples are as follows

    The executed SQL statement is as follows:

    ```
    select bid,inviter,uid,createTime from `relatives` where createTime>='2016-10-2300:00:00'and reateTime<'2016-10-24 00:00:00';
    ```

    Query statement execution started at 2016-10-25 11:01:24.875 Beijing Time \(UTC+8\). Query result return started at 2016-10-25 11:11:05.489 Beijing Time \(UTC+8\). The synchronization program waited the database to return the SQL query result, and MaxCompute waited for a long time to start.

-   Cause analysis:

    When the where condition was executed, the createTime column was not indexed and full-table scanning was enforced.

-   Solution

    We recommend that you add an index to the column you want to scan if you want to use the SQL WHERE clause.


