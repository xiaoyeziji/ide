# Rule Management {#concept_rn2_4d4_42b .concept}

This article show you how to customize alarm rules on the **Rule Management** page.

1.  On the **Rule Management** page, click **Create a new custom rule** on the right side to define alarm policies. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16372/15367358977454_en-US.png)

2.  In the displayed **Basic information** dialog box, enter the policy name, policy object, trigger method, and alarm behavior, and click **determine** to generate a policy.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16372/15367358977455_en-US.png)

    The configuration items are described as follows.

    -   Object Type: controls the monitoring granularity. A baseline, project, or task node can be selected as a monitored object.
    -   Trigger Condition: It can be set to complete, incomplete, error, or time-out.
    -   Minimum alarm Interval: a time interval between two alarms.
    -   Maximum alarm Count: maximum number of alarms, after which the alarm is not reported regardless of the status of the monitored object.
    -   Recipient: alarm object, which can be set to owner, duty schedule, or others.
    -   Do-Not-Disturb Time: No alarm is sent within this period of time.
3.  After completing the preceding settings, you can click **Details** in the Operations column of a policy on the **Rule Management** page to view rule details.

