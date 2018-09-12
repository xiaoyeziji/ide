# Configure MongoDB data source {#concept_cxg_1s1_p2b .concept}

MongoDB, as a NoSQL database, is one of the world's most popular document-based databases, next only to Oracle and MySQL. The MongoDB data source allows you to read data from and write data to MongoDB, and supports configuring synchronization tasks in script mode.

**Note:** To add a MongoDB data source, please set up a white list in the MongoDB administration console, IP White List fill in the address as follows \(addresses are separated by a comma in English \):

10.152.69.0/24,10.153.136.0/24,10.143.32.0/24,120.27.160.26,10.46.67.156,120.27.160.81,10.46.64.81,121.43.110.160,10.117.39.238,121.43.112.137,10.117.28.203,118.178.84.74,10.27.63.41,118.178.56.228,10.27.63.60,118.178.59.233,10.27.63.38,118.178.142.154,10.27.63.15,100.64.0.0/8,10.151.99.0/24

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15367208907534_en-US.png)

4.  In the new data source pop-up box, select the data source type as **MongoDB**.
5.  Complete the configuration items for the MongoDB data source.

    MongoDB data source types are divided into **Alibaba cloud database** and **public network IP self-built database**.

    -   Alibaba Cloud databases: These databases generally use classic networks. Classic networks within the same region can connect to each other, but those in different regions may not.
    -   User-created databases with public IPs: These databases generally use public networks, which may cause a certain cost.
    Consider a data source with a new **MongDB** \> **Ali cloud database** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16206/15367208907547_en-US.png)

    Configurations:

    -   Data Source Type: The selected data source type "MongoDB: Alibaba Cloud database".

        **Note:** 

        If you have not already authorized the default role of the data integration system, you need the master account to go to ram for the Role authorization, then refresh the page.

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Region: refers to the region selected when purchasing MongoDB.
    -   Instance ID: You can view the MongoDB instance ID in the MongoDB console.
    -   Database Name: you can create a new database in the MongoDB console, set the appropriate data name, user name, and password.
    -   Username/Password: The user name and password used to connect to the database.
    Consider a data source with a new **MongDB** \> **self-built database with public network IP** as an example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16206/15367208907548_en-US.png)

    Configurations:

    -   Type: The selected data source type "MongoDB: User‑created database with a public IP".
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Visit the address: The format is host:port.
    -   Add visit address: Add an access address in the format of host:port.
    -   Database name: It is the name of the database mapped to the data source.
    -   Username/Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

    **Note:** 

    -   A MongoDB cloud database in a VPC environment that is added with a public network IP data source type and saved.
    -   The VPC does not support connectivity tests for now.

## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the MongoDB data source. The document explains how to configure the MongoDB Writer plug‑in later. For more information, see [Configure MongoDB Writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure MongoDB Writer.md#).

