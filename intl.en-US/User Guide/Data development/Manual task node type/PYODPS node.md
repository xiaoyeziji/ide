# PYODPS node {#concept_ijm_dzl_q2b .concept}

DataWorks also provides the PYODPS task type and integrates the Python SDK of MaxCompute. You can directly edit the Python code to operate MaxCompute on a PYODPS node of DataWorks.

## Create a PYODPS Node {#section_e5k_pzl_q2b .section}

MaxCompute provides the [Python SDK](https://www.alibabacloud.com/help/doc-detail/34615.htm), which can be used to operate MaxCompute.

To create a PYODPS node, perform the following steps:

1.  Create a business flow

    Click **Manual Business Flow** in the left-side navigation pane, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15367340817961_en-US.png)

2.  Create a PYODPS node.

    Right-click **Data Development**, and select **Create Data Development Node** \> **PYODPS**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16322/15367340818036_en-US.png)

3.  Edit the PYODPS node.
    1.  ODPS portal

        On DataWorks, the PYODPS node contains a global variable odps or o, which is the ODPS entry.You do not need to manually define an ODPS entry.

        ```
        print(odps.exist_table('pyodps_iris'))
        ```

    2.  Run the SQL statements

        PYODPS supports ODPS SQL query and can read the execution result. The return value of the execute\_sql or run\_sql method is the running instance.

        **Note:** Not all commands that can be executed on the ODPS console are SQL statements that are accepted by ODPS. You need to use other methods to call non DDL/DML statements. For example, use the run\_security\_query method to call the GRANT or REVOKE statements, and use the run\_xflow or execute\_xflow method to call PAI commands.

        ```
        o.execute_sql('select * from dual') # Run the SQL statements in synchronous mode. Blocking continues until execution of the SQL statement is completed.
        instance = o.runsql('select * from dual') # Run the SQL statements in asynchronous mode.
        print(instance.getlogview_address()) # Obtain the logview address.
        instance.waitforsuccess() # Blocking continues until execution of the SQL statement is completed.
        ```

    3.  Configure the runtime parameters

        The runtime parameters must be set sometimes. You can set the hints parameter with the parameter type of dict.

        ```
        o.execute_sql('select * from pyodps_iris', hints={'odps.sql.mapper.split.size': 16})
        ```

        After you add sql.settings to the global configuration, related runtime parameters are added upon each running.python.

        ```
        from odps import options
        options.sql.settings = {'odps.sql.mapper.split.size': 16}
        o.execute_sql('select * from pyodps_iris') # "hints" is added based on the global configuration.
        ```

    4.  Read the SQL statement execution results

        The instance that runs the SQL statement can directly perform the open\_reader operation. In one case, the structured data is returned as the SQL statement execution result.

        ```
        with odps.execute_sql('select * from dual').open_reader() as reader:
        for record in reader: # Process each record.
        ```

        In another case, desc may be executed in an SQL statement. In this case, the original SQL statement execution result is obtained through the reader.raw attribute.

        ```
        with odps.execute_sql('desc dual').open_reader() as reader:
        print(reader.raw)
        ```

        **Note:** User-defined scheduling parameters are used in data development. If a PYODPS node is directly triggered on the page, the time must be clearly specified. The time of a PYODPS node cannot be directly replaced like that of an SQL node.

4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).

7.  Test in the production environment.

    For more information about the operation, see [Manual tasks](intl.en-US/User Guide/Operation center/Task list/Manual task.md#).


