# Configure Redis data source {#concept_t3r_4tb_p2b .concept}

Redis is a document-based NoSQL database that provides persistent memory database services. Based on its highly reliable active and standby hot backup architecture and seamlessly scalable cluster architecture, this service can meet the needs of businesses that require high read and write performance and flexible capacity configuration. The Redis data source allows you to read and write data to Redis, and supports configuring synchronization tasks in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15483999637592_en-US.png)

4.  In the new data source dialog box, select the data source type **Redis**.
5.  Complete the configuration items for the Redis data source.

    The Redis data source type is categorized into **Alibaba Cloud database** and **public network IP address on-premise database**.

    -   Alibaba Cloud databases: These databases generally use classic networks. Classic networks can connect within the same region, but not in different regions.
    -   User‑created databases with public IP addresses: These databases generally use public networks, which may cause you to incur certain costs.
    Consider a data source that adds a new **Redis** \> **Alibaba Cloud database** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15483999637593_en-US.png)

    Configurations:

    -   Type: The currently selected data source type is Redis\> Alibaba cloud database.

        **Note:** If you have not already authorized the default role of the data integration system, you need the primary account to go to RAM to authorize the role then refresh the page.

    -   Name: A name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It is a combination of letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Region: Refers to the region selected when purchasing Redis.
    -   Redis instance ID: You can go to the Redis console to view the Redis instance ID.
    -   Redis access password: The access password for the Redis server, and does not enter if not.
    Consider a data source that adds a new **Redis** \> **Alibaba cloud database** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15483999637594_en-US.png)

    Configurations:

    -   Type: The currently selected data source type is Redis \> on-premise database with public network IP address.
    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Access address: The format is host:port.
    -   Add an access address: Add an access address in the format of host:port.
    -   Redis access password: Redis's service access password.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Next step { .section}

Now you have learned how to configure the Redis data source. The document explains how to configure the Redis writer-plug in later. For more information, see [Configure Redis Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure Redis Writer.md#).

