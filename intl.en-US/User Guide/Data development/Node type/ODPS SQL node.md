# ODPS SQL node {#concept_cc4_1x3_p2b .concept}

This topic describes the ODPS SQL node functions. The ODPS SQL node syntax is similar to SQL, and is suited for distributed scenario with massive data volume at the TB-level, but has low real-time requirements. The ODPS SQL node is an OLAP application oriented throughput. We recommend you use ODPS SQL if your business needs to handle tens of thousands transactions because it requires a long period to complete the job process from preparation to submission.

1.  Create a business flow.

    Right-click **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15525307667651_en-US.png)

2.  Create ODPS SQL node.

    Right-click **Data Development**, and select **Create Data Development Node** \> **ODPS SQL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/15525307667679_en-US.png)

3.  Edit the node code.

    For more information about the SQL syntax statements, see [MaxCompute SQL](https://www.alibabacloud.com/help/doc-detail/27860.htm) statements.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/15525307667680_en-US.png)

4.  Query result display

    DataWorks query results are connected to the spreadsheet function, making it easier for users to operate the data results.

    The query results are displayed in spreadsheet style. Users can perform operations in DataWorks, open it in a spreadsheet, or freely copy content stations in local excel files.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16335/15525307668260_en-US.png)

    -   Hide column: Select one or more columns to hide the column.
    -   Copy row: Select one or more rows that need to be copied to the left side, and click Copy Row.
    -   Copy column: The top column selects a column or more points that need to be copied to the selected column.
    -   Copy: You can freely copy the selected content.
    -   Search: The search bar is displayed in the upper-right corner of the query results for facilitating data search in the table.
5.  Node scheduling configuration.

    Click **Schedule** on the right of the node task editing area to go to the Node Scheduling Configuration page. For more information about node scheduling configuration, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

6.  Submit the node.

    After the configuration is completed, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Publish a node task.

    For more information about the operation, see Release management.

8.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/O&M Center/Task list/Cyclic task.md#).


