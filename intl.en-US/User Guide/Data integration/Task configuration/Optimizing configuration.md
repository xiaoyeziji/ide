# Optimizing configuration {#concept_erz_fwc_p2b .concept}

This topic describes how to adjust DMU and concurrent configuration of synchronization jobs for optimized maximum synchronization speed. The data synchronization speed is influenced by factors, including differences between speed-limit jobs and not-speed-limit jobs, and precautions for custom resource groups.

DataWorks Data Integration supports real-time, offline data interconnection between any data sources in any location and network environment. It is a comprehensive full-stack data synchronization platform allows you to copy dozens of TBs data between various cloud and local data storage media.

The super fast data transmission performance and interconnection between more than 400 pairs of heterogeneous data sources that helps users focus on core big data issues. The service can be used to design advanced analysis solutions with deep insight in all data.

## Factors affecting data synchronization speed {#section_rqx_rwc_p2b .section}

The factors that affect data synchronization speed as follows.

-   Source-side data sources
    -   Database performance: The performance of CPU, memory module, SSD, network, and hard disk.
    -   Concurrency: A high data source concurrency results in a high database workload.
    -   Network: The bandwidth \(throughput\) and network speed. Typically, a database with better performance can tolerate a higher concurrency. Therefore, the data synchronization job can be configured for high-concurrency data extraction.
-   Synchronous task configuration for Data Integration
    -   Synchronization speed: Determines whether a synchronization speed limit is set.
    -   DMU: The resources used for running the synchronization task.
    -   Concurrency: The maximum number of threads that can be used to read/write data from the data source to the target data source at the same time in one synchronization task.
    -   The wait resource.
    -   Bytes setting: If the Bytes limit is set to 1,048,576, and the network is slow, the data transmission times out before completion. We recommend you set a lower Bytes value.
    -   Whether to create an index for query statements.
-   Objective to end Data Source
    -   Performance: The performance of the CPU, memory module, SSD, network, and hard disk.
    -   Load: The high database load that affects data write efficiency.
    -   Network: The bandwidth \(throughput\) and network speed.

You need to monitor and optimize the performance, load, and network of the original data source and destination databases. The following mainly describes how to set core configurations of a synchronization task in Data Integration.

## DMU {#section_rwn_lxc_p2b .section}

-   Configuration

    A data synchronization task can run with single or multiple DMUs. In Wizard mode, you can configure a maximum of 20 DMUs for a task. The following is an example of how to set the number of DMUs in Script mode:

    ```
    "Setting ":{
          "Speed ":{
            "dmu": 10
          }
        }
    ```

    **Note:** Although, you can configure more than 20 DMUs with a script, the system still has certain resource limitations. We recommend that you do not assign too many resources.

-   DMU writing synchronization speed factors

    The DMU represents the resource capability, and the synchronization task is configured with a higher DMU. You can allocate more resources without increasing the synchronization task speed. Speed optimization requires combining the concurrency with the DMU ratio. For example, a synchronization task that configures 3 concurrencies requires 3 DMU, and the synchronization speed is 10 Mb/s. Currently, the number of 3 concurrent resources required is 3 DMU, and the task does not require more resources. Increasing the DMU does not, therefore, increase the synchronization task speed.


## Concurrency {#section_vw4_wxc_p2b .section}

-   Configuration

    In Wizard Mode, configure a concurrency for the specified task on the wizard page. The following is an example of configuring the number of concurrency with Script Mode.

    ```
    "Setting ":{
          "Speed ":{
            "concurrent": 10
          }
        }   }
    ```

-   The relationship between concurrency and DMU

    A higher concurrency requires more DMUs. When network conditions and data source performances are good, more DMUs and higher concurrency will result in better synchronization speed.

    -   To ensure that a task in Wizard mode can be successfully executed in high concurrency , the maximum concurrency allowed cannot exceed the number of DMUs set. For example, we do not recommend that you configure more than 10 concurrency threads when the number of DMUs is set to 10.
    -   When a high concurrency is set, you need to consider data source capabilities in the reading and writing ends. Excessive concurrency may affect the source database performance. Therefore, you need to tune the database.
    -   In Script Mode you can set a high concurrency. However, the number of DMUs that are provided for a task are limited. Do not set an excessively high concurrency.

## Speed limit {#section_wc5_4yc_p2b .section}

By default, throttling is disabled after the Beta phase of Data Integration ends. In a synchronization task, data is synchronized at the maximum speed supported by the concurrency and DMUs configured for that task. Because extremely fast synchronization may overstress the database and affect production, Data Integration allows you to limit synchronization speed and optimize configuration as required. We recommend that the maximum configured speed limit cannot exceed 30 MB/s when this option is enabled. The following is an example for configuring the speed limit in Script Mode, when the transmission bandwidth is 1 MB/s:

```
"Setting ":{
      "Speed ":{
         "throttle": true // Throttling enabled.
        "mbps": 1,　// Synchronization speed
      }
    }
```

**Note:** 

-   Throttling is disabled when it is set to False. You do not need to configure the mbps parameter.
-   The traffic measured value is a Data Integration metric that does not represent actual NIC traffic. Generally, the NIC traffic is two to three times that of the channel traffic, which depends on the serialization of data storage system.
-   A semi-structured Single file does not have shard key concept. Multiple files can set the maximum job rate to increase the synchronization speed, however, the maximum job rate is related to the number of files. For example, there are n files with maximum job rate limit set to n Mb/s, then if you set n + 1 Mb/s or sync at n Mb/s speed. If you set the speed to n-1 Mb/s, then the synchronization is performed at n-1 Mb/s speed.
-   The table splitting can be performed at the set maximum job rate, only when a maximum job rate and a shard key are configured for a relational database. Relational databases only supports numeric shard keys, but Oracle databases support both Numeric and String shard keys.

## Cases of slow data synchronization {#section_tx3_zyc_p2b .section}

**Synchronization tasks remain in the waiting status when using public scheduling \(WAIT\) resources**

-   Related examples are as follows

    When you test synchronization tasks in DataWorks, multiple tasks remain in the waiting status and an internal system error occurs.

    It takes 800 seconds to synchronize a task from RDS to MaxCompute using default resource groups, but the log shows that the task runs for only 18 seconds and stops. Other synchronization tasks with hundreds of data entries also remain in the waiting status.

    The waiting log is displayed as follows:

    ```
    2017-01-03 07: 16: 54: State: 2 (wait) | Total: 0r 0b | speed: 0r/s 0b/S | error: 0r 0b | stage: 0.0%
    ```

-   Solution

    In this case, public scheduling resources are used. The capability is limited because they share many projects instead of two or three tasks of a single user. A 10-second task is extended to 800 seconds because the required resources were insufficient and must be waited when you run the task.

    If you have strict requirements for synchronization speed and waiting time, we recommend starting synchronization tasks during non-busy hours. Typically, synchronization tasks are concentrated between 00:00 and 03:00. You can perform synchronization tasks in other time except from the aforesaid period to avoid resource waiting.


**Accelerate tasks of synchronizing data in multiple tables to the same table**

-   Related examples are as follows:

    Synchronization tasks are serialized to synchronize the tables of multiple data sources to the same table, but the synchronization duration can take a long time.

-   Solution

    To start multiple write data tasks in the same database simultaneously, pay attention to the following:

    -   Ensure the load capacity of the destination database is sufficient to prevent improper runs.
    -   When you configure workflow tasks. Select a single task node and configure database or table shard tasks, or set multiple nodes to run concurrently in the same workflow.
    -   If the synchronization tasks encounter resource waiting \(WAIT\) during runs, run them during non-rush hours for high execution priority.

**No index added while using the SQL WHERE clause**

-   Related examples are as follows:

    The executed SQL statement as follows:

    ```
    select bid,inviter,uid,createTime from `relatives` where createTime>='2016-10-2300:00:00'and reateTime<'2016-10-24 00:00:00';
    ```

    If the query statement execution started at 2016-10-25 11:01:24.875 Beijing Time \(UTC+8\), then the return query result started at 2016-10-25 11:11:05.489 Beijing Time \(UTC+8\). The synchronization program waited the database to return the SQL query result, and MaxCompute waited for a long time to start.

-   Cause analysis:

    When the WHERE statement was executed, the createTime column was not indexed and full-table scanning was enforced.

-   Solution

    We recommend that you add an index to the scan column, if you want to use the SQL WHERE clause.


