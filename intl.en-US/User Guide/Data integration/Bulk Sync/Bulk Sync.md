# Bulk Sync {#concept_zb1_vk1_pfb .concept}

This article will show you how to Bulk Sync.

Bulk Sync is a tool that can help you to improve efficiency and reduce the cost. It allows you to quickly upload all tables in MySQL, Oracle, SQL Server databases to MaxCompute in one time which saves a lot of time on the creation of bulk task for initialization data migration.

**Note:** Currently Bulk Sync function only supports Shanghai region.

you can flexibility configure table name conversion ,field name conversion, field data type conversion, sink table add-on filed, sink table field value, data filter, sink table name prefix rules, etc. to meet your business requirement.

In **The Data Integration** \> **Sync Resources** \> **Bulk Sync** Page, you can check the cloud migration tasks that you configured.

**Note:** 

-   **Log** and **View Rules**, under the Actions column in the Bulk Sync list, are only readable rather than modifiable.
-   The configuration rule you submitted would invalid if the task does not submit accordingly.

## Procedure {#section_pzh_spb_pfb .section}

1.  Select the data source for synchronization.

    Select the ready successful added synchronous data source. You can select multiple data sources with the same data source type, for example MySQL, Oracle, or SQL Server. Please refer [Add data sources in Bulk Mode](reseller.en-US/User Guide/Data integration/Bulk Sync/Add data sources in Bulk Mode.md#).

2.  Configure synchronization rules.

    Currently, nine configuration rules are supported, and you can select the appropriate rule configuration according to your needs, then you can execute the rule, and check DDLs and synchronous scripts to confirm the effect of the configuration rules.

    **Note:** 

    -   If the rules in the interface do not meet your needs, you can try the script mode.
    -   After configure the rules, you must **Execution rules** and **Submitting tasks**, Otherwise the rules you configure would not be recorded after refreshing or closing the browser.
    |Action|Configuration|Instructions|
    |:-----|:------------|:-----------|
    |**Add rule**|**Target table partition field rules**|Show the content of the partition, in accordance with the schedule parameter configuration, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) for the details.|
    |**Table name conversion rules**|select any word of your database table name then convert to the content you need.|
    |**Field name conversion rules**|Select any word for the name of the field in your table to convert to what you need.|
    |**Type conversion rules**|Select data type in your source database then convert into the data tyoe you need.|
    |**The rule of create new field in target table**|You can add a column to the MaxCompute table with the name according to your needs.|
    |**The rule of assignment in target table**|Assign a value in your newly added filed.|
    |**The rule of Data Filtering**|Filter data in the table from the source database you selected.|
    |**The rule of target table name prefix**|Add a prefix to the table name.|
    |**Convert to script**|Configuration rule can convert into script mode configuration. Compared with UI mode, each rule in script mode can be specified with scope of action. However, when the UI mode is converted to script mode, it cannot be converted back to UI configuration mode.|
    |**Reset script**|Script can be reset only after converted to script mode. Unified script template will pop up when click this icon.|
    |**Execution rules**|Click **Execution rules**, You can see the effect of the rules on the DDL script and the synchronization script, and this action does not create the task, provides only a preview of the DDL and synchronization scripts.You can select a part of the table to check for the corresponding DDL and synchronization scripts to see if they comply with the rules.

|

3.  Select the tables to synchronize and commit.

    You can select multiple tables for bulk commit, and the MaxCompute table will be created based on the above configuration rules. If the execution fails, you can place mouse over the execution result and system will prompt a hint for the cause of failure.

    |Configuration|Instructions|
    |:------------|:-----------|
    |**DDL**|After you click, you can only view the related table creation statements, rather then modify them.|
    |**Sync configuration**|Click Sync configuration to view the tasks that you configured, which are displayed in script mode.|
    |**View table**|Navigate to the appropriate data management console page, where you can view the create details of MaxCompute table.|

4.  View tasks.

    After task submitted successfully, you can enter **Data Development** \> **Business Processes** Page to view your bulk cloud migration task.

    The number of business process is same as the number of source database you selected. The general naming rule is clone\_database \_ \`data source name\`. Each table generates a synchronization task, and the naming rule is the \`data source name\`2odps\_\`table name\`.

    1.  Task configuration: synchronize the MySQL, generated by bulk cloud migration, to odps synchronize task, and the data filer condition is generated by The rule of Data Filtering.
    2.  Field mapping: The mapping target field output is based on the relevant field rule, you can view the output depends on your configuration rule.
    3.  Tunnel Configuration: You can configure synchronization task DMU, job concurrency, number of error records in Tunnel Configuration. This configuration is closely related to the running speed of the task.

        **Note:** Please refer to [Configure Reader plug-in](reseller.en-US/User Guide/Data integration/Task Configuration/Configure Reader plug-in/Wizard mode configuration.md#) and [Configure writer plug-in](reseller.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) Writer.md#) for instruction of task configuration.

5.  Run the task.

    Click **Run**，the synchronization task will run immediately. Alternatively, You can submit the synchronization task to the scheduling system by click **Submit**. The scheduling system periodically runs the task according to the task configurations starting from the second day. For more detail please refer to [Scheduling Configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).


**Note:** 

-   **Simple Mode**: Task takes effect in production environment directly after submission .
-   **Standard Mode**:Task is submitted into development environment, then publish to the production environment.

