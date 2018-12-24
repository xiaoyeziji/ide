# Configure MySQL full-database migration {#concept_j41_l1v_q2b .concept}

This article demonstrates how to migrate a full MySQL database to MaxCompute with the full-database migration feature.

The full-database migration is a fast tool for improving user efficiency and reducing user usage costs, it can quickly upload all the tables in the MySQL database to MaxCompute, for a detailed introduction to the whole library migration, see [Full-database migration overview](reseller.en-US/User Guide/Data Integration/Full-database migration/Full-database migration overview.md#).

## Procedure {#section_fn2_s1v_q2b .section}

1.  Log in to [dataworks\> Data Integration console](https://account.alibabacloud.com/login/login.htm), click **offline sync** \> **data source** on the left to enter the data source management page.
2.  Click **Add-in data** source in the upper-right corner to add a Mysql Data Source library for the whole library migration.
3.  After you click **test connectivity** and verify that the data source is accessed correctly, confirm and save the data source.
4.  After successful addition, the newly added MySQL data source clone\_database is displayed in the data source list. Click the entire library migration that corresponds to the MySQL data source, you can go to the **entire library migration** features page for the corresponding data source.

    The whole library migration page mainly has three functional areas.

    -   Filter area of tables to be migrated: It lists all the database tables under the MySQL data source clone\_database. You can select database tables to be migrated in batch.
    -   Advanced Settings: It provides the conversion rules of table names, column names, and column types between MySQL and MaxCompute data tables.
    -   Control area of the migration mode and concurrency: You can select the full-database migration mode \(full or incremental\) and the concurrency \(batch upload or full upload\) and check the progress of submitting the migration task.
5.  Click **Advanced Settings** to select conversion rules based on specific requirements. For example, the prefix ods\_ was added consistently when the MaxCompute table was built.
6.  In the control area of the migration mode and concurrency, select Daily Incremental as the synchronization mode and set gmt\_modified for the incremental field. Data Integration generates a where condition of incremental extraction for each task based on the selected incremental field by default and defines a daily data extraction condition by working with a DataWorks scheduling parameter such as $\{bdp.system.bizdate\},

    Data integration is used to extract data from a MySQL library table to connect to a remote MySQL database via JDBC, and execute the corresponding SQL statement to select the data from the MySQL library. Since it is a standard SQL extraction statement, you can configure the WHERE clause to control the scope of data. Here you can view where conditions for incremental extraction are as follows:

    ```
    STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d') <= gmt_modified AND gmt_modified < DATE_ADD(STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d'), interval 1 day)
    ```

    To protect the MySQL data source from being overloaded by too many data synchronization jobs started at the same point of time, Batch Upload can be selected. You can set to start synchronizing three database tables every one hour from 00:00 everyday.

    Finally, click **Submit task**, where you can see the migration progress information and the status of the migration task for each table.

7.  Click the migration task for table a1 to jump to the task development page of Data Integration,

    As shown in the preceding figure, the table ods\_a1 in MaxCompute corresponding to the source table a1 is created successfully, and the column name and type also match the previously set conversion rules. Under the left-hand directory tree clone\_database directory, there will be all of the corresponding whole library migration tasks, the task naming rule is the source table name, as shown in the red box section above.


So far, you have completed migrating the full MySQL data source clone\_databae to MaxCompute. These tasks are scheduled to run according to the set scheduling cycle \(daily scheduling by default\). Also, you can transmit historical data by using the data completing feature of DataWorks. The **data integration** \> **whole library migration**function can greatly reduce the configuration and migration costs of your initial cloud.

The whole library migration A1 table task performs a successful log as shown in the following figure:

