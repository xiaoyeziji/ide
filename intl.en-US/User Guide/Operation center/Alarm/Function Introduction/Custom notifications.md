# Custom notifications {#concept_ndg_4yn_42b .concept}

Custom notification is a lightweight monitoring function of Alarm. Its design idea complies with the general monitoring system concept. All alert policies are set by you and the configuration covers the following.

-   Monitored object \(node, baseline, or project\)
-   Monitoring trigger condition \(error, complete, incomplete, or time-out\)
-   Alert method \(email, SMS\)
-   Alert object \(owner, duty schedule, or others\)
-   Maximum alert count \(maximum number of alerts triggered by an exception, after which the alert is no longer reported. The default value is 3\)
-   Minimum alert interval \(alert interval, which is 30 minutes by default\)
-   Alert do-not-disturb time

Monitoring trigger conditions are described as follows.

## Error {#section_vyg_syn_42b .section}

You can set alerts for errors occurring on tasks, baselines, or projects. Once a task has an error, an alert is sent to the preset alert object. Then, detailed task error information is pushed to a relevant user.

## Complete {#section_wyg_syn_42b .section}

You can set alerts for the completion of tasks, baselines, or projects. Once all tasks of an object are completed, an alert is sent. If alerts are set for the completion of baselines, an alert is sent when all tasks of a baseline are completed.

## Incomplete {#section_xyg_syn_42b .section}

You can set alerts for tasks, baselines, or projects that are not completed at a time point. For example, when the completion time of a baseline is set to 10:00, if any task of the baseline is not completed at 10:00, an alert is sent and the list of incomplete tasks is pushed to a relevant user.

## Time-out {#section_yyg_syn_42b .section}

You can set alerts for the time-out of tasks, baselines, or projects. If a monitored task on a preset object is not completed within specified time, an alert is sent.

