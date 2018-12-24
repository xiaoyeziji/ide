# Manual data intergration node {#concept_amn_wcm_q2b .concept}

Currently, the data intergration task supports the following data sources: MaxCompute, MySQL, DRDS, SQL Server, PostgreSQL, Oracle, MongoDB, DB2, OTS, OTS Stream, OSS, FTP, Hbase, LogHub, HDFS, and Stream. For details about more supported data sources, see [Supported data sources](reseller.en-US/User Guide/Data Integration/Data source configuration/Supported data sources.md#).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16323/15389925038058_en-US.png)

1.  Create a business flow

    Click **Manual Business Flow** in the left-side navigation pane, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15389925037961_en-US.png)

2.  Create a data intergration node

    Right-click **Data Integration**, and select **Create Data Data Integration Node** \> **Data Integration**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16323/15389925038059_en-US.png)

3.  Configure a intergration task

    You can enter the source table name and target table name to complete a simple task configuration.

    After you enter a table name, a list of objects that match the table name is automatically displayed\(Currently, only exact match is supported. Therefore, you must enter the correct and complete table name\), Some objects are not supported by the current intergration center and are marked **Not supported**. You can move the mouse over an object. The detailed information about the object, such as the database, IP address, and owner of the table, is automatically displayed. The information helps you select an appropriate table object. After selecting an object, click the object. The column information is automatically filled in. You can edit columns, for example, moving, deleting, or adding column.

    1.  Configure intergration tables.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16323/15389925038068_en-US.png)

    2.  Edit the data source.

        Generally, you do not need to edit the content of the source table unless necessary.

        -   Click **Insert** on the right of a column to insert a new column.
        -   Click **Delete** on the right of a column to delete the column.
    3.  Edit the data destination.

        Generally, you do not need to edit the field information of the destination table unless necessary \(for example, you need to import data of only some columns\).

        **Note:** If the destination is an ODPS table, columns cannot be deleted. In configuration of a intergration task, the field settings of the source table matches those of the destination table in one-to-one relationship by page instead of by field name.

    4.  Incremental intergration and full intergration.

        -   Shard format for incremental intergration: ds=$\{bizdate\}
        -   Shard format for full intergration: ds=\*
        **Note:** If multiple shards need to be synchronized, the intergration center supports simple regular expressions.

        -   For example, if you need to synchronize multiple shards, but it is difficult to write regular expressions, use the following method: `ds=20180312 | ds=20180313 | ds=20180314;`
        -   If you need to synchronize shards in the same range, the intergration center supports an extended syntax similar to the following: `/*query*/ds>=20180313 and ds<20180315;` If this method is used, you must add /query/.
        -   The variable bizdate must be defined in the following parameter: `-p"-Dbizdate=$bizdate -Denv_path=$env_path -Dhour=$hour"`. If you need to customize a variable, for example, `pt=${selfVar}`, also define the variable in the parameter, for example, `-p"-Dbizdate=$bizdate -Denv_path=$env_path -Dhour=$hour -DselfVar=xxxx"`.
    5.  Field mapping.

        Fields are mapped based on the locations of fields in the source table and destination table, instead of based on the field names and types.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16323/15389925038081_en-US.png)

        **Note:** If the source table is an ODPS table, fields cannot be added during data intergration. If the source table is not an ODPS table, fields can be added during data intergration.

    6.  Tunnel control.

        Tunnel control is used to control the speed and error rate when you select a intergration task. 

        -   DMU: Data migration unit, which measures the resources \(including the CPU, memory, and network\) consumed during data integration.
        -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data intergration task. 
        -   intergration speed: Maximum speed of the intergration task.
        -   Maximum error count: It is used to control the amount of dirty data, and is set by yourself based on the amount of synchronized data when the field types of the source table do not match those of the destination table. It indicates the maximum dirty data count allowed. If it is set to 0, no dirty data is allowed; if it is not specified, dirty data is allowed.
        -   Task resource group: To select a resource group where the current intergration node is located, you can add or modify the resource group on the data integration page.
4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit a node task.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment. 

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


