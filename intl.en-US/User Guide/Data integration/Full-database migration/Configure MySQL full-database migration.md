# Configure MySQL full-database migration {#concept_j41_l1v_q2b .concept}

This topic describes how to migrate a Full-MySQL Database to MaxCompute with the Full-Database Migration feature.

The Full-Database Migration is a fast tool for improving user efficiency and reducing user usage costs, it can quickly upload all tables in the MySQL database to MaxCompute. For more information about Full-Database Migration, see [Full-Database migration](reseller.en-US/User Guide/Data integration/Full-database migration/Full-database migration overview.md#).

## Procedure {#section_fn2_s1v_q2b .section}

1.  Log on to [DataWorks\>Data Integration console](https://account.alibabacloud.com/login/login.htm), and click **Offline Sync** \> **Data Source** on the left to enter the data source management page.
2.  Click **Add-In Data** source in the upper-right corner to add a MySQL Data Source database for the entire database migration.
3.  After you click **Test Connectivity**, verify the data source is accessed correctly, confirm and save the data source.
4.  After the successful addition, the newly added MySQL data source clone\_database is displayed in the data source list. Click**Entire Library Migration** to migrate the corresponding MySQL data sourcefeatures page for the corresponding data source.

    The entire library migration page mainly has three functional areas.

    -   Filter table region for migration: The filter lists all database tables under the MySQL data source clone\_database. You can select database tables for batch migration.
    -   Advanced settings: The settings of the conversion rules of table names, column names, and column types between MySQL and MaxCompute data tables.
    -   Control area of the migration mode and concurrency: You can select either the Full-Database Migration mode \(full or incremental\) and the concurrency \(batch upload or full upload\), and check the progress for submitting migration tasks.
5.  Click **Advanced Settings** to select conversion rules based on the specific requirements. For example, the prefix ods\_ is added consistently when the MaxCompute table is built.
6.  In the control area of the migration mode and concurrency, select Daily Incremental as the synchronization mode and set gmt\_modified for the incremental field. By default, Data Integration generates a WHERE clause of the incremental extraction for each task based on the selected incremental field, and defines a daily data extraction condition by working with a DataWorks scheduling parameter, such as $\{bdp.system.bizdate\}.

    Data Integration is used to extract data from a MySQL library table to connect to a remote MySQL database by JDBC, and execute the corresponding SQL statement to select data from the MySQL library. Because it is a standard SQL extraction statement, you can configure the WHERE clause to control the data scope. Here you can view the WHERE clause for incremental extraction as follows:

    ```
    STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d') <= gmt_modified AND gmt_modified < DATE_ADD(STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d'), interval 1 day)
    ```

    To protect the MySQL data source from being overloaded by too many data synchronization jobs started at the same time point, Batch Upload can be selected. You can set start synchronization three database tables every hour starting from 00:00 everyday.

    Finally, click **Submit Task**, where you can view the migration progress information and the migration task status for each table.

7.  Click the migration task for table a1 to go to the Data Integration task development page.

    As shown in the preceding figure, the table odsa1 in MaxCompute corresponds to the successfully created source table a1, and the column name and type also matches the previously set conversion rules. The entire library migration tasks, and the task naming rule is the source table name can be found under the left-hand clone\_database directory tree, as shown in the preceding red box section.


Once you complete migrating the full MySQL data source clone\_database to MaxCompute, these tasks are scheduled to run according to the set scheduling cycle \(daily scheduling by default\). Also, you can transmit historical data by using the data completion feature in DataWorks. The **Data Integration** \> **Whole Library Migration**function can greatly reduce the configuration and migration costs of the initial cloud.

The whole library migration A1 table task performs a successful log as shown in the following figure:

