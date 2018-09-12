# Configure Oracle full-database migration {#concept_frm_rfv_q2b .concept}

This article demonstrates how to migrate a full Oracle database to MaxCompute by using the full-database migration feature.

The whole library migration is a fast tool for improving user efficiency and reducing user usage costs, it can quickly upload all the tables in the Oracle database to maxcompute, for a detailed introduction to the whole library migration, see [Full-database migration overview](intl.en-US/User Guide/Data Integration/Full-database migration/Full-database migration overview.md#).

## Procedure {#section_fn2_s1v_q2b .section}

1.  Log in to the [DataWorks management console](https://workbench.data.aliyun.com/console) and select **data integration** in the top menu bar.
2.  Select **offline synchronization** \> **data source** in the left navigation bar and go to the data source management page.
3.  Click **Add-in data source** in the upper-right corner to add an Oracle Data Source hub for the whole library migration.
4.  After you click **test connectivity** and verify that the data source is accessed correctly, confirm and save the data source.
5.  After successful addition, the newly added Oracle data source clone\_database is displayed in the data source list. Click the entire library migration that corresponds to the Oracle data source, you can go to the **entire library migration** features page for the corresponding data source.

    The whole library migration page mainly has three functional areas.

    -   Filter area of tables to be migrated: It lists all the database tables under the Oracle data source clone\_database. You can select database tables to be migrated in batch.
    -   Advanced settings: It provides the conversion rules of table names, column names, and column types between Oracle and MaxCompute data tables.
    -   Control area of the migration mode and concurrency: You can select the full-database migration mode \(full or incremental\) and the concurrency \(batch upload or full upload\) and check the progress of submitting the migration task.
6.  Click **Advanced Settings** to select conversion rules based on specific requirements.
7.  In the control area of the migration mode and concurrency, select Daily Full as the synchronization mode.

    **Note:** If the date field exists in your table, you can select Daily Incremental as the synchronization mode, and set the incremental field as the date field. Data Integration generates a where condition of incremental extraction for each task based on the selected incremental field by default and defines a daily data extraction condition by working with a DataWorks scheduling parameter such as $\{bdp.system.bizdate\}.

    To protect the Oracle data source from being overloaded by too many data synchronization jobs started at the same point of time, Batch Upload can be selected. You can set to start synchronizing three database tables every one hour from 00:00 every day.

    Finally, click **Submit task**, where you can see the migration progress information and the status of the migration task for each table.

8.  Click the **view task** corresponding to the table to jump to the task Development page for data integration, you can view the run details of the task.

    So far, you have completed migrating the full Oracle data source clone\_databae to MaxCompute. These tasks are scheduled to run according to the set scheduling cycle \(daily scheduling by default\). Also, you can transmit historical data by using the data completing feature of DataWorks. The **data integration** \> **whole library migration** function can greatly reduce the configuration and migration costs of your initial cloud.


