# Data quality monitoring {#concept_fct_mps_s2b .concept}

The paper mainly discusses how to monitor the data quality in the process of using the data workshop, set up quality monitoring rules, monitor alerts and tables.

## Prerequisites {#section_q5v_qss_s2b .section}

Please complete the experiment[Data acquisition: log data upload](reseller.en-US/Best Practices/Workshop/Data acquisition: log data upload.md#) and [Data processing: user portraits](reseller.en-US/Best Practices/Workshop/Data processing: user portraits.md#)before proceeding with this experiment.

## Data quality {#section_ex5_nrs_s2b .section}

Data quality \(DQC \), is a one-stop platform that supports quality verification, notification, and management services for a wide range of heterogeneous data sources. Currently, Data Quality supports monitoring of MaxCompute data tables and DataHub real-time data streams. When the offline MaxCompute data changes, the Data Quality verifies the data, and blocks the production links to avoid spread of data pollution. Furthermore, Data Quality provides verification of historical results. Thus, you can analyze and quantify data quality. In the streaming data scenario, Data Quality can monitor the disconnections based on the DataHub data tunnel. Data Quality also provides orange and red alarm levels, and supports alarm frequency settings to minimize redundant alarms.

The process of using data quality is to configure monitoring rules for existing tables. After you configure a rule, you can run a trial to verify the rule. When the trial is successful, you can associate this rule with the scheduling task. Once the association is successful, every time the scheduling task code is run, the data quality validation rules are triggered to improve task accuracy. Once the subscription is successful, the data quality of this table will be notified by mail or alarm whenever there is a problem.

**Note:** 

The data quality will result in additional costs.

## Add Table Rule Configuration {#section_ely_qrs_s2b .section}

If you have completed the log data upload and user portrait experiments, you will have the following table: ods\_raw\_log\_d, ods\_user\_info\_d, ods\_log\_info\_d, dw\_user\_info\_all\_d, rpt\_user\_info\_d.

The most important thing in data quality is the configuration of table rules, so how to configure table rules is reasonable? Let's take a look at how the tables above be configured with table rules.

-   ods\_raw\_log\_d

    You can see all the table information under the item in the [data quality](https://dqc-cn-shanghai.data.aliyun.com/#/rule), now you are going to configure the data quality monitoring rules for the ods\_raw\_log\_d data sheet.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884689061_en-US.png)

    Select the ods\_raw\_log\_d table and click **Rule Configuration** to go to the following page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884689062_en-US.png)

    You can review the data sources for this ods\_raw\_log\_d table. The data for ods\_raw\_log\_d table is from ftp. Its partition is $\{bdp.system.bizdate\} format and is written into the table \("dbp.system.bizdate" is the date to get to the day before \).

    For this type of daily log data, you can configure the partition expression for the table. There are several kinds of partition expressions, and you can select dt = $ \[yyyymmdd-1\]. Refer to the documentation[Parameter configuration](../../../../reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) for detailed interpretation of scheduling expressions.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884689065_en-US.png)

    **Note:** 

    If there is no partition columns in the table, you can configure it as no partition. Depending on the real partition value, you can configure the corresponding partition expression.

    After confirm, you can see the interface below and choose to **create rules**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884689068_en-US.png)

    When you select to create a rule, the following interface appears.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884689070_en-US.png)

    Click **Add monitoring rule** and a prompt window appears for you to configure the rule.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699072_en-US.png)

    The data in this table comes from the log file that is uploaded by FTP as the source table. You need to determine whether there is data in this table partition as soon as possible. If there is no data in this table, you need to stop the subsequent tasks from running as if the source table does not have data, the subsequent task runs without meaning.

    **Note:** 

    Only under strong rules does the red alarm cause the task to block, setting the instance state to failure.

    When configuring rules, you need to select the template type as the number of table rows, sets the strength of the rule to strong. Click the **Save** button after the settings are completed.

    ![](images/9074_en-US.png)

    **Note:** 

    This configuration is primarily to avoid the situation that there is no data in the partition, which causes the data source for the downstream task to be empty.

    Rules test

    In the upper-right corner, there is a node test button that can be used to verify configured rules. The test button can immediately trigger the validation rules for data quality.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699075_en-US.png)

    When you click the test button, you are prompted for a window to confirm the test date. After a run is clicked, there will be a prompt information below telling you to jump to the test results by clicking prompt information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699077_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699078_en-US.png)

    **Note:** 

    According to the test results, the data of the Mission output can be confirmed to be in line with the expectations. It is recommended that once each table rule is configured, a trial operation should be carried out to verify the applicability of the table rules.

    When the rules are configured and the trial runs are successful, you need to associate the table with its output task. In this way, every time the output task of the table is run, the validation of the data quality rules is triggered to ensure the accuracy of the data.

    Associated Scheduling

    Data quality support being associated with scheduling tasks. After the table rules and scheduling tasks are bound, when the task instance is run, the data quality check is triggered. There are two ways to schedule table ruless:

    -   Perform table rule associations in operations center tasks.
    -   Association in the regular configuration interface for data quality.
    Operations Center Association Table rules

    In care center, in cycle tasks, locate the **ftp\_datasync** task, and in **more**, select **configure data monitoring**.

    ![](images/9080_en-US.png)

    Enter the monitored table name in the burst window, as well as the partition expression. The table entered here is named as ods\_user\_info\_d and the partition expression is dt = $ \[yyyymmdd-1\].

    Click **Configure** to quickly go to the rule configuration interface.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699087_en-US.png)

    Configure task subscriptions

    After the associated scheduling, every time the scheduling task is run, the data quality verification is triggered. Data quality supports setting up rule subscriptions, and you can set up subscriptions for important tables and their rules, set up your subscription to alert you based on the results of the data quality check. If the data quality check results are abnormal, notifications are made based on the configured alarm policy.

    Click **Subscription Management** to set up subscription methods. Mail notifications, email and SMS notifications are currently supported.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699104_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699105_en-US.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699106_en-US.png)

    After the subscription management settings are set up, you can view and modify them in **My Subscription**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699107_en-US.png)

    It is recommended that you subscribe to all rules so that the verification results are not notified in a timely manner.

