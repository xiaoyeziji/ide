# Billing method {#concept_b2c_3fv_42b .concept}

## How does Alibaba Cloud Data Integration bill users? {#section_e4p_kfv_42b .section}

The basic unit for data integration is Data Migration Unit \(DMU\) , which represents a single data integration unit, including CPU, memory, and network resource allocation.

A DMU represents the minimum amount of CPU, memory, and network resources used for data integration. A data integration task can run using single or multiple DMUs.

-   If the system resource group is used when your synchronization task runs, the billing is calculated as follows:

    ```
    Synchronization task billing= Number of DMUs configured for the task × Price of using a DMU for one hour × Time consumed by task execution
    ```

    Billing for using a DMU per hour is as follows:

    |Item|Price|
    |:---|:----|
    |DMU|$0.35 per hour|

-   If a custom resource group is used when running a synchronization task, the billing is calculated as follows:

    ```
    Synchronization task billing= Price of running a synchronization task for one hour × Number of hours consumed by task execution
    ```

    Billing for running a synchronization task for one hour is as follows:

    |Item |Price|
    |:----|:----|
    |Duration|$0.14 per hour|


**Note:** The time consumed by task execution is measured in minutes. The number of minutes consumed is rounded to the nearest integer.

Data integration service is free in all regions during the special discount period. During this period, you can view usage details and service usage records in **Billing Management** on the Alibaba Cloud console. We will notify you about billing.

## Does Alibaba Cloud Data Integration incur other costs? {#section_n4p_kfv_42b .section}

Data integration is independent from the data source from which data is read, and the target data source to which data is written. You need to pay upstream and downstream services related to the data source and target data source. For example, if you write data to the Object Storage Service \(OSS\), you need to pay for storage used. Check the billing details of the used storage product. In addition, Internet traffic costs may result from data transmission. These costs are excluded from data integration bills.

