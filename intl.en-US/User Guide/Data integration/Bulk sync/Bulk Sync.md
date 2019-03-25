# Bulk Sync {#concept_zb1_vk1_pfb .concept}

This topic describes how to Bulk Sync.

Bulk Sync is a tool that can improve data upload efficiency and reduce costs. It allows you to quickly upload all tables in MySQL, Oracle, SQL Server databases to MaxCompute in bulk, which greatly reduces time spent on creating the bulk task for data migration initialization.

You can flexibly configure the following items to meet your business requirements: table name conversion, field name conversion, field data type conversion, sink table add-on filed, sink table field value, data filter, sink table name prefix rules, and others.

In **The Data Integration** \> **Sync Resources** \> **Bulk Sync** Page, you can check the configured cloud migration tasks.

**Note:** 

-   The **Log** and **View Rules** options under the Actions column in the Bulk Sync list, are read-only and cannot be modified.
-   The submitted configuration rule becomes invalid, if the task is not submitted.

## Procedure {#section_pzh_spb_pfb .section}

1.  Select the data source for synchronization.

    Select the successfully added synchronous data source. You can select multiple data sources with the same data source type, for example MySQL, Oracle, or SQL Server. For more information, see [Add data sources in Bulk Mode](reseller.en-US/User Guide/Data integration/Bulk sync/Add data sources in Bulk Mode.md#).

2.  Configure synchronization rules.

    Currently, nine configuration rules are supported. You can select the rule configuration based on your needs, and execute the rule, and then check DDLs and synchronous Scripts to confirm the configuration rule effect.

    **Note:** 

    -   You can try Script Mode, if the rules in the interface do not meet your requirements.
    -   After the rules are configured , you must **Execute Rules** and **Submit Tasks**, otherwise the rules you configure will not be recorded after refreshing or closing the browser.
    |Action|Configuration|Description|
    |:-----|:------------|:----------|
    |**Add rule**|**Target table partition field rules**|Displays the partition content, based on the schedule parameter configuration. For more information about parameter configuration, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Parameter configuration.md#).|
    |**Table name conversion rules**|Selects any word in the database table name to convert into the required content.|
    |**Field name conversion rules**|Select any word from the field name in the table to convert the required content.|
    |**Type conversion rules**|Select the data type in your source database to convert into the required data type.|
    |**Create new field in target table rule**|You can add a column to the MaxCompute table based on the name you specified.|
    |**Assignment in target table rule**|Assign a value in the newly added field.|
    |**Data filtering rule**|Filter data in the table from the selected source database.|
    |**Target table name prefix rule**|Add a prefix to the table name.|
    |**Convert to script**|When this action is configured, it can perform conversions under Script Mode configuration. Compared to the UI Mode, each rule in Script Mode can be specified with an action scope. However, when the UI Mode is converted to Script Mode, and cannot be reverted to UI Mode configuration.|
    |**Reset script**|The script can only be reset after it is converted to Script Mode. When you click this icon, the unified script template will pop-up .|
    |**Execution rules**|Click **Execution Rules** to view the rules effect on the DDL script and the synchronization script. This action does not create tasks, and provides only a preview of the DDL and synchronization scripts.You can select a part of the table to check the corresponding DDL, and synchronization scripts to see if it complies with the rules.

|

3.  Select the tables for synchronization and commit.

    You can select multiple tables for bulk commit, and the MaxCompute table will be created based on the preceding configuration rules. If the execution fails, you can move the mouse over to the execution result and the system will prompt the cause of failure.

    |Configuration|Descriptions|
    |:------------|:-----------|
    |**DDL**|You can only view related table creation statements, and cannot modify them after clicking DDL.|
    |**Sync configuration**|Click Sync configuration to view configured tasks, which are displayed in Script Mode.|
    |**View table**|Go to the data management console page to view details of the created MaxCompute table.|

4.  View tasks.

    After a task is submitted successfully, you can enter **Data Development** \> **Business Processes** Page to view bulk cloud migration task.

    The number of business processes is the same as the number of selected source databases . The general naming rule is clone\_database \_ \`data source name\`. Each table generates a synchronization task, and the naming rule is the \`data source name\`2odps\_\`table name\`.

    1.  Task configuration: Synchronize the MySQL generated by bulk cloud migration to MaxCompute synchronize task, where the data filter condition is generated by the Data Filtering Rule.
    2.  Field mapping: The mapping target field output is based on the relevant field rule, and you can view the output based on the configuration rule.
    3.  Tunnel configuration: You can configure synchronization task DMU, job concurrency, number of error records in Tunnel Configuration. This configuration is closely related to the task running speed.

        **Note:** Please go to [Configure Reader plug-in](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#) and [Configure Writer plug-in](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#) for task configuration instructions.

5.  Run the task.

    Click **Run**to immediately run the synchronization task. Alternatively, you can submit the synchronization task to the scheduling system by clicking **Submit**. The scheduling system periodically runs the task based on the task configurations starting from the second day. For more information about run task, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).


**Note:** 

-   **Simple Mode**: The task takes effect in the production environment immediately after submission.
-   **Standard Mode**:The task is submitted in the development environment, and then published to the production environment.

