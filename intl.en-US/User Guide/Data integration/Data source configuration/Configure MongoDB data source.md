# Configure MongoDB data source {#concept_cxg_1s1_p2b .concept}

This topic describes how to configure MongoDB data sources in DataWorks. MongoDB, is one of the world's most popular document-based NoSQL databases following Oracle and MySQL. The MongoDB data source allows you to read/write data to MongoDB, and supports configuring synchronization tasks in Script Mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15514306867534_en-US.png)

4.  Select the data source type **MongoDB** in the new data source dialog box.
5.  Complete the MongoDB data source item configuration.

    MongoDB data source types are categorized into **ApsaraDB** and **On-Premise Database Public Network IP Address**.

    -   ApsaraDB: These databases generally use classic networks. The classic network does not support cross-region connections.
    -   User-created databases with public IP addresses: These databases generally use public networks that may incur certain costs.
    Consider a data source with a new **MongoDB** \> **ApsaraDB** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16206/15514306867547_en-US.png)

    Configurations:

    -   Data Source Type: Select the data source type "MongoDB: Alibaba Cloud database".

        **Note:** 

        If you have not granted the default role data integration system permission , the primary account is required to go to RAM for role authorization and then refresh the page.

    -   Name: A name must start with a letter or underscores \(\_\) and can be 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Region: Refers to the selected region when purchasing MongoDB.
    -   Instance ID: You can view the MongoDB instance ID in the MongoDB console.
    -   Database name: You can create a new database in the MongoDB console, configure the corresponding data name, user name, and password.
    -   Username and password: The user name and password used for the database connection.
    The following is an example of a data source with a new **MongoDB** \> **On-Premise Database with Public Network IP Address**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16206/15514306867548_en-US.png)

    Configurations:

    -   Type: Select the data source type "MongoDB: User‑created database with a public IP address".
    -   Name: A name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Visit address: The format is host:port.
    -   Add visit address: Add an access address in the format of host:port.
    -   Database name: The database name mapped to the data source.
    -   Username and password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**
7.  If the connectivity passed the test, click **Complete**.

    **Note:** 

    -   A MongoDB cloud database in a VPC environment is added with a public network IP address data source type and saved.
    -   Currently, the VPC network does not support connectivity tests.

## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure the MongoDB Writer plug‑in, see [Configure MongoDB Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure MongoDB Writer.md#).

