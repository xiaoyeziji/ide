# PyODPS node {#concept_d5y_vhl_p2b .concept}

DataWorks also provides the PyODPS task type and integrates the Python SDK of MaxCompute. You can directly edit the Python code to operate MaxCompute on a PyODPS node of DataWorks.

MaxCompute provides the [Python SDK](https://www.alibabacloud.com/help/doc-detail/34615.htm), which can be used to operate MaxCompute.

**Note:** Python 2.7 is used at the underlying layer. The size of data that PyODPS nodes process should not exceed 50MB, while the memory they occupiy should not exceed 1GB.

## Create a PyODPS node {#section_eyd_w3l_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15451266807651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **PyODPS**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16295/15451266807741_en-US.png)

3.  Edit the PyODPS node.

    1.  ODPS portal

        On DataWorks, the PyODPS node contains a global variable odps or o, which is the ODPS entry.You do not need to manually define an ODPS entry.

        ```
        print(odps.exist_table('PyODPS_iris'))
        ```

    2.  Run the SQL statements

        PyODPS supports ODPS SQL query and can read the execution result. The return value of the execute\_sql or run\_sql method is the running instance.

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
        o.execute_sql('select * from PyODPS_iris', hints={'odps.sql.mapper.split.size': 16})
        ```

        After you add sql.settings to the global configuration, related runtime parameters are added upon each running.python.

        ```
        from odps import options
        options.sql.settings = {'odps.sql.mapper.split.size': 16}
        o.execute_sql('select * from PyODPS_iris')  # "hints" is added based on the global configuration. 
        ```

    4.  Read the SQL statement execution results

        The instance that runs the SQL statement can directly perform the open\_reader operation. In one case, the structured data is returned as the SQL statement execution result.

        ```
        with o.execute_sql('select * from dual').open_reader() as reader:
        for record in reader:  # Process each record.
        ```

        In another case, desc may be executed in an SQL statement. In this case, the original SQL statement execution result is obtained through the reader.raw attribute.

        ```
        with o.execute_sql('desc dual').open_reader() as reader:
        print(reader.raw)
        ```

        **Note:** User-defined scheduling parameters are used in data development. If a PyODPS node is directly triggered on the page, the time must be clearly specified. The time of a PyODPS node cannot be directly replaced like that of an SQL node.

    You can configure system parameters like this.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16295/154512668034264_en-US.png)

    You can configure user-defined parameters like this.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16295/154512668034268_en-US.png)

4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in the production environment.

    For more information about the operation, see [Cyclic task](intl.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


