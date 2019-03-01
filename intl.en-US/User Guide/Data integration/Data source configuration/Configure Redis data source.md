# Configure Redis data source {#concept_t3r_4tb_p2b .concept}

This topic describes how to configure a Redis data source. Redis is a document-based NoSQL database that provides persistent memory database services. Based on its highly reliable active/standby hot backup architecture and seamlessly scalable cluster architecture, this service can meet high read/write performance and flexible capacity configuration requirements of businesses. The Redis data source allows you to read/write data to Redis, and supports configuring synchronization tasks in Script Mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15514310817592_en-US.png)

4.  Select the data source type **Redis** in the new dialog box.
5.  Complete the Redis data source configuration items.

    The Redis data source type is categorized into **ApsaraDB for RDS** and **Public Network IP Address On-Premise Database**.

    -   ApsaraDB for RDS: These databases generally use classic networks. You cannot connect cross-region classic networks, only networks in the same region can connect..
    -   User‑created databases with public IP addresses: Generally, these databases use public networks, which may cause you to incur certain costs.
    The following figure is an example of adding a**Redis** \> **ApsaraDB RDS** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15514310827593_en-US.png)

    Configurations:

    -   Type: Currently, the selected data source type is Redis\> Apsara DB RDS.

        **Note:** If you have not authorized the default role of the Data Integration system you can authorize the role by logging onto RAM using the primary account and then refresh the page.

    -   Name: A name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   Region: The region you selected when purchasing Redis.
    -   Redis instance ID: You can go to the Redis console to view the Redis instance ID.
    -   Redis access password: The Redis Server access password. This field can be left blank, if there is no Redis access password.
    The following figure is an example of adding a new **Redis** \> **ApsaraDB RDS** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15514310827594_en-US.png)

    Configurations:

    -   Type: Currently, the selected data source type is Redis \> On-premise Database with Public Network IP Address.
    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Access address: The format is host:port.
    -   Add an access address: Add an access address in the format of host:port.
    -   Redis access password: The Redis service access password.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Next step { .section}

This document explains how to configure the Redis Writer plug-in later. For more information, see [Configure Redis Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure Redis Writer.md#).

