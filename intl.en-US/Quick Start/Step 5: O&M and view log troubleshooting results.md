# Step 5: O&M and view log troubleshooting results {#concept_whk_pzl_s2b .concept}

This topic describes how to implement task operations.

In the previous operations, you set a synchronization task to run at 02:00 every Tuesday. After the task is submitted, you can view the automatic operation results in the scheduling system the next day.

To check whether the instance schedule and dependency are operating as expected, DataWorks provides three triggering methods: test run, data population, and periodic running, which are described as follows:

-   Test run: The task is triggered manually. If you need to check the timing and operation of a single task, test run is recommended.
-   Data population: The task is triggered manually. This method applies if you need to check the timing and dependencies of multiple tasks or re-execute data analysis and computing from a root task.
-   Periodic running: The task is triggered automatically. After successful submission, the scheduling system automatically generates task instances at different time points starting from 00:00 the next day. It checks whether upstream instances of each instance can run successfully according to the scheduled time. If all the upstream instances run successfully at the scheduled time, the current instance runs automatically.

**Note:** The scheduling system periodically generates instances based on the same rules that apply to both manual and automatic triggering modes.

-   The period can be set to monthly, weekly, daily, hourly, or even by minutes. The scheduling system always generates an instance for the task on a specified day or at a specified time.
-   The scheduling system regularly runs the instance on a specified date and generates operation logs.
-   Instances rather than a specified date does not run, and their statuses are directly changed to “Successful” if the running conditions are met. Therefore, no running logs are generated.

For more operational and functional instructions, see [Task operations](../../../../../reseller.en-US/User Guide/O&M Center/Task O&M/Cycle instance.md#).

## Test {#section_hmf_k2s_s2b .section}

**Manually trigger a test**

1.  On the Cycle Task page, locate the task that you want to run, and click **Test**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719007_en-US.png)

2.  Enter the business Date and click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719008_en-US.png)

3.  Go to the Basic information page to view the task run status.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719009_en-US.png)


**View the information and operation logs of the test instance**

You can see the instance DAG graph by selecting the appropriate task instance in the test instance page and clicking.

-   Right-click an instance, you can view the instance's dependency details and perform specific actions such as stop, resume, and more.
-   Double-click an instance to enter the pop up task properties, run log, operation log, code, and so on.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719010_en-US.png)


**Note:** 

-   In test run mode, the task is triggered manually. The task runs immediately as long as the set time is reached, regardless of the instance's upstream dependencies.
-   The task write\_result is configured to run every Tuesday morning, based on the instance generation rules described earlier in the topic. The business date selected by the test Runtime is Monday \(business date = run date-1\), the instance will actually run at 2. If it is not Monday, the instance is converted to a successful state at 2 points, and there is no log generation.

## Replenishment data operation {#section_idd_mhs_s2b .section}

**Manually trigger data population**

If you need to confirm the timing and dependencies of multiple tasks, or need to re-perform the data analysis calculation from a root task, you can select the **O&M center** \> **task list** \> **cycle task** page and click the **replenishment data** after the task, to enter multiple tasks scheduled at a certain period of time.

1.  Select the **O&M center** \> **cycle task** page and enter the task name.
2.  Click **replenishment data**after the query results.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719016_en-US.png)

3.  Set the business date for the replenishment data as "to", select the write\_result node task, and click **OK**.
4.  Click to **view the replenishment data results**.

    ![](images/9018_en-US.png)


**View the information and operation logs of the data population instance**

You can see the instance DAG graph by selecting the appropriate task instance.

-   Right-click an instance, you can view the dependencies and details of this instance and perform specific actions such as stop, resume, and more.
-   Double-click an instance to pop up task properties, run log, operation log, code, and so on.

    ![](images/9019_en-US.png)


**Note:** 

-   2017-09-18 15:56:30. 919 \[job-51109647\] is the job ID in the preceding figure.
-   In the preceding figure, the task failed because the source does not have the partition value in the synchronized table, resulting in a read error.
-   The instance of a replenishment data task is day-to-day. For example, the task runs from 2017-09-15 to 2017-09-18 , if the instance number 15 fails during this period, an instance of number 16 also will not run.
-   The task write\_result is configured to run every Tuesday morning, and based on the instance generation rules described earlier in the article. The business date selected by the replenishment data Runtime is Monday \(business date = run date-1\). The instance will run at 2 AM. If it is not Monday, the instance is converted to a successful state at 2 AM, and no log is generated.

## Periodic automatic run {#section_frv_rjs_s2b .section}

Under periodic automatic run mode, the scheduling system automatically triggers tasks according to all task scheduling configurations. Therefore, no operation portal is provided. You can view the instance information and operation logs by using either of the following methods.

-   Select the parameters such as the business date or the running date on the **O&M center** \> **cycle instance** page, search for the instance that corresponds to the write\_result task, and then right-click on the instance information and the run log.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719020_en-US.png)

-   You can see the instance DAG graph by selecting the appropriate task instance in the cycle instance page and clicking.

    -   Right-click an instance, you can view the dependencies and details of this instance and perform specific actions such as stop, resume, and more.
    -   Double-click an instance to pop up task properties, run log, operation log, code, and more.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16184/15514203719021_en-US.png)

    **Note:** 

    -   The task is not running because the upstream task is not running.
    -   If the initial state of an instance of a task is “Not Run,” when the scheduled time arrives, the scheduling system checks all upstream instances of this instance are running successfully.
    -   The instance will be triggered only when all of its upstream instances are successful and its scheduled time is reached.
    -   For a Not Run status instance , check all its upstream instances are successful and has reached its scheduled time.

