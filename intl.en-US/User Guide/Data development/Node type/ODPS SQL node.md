# ODPS SQL node {#concept_cc4_1x3_p2b .concept}

ODPS SQL adopts the syntax similar to that of SQL, and is applicable to the distributed scenario in which the amount of data is massive \(TB-level\) but the real-time requirement is not high. It is an OLAP application oriented to throughput. Because it takes a long time to complete the process from preparation to submission of a job, ODPS SQL is recommended if a business needs to handle tens of thousands of transactions.

1.  Create a business flow.

    Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15367307877651_en-US.png)

2.  Create ODPS SQL node.

    Right-click **Data Development**, and select **Create Data Development Node** \> **ODPS SQL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/15367307877679_en-US.png)

3.  Edit the node code.

    For more information about the syntax of the SQL statements, see [MaxCompute SQL](https://www.alibabacloud.com/help/doc-detail/27860.htm) statements.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16293/15367307877680_en-US.png)

4.  Query result display

    DataWorks query results are connected to the spreadsheet function, making it easier for users to operate on data results.

    The query results are displayed directly in the style of a spreadsheet. Users can perform operations in DataWorks, open them in a spreadsheet, or freely copy content stations in local excels.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16335/15367307878260_en-US.png)

    -   Hidden column: select one or more columns hidden to hide the column.
    -   Copy the row: select one or more rows that need to be copied on the left side and click Copy the row.
    -   Copy the column: the column at the top selects a column or more points that need to be copied to copy the column.
    -   Copy: you can freely copy the selected content.
    -   Search: the search box will appear in the upper right corner of the query results to facilitate searching the data in the table.
5.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

6.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Publish a node task.

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).

8.  Test in the production environment.

    For more information about the operation, see [Cyclic tasks](intl.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


