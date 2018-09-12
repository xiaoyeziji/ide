# Event Management {#concept_e5p_fc4_42b .concept}

The **Event Management** page lists all **slowdown** and **error** events. You can search for events by owner, name/ID of task node or instance, or event discovery time, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16371/15367358647451_en-US.png)

In the search results, each row indicates one event \(associated with an abnormal task\). The worst baseline indicates a baseline with the minimum margin among the baselines affected by this event.

-   Click **Details** in the Actions column of an event to view the event details.
-   Click **deal with** to record the event handling operation and pause the alarm in the operation period.
-   Click **Ignore** to record the event ignorance record and stop the alarm permanently.

As shown in the following figure, after **Details** is clicked, the event generation time, alarm time, clearing event, previous running record of the task, and detailed task logs are displayed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16371/15367358647452_en-US.png)

The actual alarm recipient is the person whom an alarm is assigned to. You can click **Alarm Info** to redirect to the alarm details page of an event. Baseline influence displays all downstream baselines affected by tasks related to the event. You can check downstream baselines and baseline breaking severity, in combination with task logs, to investigate causes for the event.

