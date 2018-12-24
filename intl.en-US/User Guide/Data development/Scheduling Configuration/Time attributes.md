# Time attributes {#concept_srx_kjw_p2b .concept}

The time attribute configuration page is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628607912_en-US.png)

## Node states {#section_dtf_fvw_p2b .section}

-   Normal: Nodes are normally scheduled based on the following scheduling cycle. This option is selected by default.
-   Zero-load: After this option is selected, nodes are configured and scheduled based on the following scheduling cycle. However, once this task is scheduled, a success is directly returned without executing the task.
-   Error retries: the node has encountered an error, and the node can be rerun. Default error automatically retries 3 times, time interval 2 minutes.
-   Suspend scheduling: After this check box is selected, nodes are configured and scheduled based on the following scheduling cycle. However, once this task is scheduled, a failure is directly returned without executing the task. It is used when a task is suspended but will be executed later.

## Scheduling interval {#section_fpz_kvw_p2b .section}

In DataWorks, when a task is successfully submitted, the underlying scheduling system generates an instance every day starting from the next day based on the time attributes of the task, and runs the instances based on the running results and time points of the depended upstream instances. For a task that is successfully submitted after 23:30, the instances are generated starting from the third day.

**Note:** 

If a task needs to run on every Monday, the task runs only when the runtime is Monday. If the runtime is not Monday, the task \(which is directly set to successful\) runs pretendedly. For this reason, select Business date = Runtime -1 for weekly scheduled tasks during test or data supplement run.

For a task that runs cyclically, the priority of its dependency is higher than that of its time attribute. This means that, when the time specified by its time attribute reaches, the task instance does not run immediately but first checks whether all the upstream instances have run successfully.

-   If not all the depended upstream instances run successfully and the scheduled runtime is reached, the instance remains in the not running status.
-   If not all the depended upstream instances run successfully and the scheduled runtime is reached, the instance remains in the not running status.
-   If all the depended upstream instances run successfully and the scheduled runtime is reached, the instance enters the waiting for resource status to be ready for running.

## Daily scheduling {#section_o21_jrx_p2b .section}

Daily scheduled tasks run automatically once every day. When you create a cyclic task, the task is set to run at 00:00 every day by default. You can specify another runtime as needed. For example, you can specify the runtime as 13:00 every day, as shown in the following figure.

1.  If Regular Scheduling is deselected, the scheduled time of instances of the daily task is the date of the current day in YYYY-MM-DD and the default scheduling time that is randomly generated between 0:00 and 0:30.
2.  If Regular Scheduling is selected, the scheduled time of instances of the daily task is the date of the current day in YYYY-MM-DD and the scheduled time in HH:MM:SS. A scheduled task can run only when the upstream task successfully runs, and the scheduled time is reached. If either condition is not met, the task cannot run. The conditions do not have the order.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628607913_en-US.png)

Use cases:

Import, statistical processing, and export tasks are all daily tasks with the runtime of 13:00, as shown in the preceding figure. Statistical processing tasks depend on import tasks, and export tasks depend on statistical processing tasks. The following figure shows the configuration of their dependencies\(In the dependency attribute configuration for the statistical processing tasks, the upstream task is set to import task\).

Based on the configuration in the preceding figure, the scheduling system automatically generates instances for the tasks and runs them as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628607915_en-US.png)

## Weekly scheduling {#section_lz2_btx_p2b .section}

Weekly scheduled tasks automatically run at specific time points of specific days each week. When an unspecified date reaches, the system also generates instances and directly sets them as successfully running without running any logic or consuming any resource to ensure the proper running of downstream instances.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617916_en-US.png)

As shown in the preceding figure, instances generated on every Monday and Friday run as scheduled, and other instances generated on every Tuesday, Wednesday, Thursday, Saturday, and Sunday are directly set as successfully running.

Based on the configuration in the preceding figure, the scheduling system automatically generates instances for the tasks and runs them as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617917_en-US.png)

## Monthly scheduling {#section_bgl_j5x_p2b .section}

Monthly scheduled tasks run automatically at specific time points of specific days each month. When an unspecified date reaches, the system also generates instances every day and directly sets them as successfully running without running any logic or consuming any resource to ensure the proper running of downstream instances.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617918_en-US.png)

As shown in the preceding figure, instances generated on the first day of each month run as scheduled, and instances generated every day for the rest days of the month are directly set as successfully running.

Based on the configuration in the preceding figure, the scheduling system automatically generates instances for the tasks and runs them as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617919_en-US.png)

## Hourly scheduling {#section_lfn_1vx_p2b .section}

Hourly scheduled tasks run every N x 1 hours in a specific period each day, such as running every one hour every day from 1:00 to 4:00.

**Note:** The running interval is calculated based on the left-close and right-close principle. For example, if an hourly scheduled task is configured to run every one hour between 0:00 and 3:00, it indicates that the time period is \[00:00, 03:00\], and the interval is one hour. The scheduling system generates four instances every day, which run at 0:00, 1:00,2:00 and 3:00.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617920_en-US.png)

As shown in the preceding figure, an automatic scheduling is triggered every six hours every day from 00:00 to 23:59. Therefore, the scheduling system automatically generates instances for the task and runs them as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617921_en-US.png)

## By-minute scheduling {#section_s5w_svx_p2b .section}

By-minute scheduled tasks run every N x 1 minutes in a specific period each day, as shown in the following figure:

The task is scheduled every 30 minutes from 00:00 to 23:00 each day.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617922_en-US.png)

Currently, by-minute scheduling supports the granularity of at least five minutes. The time expression must be selected and cannot be manually modified.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617923_en-US.png)

## FAQ {#section_q3v_qwx_p2b .section}

Q: If my upstream task A is an hourly scheduled task and downstream task B is a daily scheduled task, and task B needs to be executed once each day after task A is completed, can tasks A and B be mutually dependent?

A: A daily task can depend on an hourly task. If task A is configured as an hourly scheduled task, task B is configured as a daily task that is irregularly scheduled, and tasks A and B are mutually dependent, task B can run after task A successfully runs instances for 24 hours each day. \(For more information about the dependency configuration, see the scheduling dependency description\). Therefore, tasks of each cycle can depend on each other, and the scheduling cycle of each task is determined by the time attribute of the task.

Q: I want my task A to run once each hour and task B to run once each day, and task B starts to run after the first time that task A successfully runs. How can I configure it?

A: When configuring task A, you need to select Previous Cycle Dependent and Current Node, and set the scheduled time of task B to 0:00. In this way, instances of task B in the automatically scheduled instances each day only depend on the 0:00 instance of task A, that is, the first instance of task A.

Q: If task A runs on every Monday and task B depends on task A, how can I configure to enable task B to run on every Monday?

A: You can set the time attribute of task B the same as that of task A, that is, you need to set the scheduling cycle to Weekly Scheduling and Monday.

Q: Are the instances of a task affected when the task is deleted?

A: When a task is deleted after running for a period, its instances are remained because the scheduling system still generates one or more instances for the task according to the time attribute.  For this reason, when the instances are triggered after the task is deleted, the following error message is displayed because the required code cannot be found:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16302/15422628617924_en-US.png)

Q: What can I do if I want to calculate monthly data on the last day of each month?

A: Currently, the system does not support setting the runtime as the last day of each month. Therefore, if the task is set to run on the 31st day of each month, scheduling is triggered on one day for the month having 31 days, and instances are generated and directly set as successfully running on other days.

For monthly statistics, we recommend that you calculate the data for the previous month on the first day of each month.

