# Step 1: Create a table and upload data {#concept_h1h_nzl_s2b .concept}

In this article, we use creation of the tables bank\_data and result\_table as an example to describe how to create a table and upload data. The table of bank\_data stores the business data, while the result\_table stores the results after data analysis.

## Procedure {#section_cx2_hvq_s2b .section}

**Create a table called bank\_data**

1.  After [Create a project](../../../../intl.en-US/Preparation/Administrator operations/Create a project.md#) , click **Enter workspace**in the corresponding project.
2.  Go to the Data Studio \(original data development\) page and select **new** \> **table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150648972_en-US.png)

3.  Fill in the name of the table in the **new table** dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150648973_en-US.png)

4.  Click **Submit**.
5.  Enter the new table page, and select the **DDL mode**.
6.  Enter the table creation statement in the DDL schema dialog box, and click **build table structure**.

    For more SQL syntax for creating tables, see [creating/viewing/deleting tables](https://www.alibabacloud.com/help/doc-detail/27808.htm).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658974_en-US.png)

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

7.  After the table structure is generated, enter the Chinese name of the table and click **Submit to development environment**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658976_en-US.png)

8.  After the creation is successful, you can search it by entering the table name in the left-hand navigation **table management**, view table information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658977_en-US.png)


**Create result\_table**

1.  Go to the DataStudio page and select **new** \> **table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150648972_en-US.png)

2.  Fill in the name of the table in the **new table** dialog box and click **Submit**.
3.  Enter the new table page, and select the **DDL mode**.
4.  Enter the build TABLE statement in the DDL schema dialog box, and click **build table structure**. An example of table creation is as follows:

    ```
    CREATE TABLE IF NOT EXISTS result_table
    (  
     education   STRING COMMENT 'educational level',
     num         BIGINT COMMENT 'number of people'
    );
    ```

5.  After the creation is successful, you can search it by entering the table name in the left-hand navigation **table management**, view table information.

**Upload local data to bank\_data**

Dataworks supports the following actions:

-   Uploading the data in locally stored text files to the table in the workspace.
-   Using the data integration module to import business data from various data sources to the workspace.

**Note:** In this section, local files are used as the data source. Local text file uploads have the following restrictions:

-   File type: Only .txt and .csv files are supported.
-   File Size: Not exceeding 10 M.
-   Operation objects: Partition and non-partition tables can be imported, but Chinese partition values are not supported.

For example, import local file [banking.txt](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/shujia/0.2.00/assets/pic/data-develop/banking.txt)to DataWorks, the operation is as follows:

1.  Click **Import** to select **import local data**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658978_en-US.png)

2.  Select a local data file, configure the import information, and click **Next**,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658979_en-US.png)

3.  Enter at least two letters to search for the table by name. Select the table to which the data is to be imported, for example, bank\_data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658980_en-US.png)

4.  Select the field matching method \("Match by Position" is used in this example\), and click **Import**,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16180/15362150658981_en-US.png)


After the file is imported, the system returns the number of lines that were successful in your data import or an exception that failed.

## Other data import methods {#section_msh_yfr_s2b .section}

-   Create a Data Synchronization task

    This method applies to saving RDS, MySQL, SQL Server, PostgreSQL, MaxCompute, OSS, DRDs, OSS data from a variety of data sources such as, Oracle, FTP, DM, HDFS, and MongoDB.

    For details on creating a data synchronization task with Dataworks, see [creating a data synchronization task](intl.en-US/Quick Start/Step 3: Create a synchronization task.md#).

-   Local file uploads

    Ci fang shi yong yu wen jian great&small bu chao guo 10m、 wen jian lei xing wei .txt he .csv data, the target supports partition tables and non-partition tables, but does not support Chinese as a partition.

    For local file upload via DataWorks, see local data upload to bank\_data above for details.

-   Upload files using tunnel command

    This method applies to local files and other resource files more than 10 m in size.

    Upload and download the data through tunnel commands provided by the [MaxCompute client](https://www.alibabacloud.com/help/doc-detail/27971.htm), when local data files need to be uploaded to the partition table, they can be uploaded using the client tunnel command. See [Tunnel command actions](https://www.alibabacloud.com/help/doc-detail/27833.htm) for details.


## Next steps {#section_jvs_vgr_s2b .section}

You have learned know how to create a table and upload data now. You can go to the next tutorial which will show you how to create a flow for further data analysis and computing in the project space. For more information, see [creating a business process](intl.en-US/Quick Start/Step two: Create a Business Process.md#).

