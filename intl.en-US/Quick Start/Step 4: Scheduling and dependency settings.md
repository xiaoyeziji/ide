# Step 4: Scheduling and dependency settings {#concept_c55_4zl_s2b .concept}

This article takes the "write\_result" created in [creating synchronization tasks](reseller.en-US/Quick Start/Step 3: Create a synchronization task.md#) as an example, configure its scheduling cycle as weekly scheduling, introduces the scheduling configuration and task operations features of DataWorks.

DataWorks provides powerful scheduling capabilities including time-based or dependency-based task trigger functions to perform **tens of millions** of tasks accurately and timely each day, based on DAG relationships. It supports scheduling by minutes, hours, days, weeks, and months. For more information, see [Create a synchronization task](../../../../../reseller.en-US/User Guide/Data development/Scheduling Configuration/Time attributes.md#).

## Procedure {#section_jy4_cbs_s2b .section}

**Configure the scheduling attribute of a synchronization task**

1.  Select **data development** \> **task Development** page.
2.  Double-click the synchronization task \(write\_result\) that you want to configure\).
3.  Click **schedule configuration** on the right to configure scheduling properties for the task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16183/15514203599000_en-US.png)

    Parameters:

    -   Scheduling status: The task is paused when this parameter is selected.
    -   Error retry: Error retry is enabled when this parameter is selected.
    -   Start date: The date that the task takes effect can be set based on requirements.
    -   Scheduling period: The operating cycle of the task can be set by month, week, day, hour, and minute. For example, a task can be scheduled weekly.
    -   Specific time: The specific task operating time. For example, you can set up the task to run at 02:00 every Tuesday.

**Configure dependency properties for a synchronization task**

After completing the synchronization task schedule properties configuration , you can configure its deployment dependency properties.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16183/15514203599001_en-US.png)

You can configure an upstream dependency for a task. In this way, even if the current task instance reaches the scheduled time, the task only run after the instance upstream task is completed.

The configuration in the preceding figure indicates the instances of the current task are triggered only after the upstream task write\_result is finished. You can enter **work** in the upstream task to configure an upstream task for write\_result.

If no upstream tasks is configured then, by default the current task is triggered by the project. As a result, the default upstream task of the current task is project\_start in the scheduling system. By default, a project\_start task is created as a root task for each project.

**Submit a data synchronization task**

Save the synchronization task **write\_result** and click **Submit** to submit it to the scheduling system.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16183/15514203599003_en-US.png)

The system automatically generates an instance for the task at each time point according to the scheduling attribute configuration and periodically runs the task from the second day only after a task is submitted to the scheduling system.

**Note:** If the task is submitted after 23: 30, the scheduling system automatically cycle-generate instances from the third day and run on time.

## Subsequent steps {#section_gnn_nds_s2b .section}

Now you know how to set a synchronization task scheduling attribute and dependency, now you can continue to the next topic to learn how to perform periodic O&M for submitted tasks and view the log troubleshooting results. For more information, see [cycle care operations and check for log ranking errors](reseller.en-US/Quick Start/Step 5: O&M and view log troubleshooting results.md#).

