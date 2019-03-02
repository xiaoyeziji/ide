# Configure Oracle full-database migration {#concept_frm_rfv_q2b .concept}

This article demonstrates how to migrate a full Oracle database to MaxCompute using the Full-Database Migration feature.

The Full-Database Migration is a fast tool for improving user efficiency and reducing user usage costs, it can quickly upload all tables in the Oracle database to MaxCompute. For more information about Full-Database Migration, see [Full-database migration overview](reseller.en-US/User Guide/Data integration/Full-database migration/Full-database migration overview.md#).

## Procedure {#section_fn2_s1v_q2b .section}

1.  Log on to the [DataWorks management console](https://partners-intl.aliyun.com) and select **Data Integration** in the top menu bar.
2.  Select **Offline Synchronization** \> **Data Source** in the left-navigation pane and go to the Data Source Management Page.
3.  Click **Add-in Data Source** in the upper-right corner to add an Oracle Data Source hub for the Full-database migration.
4.  After you click **Test Connectivity**, verify the data source is accessed correctly, and save the data source after confirmation.
5.  After successful addition, the newly added Oracle data source clone\_database is displayed in the data source list. Click the entire library migration that corresponds to the Oracle data source, you can go to the **Full-Database Migration** features page for the corresponding data source.

    The Full-Database Migration page has three main functions.

    -   Filter migration table area: Lists all database tables under the Oracle data source clone\_database. You can select database tables for batch migration.
    -   Advanced settings: The settings provide conversion rules of table names, column names, and column types between Oracle and MaxCompute data tables.
    -   Control area of the migration mode and concurrency: You can select the full-database migration mode \(full or incremental\) and the concurrency \(batch upload or full upload\), and check the submitted migration task progress.
6.  Click **Advanced Settings** to select conversion rules based on specific requirements.
7.  In the control area of the migration mode and concurrency, select the synchronization mode Daily Full.

    **Note:** If the date field exists in your table, you can select the synchronization mode Daily Incremental, and set the incremental field as the date field. Data Integration generates a WHERE clause for incremental extraction. By default, each task is based on the selected incremental field, and defines a daily data extraction condition by working with a DataWorks scheduling parameter, such as $\{bdp.system.bizdate\}.

    You can select Batch Upload to protect the Oracle data source from being overloaded by too many data synchronization jobs that start at the same time point. You can set to start synchronizing three database tables every one hour starting from 00:00 every day.

    Click **Submit Task**to view the migration progress information and the migration task status for each table.

8.  Click **View Task** corresponding to the table to go to the Task Development page for Data Integration, where you can view the task run details.

    After completing the full migration of the Oracle data source clone\_database to MaxCompute, these tasks are scheduled to run according to the set scheduling cycle. By default, the daily scheduling is the set scheduling cycle. Also, you can transmit historical data using the data completing feature for DataWorks. The **Data Integration** \> **Full-Database Migration** function can greatly reduce the configuration and migration costs of the initial cloud.


