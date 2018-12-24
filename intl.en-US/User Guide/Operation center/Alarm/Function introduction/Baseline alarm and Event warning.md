# Baseline alarm and Event warning {#concept_tdf_bvn_42b .concept}

This topic intuitively describes the logics of the baseline warning and event alarm functions in terms of the monitoring scope, task capture, alarm object judgment, alarm time judgment, alarm method judgment, and alarm escalation.

## Monitoring scope {#section_gzz_cvn_42b .section}

Tasks are put under monitoring through baselines \(a baseline is the management unit of a group of nodes, which can be understood as a node group for the ease of management\). After one baseline is put under monitoring, this baseline and all upstream tasks of the baseline are monitored. Alarm does not monitor all tasks by default but the downstream node of a monitored task must have tasks incorporated into a monitoring baseline. If a downstream node of the monitored task does not have tasks incorporated into a monitoring baseline, Alarm does not report an alarm even if the task has an error.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16366/15390801187435_en-US.png)

As shown in the above figure, assume that DataWorks has only six task nodes and Task D and Task E are incorporated into a baseline. Task D, Task E, and all their upstream nodes are included in the monitoring scope. That is, exceptions \(error or slowdown\), if any, occurring on Task A, Task B, Task D, and Task E can be spot by Alarm, but Task C and Task F are not monitored by Alarm.

## Task capture {#section_pbw_4wn_42b .section}

After the monitoring scope is determined, Alarm generates an event if any task within this monitoring scope has an exception. All alarm decisions are based on the analysis of this event. There are two types of task exceptions, you can select **Event Management** \> **Event Type** to view the task exceptions.

-   **Error**: a task running failure.
-   **Slowdown**: The running duration of a task is much longer in comparison with the average running duration of tasks in a previous time range.

**Note:** If a task times out and then encounters an error, two events are generated.

## Alarm object judgment {#section_qwt_zwn_42b .section}

After capturing an abnormal task and generating an event, Alarm determines the alarm object first as follows.

1.  Alarm checks whether the rule of the task has a duty schedule. If yes, Alarm considers the on-duty operator in the duty schedule as the alarm recipient.
2.  If no duty schedule exists, Alarm sets the task owner as the alarm recipient.

In the task rule, on-duty operators in the duty schedule serve as recipients of alarms using this task rule. Owners of some applications implement the on-duty system and specify an operator for receiving alarms in a period of time. If the duty schedule is absent, Alarm determines that the task owner is responsible for the exception.

## Alarm time judgment {#section_vbj_2xn_42b .section}

Alarm time involves a key concept **margin** in Alarming. Margin indicates the maximum allowable delay before a task is started.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16366/15390801187438_en-US.png)

Latest start time of a task = Baseline time – Average running time. As shown in the above figure, in order to meet the baseline time \(5:00\) of Baseline A, it is required to calculate the latest start time of Task E backwards. The latest start time of Task E is 5:00 minus the sum of the running time of Task F \(20 min\) plus the running time of Task E \(30 min\), that is, 4:10, which is also the latest completion time of Task B that meets Baseline A.

To meet the baseline time \(6:00\) of Baseline B, it is required to calculate the latest completion time of Task B backwards. The result is 6:00 minus the running time of Task D \(2 hours\), that is, 4:00, which is earlier than 4:10. If both Baseline A and Baseline B need to be met, the latest completion time of Task B is 4:00. The latest completion time of Task A is 4:00 minus the running time of Task B \(2 hours\), that is, 2:00. The latest start time of Task A is 2:00 minus the running time of Task A \(10 min\), that is, 1:50. If Task A cannot start at 1:50, it is difficult to meet Baseline A.

Assume that Task A has an error during running at 1:00. The margin time of Task A is the difference between 1:50 and 1:00, that is, 50 minutes. This example shows that the margin reflects the alarm level of a task exception.

## Baseline alarm {#section_p2l_zxn_42b .section}

Baseline alarm is an additional function targeted for baselines with the baseline function enabled, Each baseline must provide the warning margin and commitment time. When Alarm predicts that the baseline completion time is beyond the warning margin at a specific time, it directly notifies the alarm object of the case three times at an interval of 30 minutes. This is called baseline alarm.

## Alarm method {#section_q2l_zxn_42b .section}

You can set the alarm trigger mode and alarm behavior on the **Rule Management** page.

## Alarm escalation {#section_r2l_zxn_42b .section}

If you fail to close an event alarm on Alarm within 40 minutes, the alarm is escalated. The alarm escalation process is as follows:

1.  Alarm checks whether the rule of an abnormal task has a duty schedule. If yes, Alarm sends the alarm to the on-duty operator specified in the duty schedule.
2.  If no duty schedule exists, Alarm sends the alarm to the supervisor of the task owner.

You can close an alarm by closing the event on the homepage of Alarm.

## Gantt chart function {#section_s2l_zxn_42b .section}

The Gantt chart function is embedded in the **baseline instance** module of Alarm. It reflects the key path of a task.

**Note:** A key path is the slowest upstream link that causes the task completion at a time point.

