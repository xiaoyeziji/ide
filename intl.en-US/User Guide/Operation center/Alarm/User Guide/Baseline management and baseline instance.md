# Baseline management and baseline instance {#concept_uwf_rzn_42b .concept}

The baseline function involves the **Baseline Management** and **Baseline Instance** pages. On the **Baseline Management** page, you can create and define a baseline while on the **Baseline Instance** page, you can view baseline-relevant information.

## Baseline management {#section_tzy_vzn_42b .section}

1.  On the **Baseline Management** page, click **New Baseline** in the upper right corner to create a baseline.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16370/15367358307441_en-US.png)

2.  On the displayed page, set the baseline and click **determine** in the lower right corner to complete the creation.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16370/15367358307442_en-US.png)

    The configuration items are as follows:

    -   Project: the project to which a task associated with the baseline belongs.
    -   Baseline Type: determines whether the baseline is detected by day or hour. The option includes day baseline and hour baseline.
    -   Support Task: a task node associated with the baseline. Enter the task node name or ID and then click the icon behind to add the task node. You can add multiple task nodes.
    -   Priority: A baseline with a large number is scheduled at a higher priority.
    -   Estimated finish time: The expected completion time is estimated based on the average completion time of task nodes in the previous periodical scheduling.
    -   Commitment Time: An alert is triggered if the actual completion time is later than the difference of the commitment time minus the warning margin time.
3.  After a baseline is created, click **Enable** in the Operations column to enable the baseline function.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16370/15367358307443_en-US.png)


## Baseline instance {#section_my5_bb4_42b .section}

After a baseline is created, you need to enable the baseline function so that baseline instances can be generated. On the Baseline Instance page, you can search for instances by owner, baseline name, project name, or baseline status, and click **Details**, **deal with**, or **Gantt Chart** in the Operations column to perform operations.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16370/15367358307445_en-US.png)

The baseline status is described as follows.

-   Secure: A task is completed prior to the warning time.
-   Warning: A task is not completed after the warning time expires but the commitment time is not reached.
-   Breakage: A task is not completed yet after the commitment time expires.
-   Other: All tasks of a baseline are paused or the baseline has no task associated.

Operation buttons are described as follows.

-   Details: Click this button to go to the Baseline Management page.
-   deal with: The baseline that generates an alert stops reporting the alert within the handling time.
-   Gantt Chart: Click this button to view the key path of a task in a Gantt chart.

**Gantt chart** reflects the key path of a task. The chart displays the average running time of a task, task running status, task running history, and generated exception events. As shown in the following figure, the Gantt chart shows the key path of a task on the left side, the frame in light green shows the average running time of the task, and the frame in dark green shows the actual running time of the task.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16370/15367358307449_en-US.png)

