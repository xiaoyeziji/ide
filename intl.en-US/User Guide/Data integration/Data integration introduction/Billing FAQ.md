# Billing FAQ {#concept_b2c_3fv_42b .concept}

## How does Alibaba Cloud Data Integration bill users? {#section_e4p_kfv_42b .section}

The basic unit of measurement for data integration is DMU \(Data Migration Unit\), representing the ability of a single unit in data integration \(including CPU, memory, network resource allocation\).

A DMU represents the minimum amount of CPU, memory, and network resources used for data integration. A data integration task can run using single or multiple DMUs.

-   If the system resource group is used when your synchronization task runs, the charge is calculated as follows:

    ```
    Charge for one synchronization task = Number of DMUs configured for the task × Price of using a single DMU for one hour × Time consumed by task execution
    ```

    The charge for using a single DMU for one hour is as follows:

    |Item|Price|
    |:---|:----|
    |DMU|$0.35 per hour|

-   If a custom resource group is used when your synchronization task runs, the charge is calculated as follows:

    ```
    Charge for one synchronization task = Price of running a synchronization task for one hour × Number of hours consumed by task execution
    ```

    The charge for running a synchronization task for one hour is as follows:

    |Item |Price|
    |:----|:----|
    |Duration|$0.14 per hour|


**Note:** The time consumed by task execution is measured in minutes. The number of minutes used is then rounded up to the nearest integer.

Data Integration service remains free for users in all regions. During this period, you can view your usage details and service usage records in **Billing Management** on the Alibaba Cloud console. We will inform you before the charging starts.

## Does Alibaba Cloud Data Integration incur other costs? {#section_n4p_kfv_42b .section}

Data Integration is independent from the data source from which data is read, and the target data source to which data is written. You need to pay for upstream and downstream services related to the data source and target data source. For example, if you write data to the object storage service \(OSS\), you need to pay for the storage used. Check billing details for the storage product you are using. In addition, Internet traffic costs may result due to data transmission. These costs are not included in data integration bills.