-   ods\_user\_info\_d

    The data in the ods\_user\_info\_d table is from RDS database. When you configure rules, you need to configure the table to check the number of rows and the unique validation of the primary key to avoid duplication of data.

    Similarly, you need to configure a monitoring rule for a partition field first, and the monitoring time expression is: dt = $\[yyyymmdd-1\]. After successful configuration, you can see a successful partition configuration record in the partition expression that has been added.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699115_en-US.png)

    After the partition expression is configured, click **Create Rule** on the right to configure the validation rules for data quality. Add monitoring rules for table rows, rule intensity is set to strong, comparison mode is set to expectations greater than 0.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699121_en-US.png)

    Add column-level rules and set primary key columns to monitor columns. The template type is: the number of repeated values in the field is verified, and the rule is set to weak, the comparison mode is set to a field where the number of duplicate values is less than 1. After the setting is completed, click the bulk **Save** button.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884699123_en-US.png)

    **Note:** 

    This configuration is primarily designed to avoid duplication of data which may result in contamination of downstream data.

    **ods\_log\_info\_d**

    The data of this ods\_log\_info\_d table mainly is the analysis of the data in the table. Because the data in the log cannot be configured for excessive monitoring, you only need to configure the validation rules that is not empty for the table data. The partition expression for the first configuration table is: dt = $\[yyyymmdd-1\]

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884709126_en-US.png)

    The configuration table data is not an empty calibration rule, and the rule strength should be set to strong. The comparison is set to an expected value of not equal to 0, and after the setup is complete, click the **Save** button.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884709127_en-US.png)

-   dw\_user\_info\_all\_d

    This dw\_user\_info\_all\_d table is a summary of data for both the ods\_user\_info\_d table and the ods\_log\_info\_d table, because the process is relatively simple, the ODS layer is also configured with a rule that the number of table rows is not empty, so the table does not have the data quality monitoring rules configured to save on computing resources.

-   rpt\_user\_info\_d

    The rpt\_user\_info\_d table is the result table after the data aggregation. Based on the data in this table, you can monitor the number of table rows for fluctuations, and verify the unique values for primary keys. Partition expression for the first configuration table: dt = $\[yyyymmdd-1\]

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884709130_en-US.png)

    Then you may configure the monitoring rules: Click **Create rule** on the right, and click **Add Monitoring Rules** to monitor columns. The number of repeated values in the field is verified, and the rule is set to weak. The comparison mode is set to field repeat values less than 1.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884709133_en-US.png)

    Continue to add monitoring and table rules.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16423/15401884709135_en-US.png)

    As you may notice, the lower are the tables in the data warehouse, the more times the strong rules are set. That's because the data in the ODS layer is used as the raw data in the warehouse and you need to ensure the accuracy of its data, avoiding poor data quality in the ODS layer, and stop it in time.

    Data quality also provides an interface for task queries on which you can view the validation results for configured rules.


