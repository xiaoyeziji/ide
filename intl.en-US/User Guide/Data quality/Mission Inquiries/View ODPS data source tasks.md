# View ODPS data source tasks {#concept_wq3_v43_r2b .concept}

The task query module allows you to query and view rule verification results. Rule run is the task run, where the rule run record can be viewed in the **Mission Inquiries** module.

1.  Visit the Data Quality Center, click **Mission Inquiries**, and enter the query page.
2.  Select the **ODPS data source** and, according to the search box, enter content to locate exactly the table you want to find.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16399/15368276558828_en-US.png)

    -   Display the task running state

        You can view the task execution status, number of rules, and number of exceptional rules in the task list. By clicking the hyperlinks on the right side, you can go to the relevant pages, and view details and make modifications.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16399/15368276558830_en-US.png)

    -   View Details of the partition expression

        Click the **details** of the corresponding task to enter the instance details page. This page shows the running status of all rules created for the current partition expression.

        -   Click **More** to view information about the data source, app name, node ID, and owner.
        -   Click **view history** after the corresponding field to view the running records after each schedule.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16399/15368276558832_en-US.png)

    -   Viewing rule configurations

        Click **Rules** after the corresponding task to jump to the rule configuration page. On this page, you can view and modify existing partition expressions and rules. See for details [Rules configuration for ODPS data source](intl.en-US/User Guide/Data quality/Rule Configuration/Rules configuration for ODPS data source.md#).

    -   View log

        Click the **log** after the corresponding task to view the running log for the current task.

    -   View data distribution

        Click **data distribution** after the corresponding task to view the task from creation to date, the situation of each run.


