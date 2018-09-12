# Data quality overview {#concept_zsz_44h_r2b .concept}

**Note:** Currently, Data Quality Center service is in the internal beta stage. It can be activated only in Shanghai region.

DataWorks Data Quality Center \(DQC\) is a one-stop platform supporting multiple heterogeneous data sources quality check, notifications, and management services.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16388/15367417308743_en-US.png)

Data Quality monitors DataSet. Currently, Data Quality supports monitoring of MaxCompute data tables and DataHub real-time data streams. When the offline MaxCompute data changes, the Data Quality verifies the data, and blocks the production links to avoid spread of data pollution. Furthermore, Data Quality provides verification of historical results. Thus, you can analyze and quantify data.

In the streaming data scenario, Data Quality can monitor the disconnections based on the Datahub data tunnel. For the first time, warning is sent to the subscriber. Data Quality also provides orange and red alarm levels, and supports alarm frequency settings to minimize redundant alarms.

This article briefly introduces the main interface components of Data Quality. The interface consists of four function modules, as follows:

-   **Overview**: By default, home page is the overview page that shows MaxCompute data tables alarms and blockings, DataHub Topic alarms, and current and historical tasks. Current tasks include personal subscriptions, alarms, and blockings for all tasks under the project. You can also browse historical tasks for last seven and last thirty days \(date range of up to three months\). Additionally, a quick way to go to the task query page is provided.
-   **My subscriptions**: The page shows the running status of all subscribed tasks. You can switch between MaxCompute and DataHub data sources to find subscribed tables or Topics. You can also change the notification method \(currently, email notification, and email and SMS notifications are supported\).

    Select MaxCompute data source, click partition expression on the right \(or select the DataHub data source, and click Topic name\) to enter the currently selected rule configuration interface.

-   **Rule configuration**: Rule configuration is the core function module of Data Quality. Using this module, you can manage the features related to partition expressions and rule configurations \(template rules and customized rules\).
-   **Mission Inquiries**: The task query module mainly queries the rule validation situation.

