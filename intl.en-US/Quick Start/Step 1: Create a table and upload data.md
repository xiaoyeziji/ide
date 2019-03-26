# Step 1: Create a table and upload data {#concept_h1h_nzl_s2b .concept}

In this topic, the created tables bank\_data and result\_table are used as an example for creating a table and uploading data. The bank\_data table stores business data, while the result\_table stores data analysis results.

## Procedure {#section_cx2_hvq_s2b .section}

**Create a table called bank\_data**

1.  After [Create Workspace](../../../../../reseller.en-US/Preparation/Administrator Operations/Create Workspace.md#) , click **Enter workspace**in the corresponding project.
2.  Go to the Data Studio \(original data development\) page and select **new** \> **table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858972_en-US.png)

3.  Enter the table name in the **new table** dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858973_en-US.png)

4.  Click **Submit**.
5.  Enter the new table page, and select the **DDL mode**.
6.  Enter the table creation statement in the DDL schema dialog box, and click **build table structure**.

    For more SQL syntax for creating tables, see [creating/viewing/deleting tables](https://www.alibabacloud.com/help/doc-detail/27808.htm).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858974_en-US.png)

    The statements used for table creation in this example are as follows:

    ```
    CREATE TABLE IF NOT EXISTS bank_data
    (
     age             BIGINT COMMENT 'age',
     job             STRING COMMENT 'job type',
     marital         STRING COMMENT 'marital status',
     education       STRING COMMENT 'educational level',
     default         STRING COMMENT 'credit card ownership',
     housing         STRING COMMENT 'mortgage',
     loan            STRING COMMENT 'loan',
     contact         STRING COMMENT 'contact information',
     month           STRING COMMENT 'month',
     day_of_week     STRING COMMENT 'day of the week',
     duration        STRING COMMENT 'Duration',
     campaign        BIGINT COMMENT 'contact times during the campaign',
     pdays           DOUBLE COMMENT 'time interval from the last contact',
     previous        DOUBLE COMMENT 'previous contact times with the customer',
     poutcome        STRING COMMENT 'marketing result',
     emp_var_rate    DOUBLE COMMENT 'employment change rate',
     cons_price_idx  DOUBLE COMMENT 'consumer price index',
     cons_conf_idx   DOUBLE COMMENT 'consumer confidence index',
     euribor3m       DOUBLE COMMENT 'euro deposit rate',
     nr_employed     DOUBLE COMMENT 'number of employees',
     y               BIGINT COMMENT 'has time deposit or not'
    );
    ```

7.  After the table structure is generated, enter the table name and click **Submit to Production Environment**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858976_en-US.png)

8.  You can search the created table by entering the table name in the left-hand navigation **table management** to view the table information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858977_en-US.png)


**Create result\_table**

1.  Go to the DataStudio page and select **new** \> **table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858972_en-US.png)

2.  Enter the table name in the **new table** dialog box and click **Submit**.
3.  Enter the new table page and select **DDL mode**.
4.  Enter the build TABLE statement in the DDL schema dialog box, and click **build table structure**. The following is a create table example:

    ```
    CREATE TABLE IF NOT EXISTS result_table
    (  
     education   STRING COMMENT 'educational level',
     num         BIGINT COMMENT 'number of people'
    );
    ```

5.  You can search the created table by the table name in the left-hand navigation **table management** and view table information.

**Upload local data to bank\_data**

DataWorks supports the following actions:

-   Uploading data in locally stored text files to the workspace table.
-   Using data integration to import business data from various data sources to the workspace.

**Note:** In this section, local files are used as the data source. Local text file uploads have the following restrictions:

-   File type: Only .txt and .csv files are supported.
-   File Size: Not exceeding 10 M.
-   Operation objects: Partition and non-partition tables can be imported, but Chinese partition values are not supported.

For example, import local file [banking.txt](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/shujia/0.2.00/assets/pic/data-develop/banking.txt)to DataWorks, the operation is as follows:

1.  Click **Import** to select **import local data**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790858978_en-US.png)

2.  Select a local data file, configure the import information, and click **Next**,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790868979_en-US.png)

3.  Enter at least two letters to search the table by name. Select the table for the data to be imported, for example, bank\_data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790868980_en-US.png)

4.  Select the field matching method \("Match by Position" used in this example\) and click **Import**,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15535790868981_en-US.png)


After the file is imported, the system returns the number of lines that were successful in the data import or an exception that failed.

## Other data import methods {#section_msh_yfr_s2b .section}

-   Create a Data Synchronization task

    This method applies to saving RDS, MySQL, SQL Server, PostgreSQL, MaxCompute, OSS, DRDs, OSS data from a variety of data sources, such as, Oracle, FTP, DM, HDFS, and MongoDB.

    For details on creating data synchronization tasks with DataWorks, see [creating a data synchronization task](reseller.en-US/Quick Start/Step 3: Create a synchronization task.md#).

-   Local file uploads

    This file upload method is suitable for .txt and .csv files smaller than 10M , and the target supports both partition and non-partition tables , but does not support Chinese partition.

    For DataWorks local file upload, see preceding local data upload to bank\_data for details.

-   Upload files using the tunnel command

    This method applies to local files and other resource files greater than 10M in size.

    Upload and download the data through tunnel commands provided by the [MaxCompute client](https://www.alibabacloud.com/help/doc-detail/27971.htm), when local data files need to be uploaded to the partition table, so they can be uploaded using the client tunnel command. See [Tunnel command actions](https://www.alibabacloud.com/help/doc-detail/27833.htm) for details.


## Next steps {#section_jvs_vgr_s2b .section}

You have learned how to create a table and upload data now. You can go to the next topic, which will show you how to create a work flow for further data analysis and project space computing . For more information, see [creating a workflow](reseller.en-US/Quick Start/Step 2: Create a Business Flow.md#).

