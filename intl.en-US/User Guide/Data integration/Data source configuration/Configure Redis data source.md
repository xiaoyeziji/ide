# Configure Redis data source {#concept_t3r_4tb_p2b .concept}

Redis is a document-based NoSQL database that provides persistent memory database services. Based on its highly reliable master/slave hot backup architecture and seamlessly scalable cluster architecture, this service can meet the needs of businesses that require high read/write performance and flexible capacity configuration. The Redis data source allows you to read data from and write data to Redis, and supports configuring synchronization tasks in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15396618507592_en-US.png)

4.  In the new data source pop-up box, select a data source type of **Redis**.
5.  Complete the configuration items for the Redis data source.

    The data source types of redis are divided into **Alibaba cloud database** and **public network IP self-built database**.

    -   Alibaba Cloud databases: These databases generally use classic networks. Classic networks within the same region can connect to each other, but those in different regions may not.
    -   User‑created databases with public IPs: These databases generally use public networks, which may cause a certain cost.
    Consider a data source that adds a new **Redis** \> **Ali cloud database** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15396618507593_en-US.png)

    Configurations:

    -   Type: the currently selected data source type is Redis\> Ali cloud database.

        **Note:** If you have not already authorized the default role for the data integration system, you need the master account to go to ram to authorize the role, then refresh the page.

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Region: refers to the region selected when purchasing redis.
    -   Instance ID of Reids: You can go to the Redis console to view the redis instance ID.
    -   Redis access password: the access password for the Redis Server, and does not fill in if not.
    Consider a data source that adds a new **Redis** \> **Alibaba cloud database** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16212/15396618507594_en-US.png)

    Configurations:

    -   Type: the currently selected data source type is redis \> self-built database with public network IP.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Access address: The format is host:port.
    -   Add an Access Address: Add an access address in the format of host:port.
    -   Redis access password: Redis's service access password.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Next step { .section}

Now you have learned how to configure the Redis data source. The document explains how to configure the Redis Writer-plug in later. For more information, see [Configure Redis Writer](reseller.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure Redis Writer.md#).

