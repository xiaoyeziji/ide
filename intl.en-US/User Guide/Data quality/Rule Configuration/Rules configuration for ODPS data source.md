# Rules configuration for ODPS data source {#concept_j2n_f13_r2b .concept}

This article introduces how to configure ODPS data source.

Rules configuration is the core function module of Data Quality. Data sources are divided into ODPS data source and DataHub data source.

## Select a data source {#section_tc4_h13_r2b .section}

1.  Click **Rules Configuration** in the left-side navigation pane to enter the Rules configuration page.
2.  Select **ODPS data source** to display all the tables in the project you have joined.

    You can also use the search box to find topics in other data sources quickly.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298793_en-US.png)

3.  Click **Configure monitoring rules**on the right side.

**Note:** 

Additionally, you can select **ODPS data source** in My subscriptions by [My subscription](intl.en-US/User Guide/Data quality/My subscription.md#) , and click **Partition expressions**on the right to enter the Rules configuration page.

## Configure the partition expression {#section_bby_bb3_r2b .section}

A partition expression is a filtering condition used to match a validation rule.

In the Rule Configuration page, click the plus sign**+** in the upper left corner to add a partition expression.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298796_en-US.png)

-   Expression for new partition: Click **+** in the upper left corner to pop up Add a partition, you can edit a syntax-compliant partition expression to suit your needs. Non-partition table can be directly selected **NOTAPARTITIONTABLE** in the recommended partition expressions list.
-   Format of the first-level partition expressions: Partition name = partition value. Partition value can be a fixed value or a built-in parameter expression.
-   Format of the multi-level partition expressions: First-level partition name = partition value / second-level partition name = partition value / N-level partition name = partition value. Partition value can be a fixed value or a built-in parameter expression.

## Built-in parameter expression {#section_hhv_sb3_r2b .section}

-   $\[yyyymmddmiss-1\]

    The format is `yyyymmddmiss-1`. The previous day’s \(year-month-day\) scheduled time of the daily scheduled instance; and is equal to the time \(year-month-day\) for the instance of the automatically scheduled daily node, minus 1 day.

-   $\[yyyymmddhh24miss\]

    The format is `yyyymmddhh24miss`. It specifies the scheduled time \(year-month-date-hour-minute-second\) for the routinely scheduled instance.

    -   Yyyy indicates 4-digit year
    -   Mm for 2-digit month
    -   DD for 2-digit days
    -   Hh24 is a 24-hour system.
    -   MI 2-digit minutes
    -   SS for 2-digit seconds

## Get +/- period method {#section_il4_xb3_r2b .section}

The partition expression cycle is determined by the configured run time, for example, the configuration run time is the first 5 days, the cycle is scheduled every 5 days.

-   N days before: $\{yyyymmdd-N\}
-   The 1st day of each month: $\{yyyymm01-1\}
-   The 1st day of N months before: $\{yyyymm01-Nm\}
-   The last day of each month: dt=$\{yyyymmld-1\}
-   The last day of N months before: dt=$\{yyyymmld-Nm\}
-   One hour ago: $ \[hh24miss-1/24\]
-   Half an hour ago: $ \[hh24miss-30/24/60\]

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298798_en-US.png)

-   Added partition expressions: Indicates the partition expressions already added to the table.
-   Recommended partition expressions: Indicates the partition expressions recommended by Data Quality. In the list of recommended partition expressions, you can find the partition expression that meets your requirements, and select to add it. When a recommended partition is successfully added to the table, it is displayed in the Added Partitions section.

    If you don't know if the recommended and custom expressions match your expectations, you can use the partition calculation function for calculations.

-   Delete partition expression: Partition expressions that are no longer used can be deleted. If the partition expression has been configured with rules, all rules under the expression are also deleted.

**Note:** 

In the following example, the partition name dt is taken as an example. If the table is a dynamic partition table, the use of a regular partition expressions is not recommended.

|Partition expression|Description|
|:-------------------|:----------|
|ALL\_PARTITIONS|This partition expression can be selected for non-partition tables.|
|dt = \[\[a-zA-Z0-9 \_-\] \*\>|The expression is generally used for hours tasks. If the table partition is an hour partition, it automatically replaces the regular expression with the partition expression.|
|dt=$\[yyyymmdd-N\]|Indicates N days before.|
|dt=$\[yyyymm01-1\]|Indicates the 1st day of each month.|
|dt=$\[yyyymm01-Nm\]|Indicates the 1st day of N months before.|
|dt=$\[yyyymmld-1\]|Indicates the last day of N months before.|
|dt=$\[yyyymmld-1m\]|Represents the last day of N months ago.|
|dt=$\[hh24miss-1/24\]|Represents an hour ago.|
|dt=$\[hh24miss-30/24/60\]|Representing half an hour ago.|

Click the input expression window, and the recommended partition expressions are displayed in the drop-down list.

-   If an appropriate expression is in the list, click the line to automatically synchronize it to the output window.
-   If none of partition expressions meet your requirements, you can input partition expressions as needed.

After the operation is complete, click **Calculate**. Data Quality calculates the value of partition expressions according to the current time \(scheduled time\) to verify the correctness of the partition expressions.

Click **Ok**to complete the operation.

## Associated scheduling {#section_qym_j23_r2b .section}

To monitor offline data on the production links, you must use Data Quality associated scheduling feature. Сurrently, there are two associated portals: one in the Operation and maintenance center and the other on the Data Quality Rules configuration page.

You can add associated scheduling to the already created Task node. After associating with the schedule, the task run automatically \(you can also select not to associate, and skip this step\). Currently, associated scheduling supports Operation and maintenance center.

**Note:** 

You can enter Operation Center to set the associated scheduling quality monitoring configuration.

1.  Click**more** in the corresponding task tab, and select **Quality monitoring configuration** .
2.  Input the name of the table, and click **Configuration** in the corresponding partition expression tab \(you can also add a partition expression by yourself\) to configure this partition expression.

## Create rules {#section_jdy_hh3_r2b .section}

Creating rules according the actual needs of the table is the core function module of Data Quality.

Currently, rules can be created in two ways: Template rules and Customized rules, specific usage depends on the actual needs. These two kinds of rules are divided into **Add monitoring rules** and **Quick add**.

After creating the rules, click **Save batches**, you can save all the rules to the already created partition expressions.

**Template rules**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298809_en-US.png)

-   **Add monitoring rules**
    -   Field type: Consists of table-level rules and field-level rules. The field-level rules configure monitoring rules for specific fields in the table. The table-level rules are selected here, and other setting items in the interface correspond to the table-level rules configuration.
    -   Intensity: You can configure the intensity of the rule. For example, when strong is selected, if the red threshold is triggered while the task is running, the task is set to fail.
    -   Template type: The system has a built-in table-level monitoring rules module.
    -   Tendency: Depending on the type of template selected, tendency can include the following types: absolute value, increasing, and decreasing.
    -   Comparison of fluctuation values: Set the orange and red thresholds of the fluctuation value. You can manually drag the progress bar, or directly input the threshold value.
-   **Quick add**
    -   Field name: Can be used only for field-level rules. Field-level rules configure monitoring rules for specific fields in the table. Select specific fields to set the field-level rules.
    -   Rule type: Select the field null value or field repetition value.

If the template rules do not meet your requirements for partition expressions quality monitoring, you can use customized rules to create the custom monitoring rules.

**Customized rules**

On the Customized rules page, you can select to create table-level rules or custom SQL.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298814_en-US.png)

-   **Add monitoring rules**
    -   Field Type: Consists of table-level rules, field-level rules, and custom SQL. The table-level rules are selected here, and other settings items in the interface correspond to the table-level customized rules configuration.
    -   Intensity: When strong is selected, if the red threshold is triggered while the task is running, the task is set to fail.
    -   Statistical functions: Include two types: count and count/table\_count.
    -   Filter conditions: Custom SQL.
    -   Verification method: The built-in verification method can be selected. The verification method defaults to a fixed value.
    -   Tendency: Includes three types: absolute value, increasing, and decreasing. If the statistical function is set to count/table\_count, the tendency defaults to a fixed value.
    -   Comparison method: According to the actual needs, there are many options: greater than, greater than or equal to, equal to, not equal to, less than, less than or equal to.
    -   Expected value: The expected target value.
    -   Description: The detailed description of the customized rule.
-   **Quick add**
    -   Rule type: Includes two types: Number of rows in the table is greater than null and Multiple fields repetition value.
    -   Field name: When the rule type is Multiple fields repetition value, the field names that must be added are displayed, and the multiple field names can be added.

## Test run {#section_bpf_sk3_r2b .section}

After the rules are configured, you can perform a test run for all the rules under a partition expression, and view the test run results.

1.  Select the required scheduling date, and click **Test run**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298816_en-US.png)

    -   **Test run partition**: the actual partition changes with the change of business date. If NOPARTITIONTABLE, the actual partition is automatically added.
    -   **Scheduling time**: The default is the current time.
2.  Click **test run success! Click Trial Run Success**，Click to view the test run results, and go to the [task query](intl.en-US/User Guide/Data quality/Mission Inquiries/View ODPS data source tasks.md#) page to check the results.

## Change the responsible person {#section_fj5_4c4_r2b .section}

When the responsible person leaves or changes job, person in charge of the partition expressions can be changed with another project member. Place the mouse over the responsible person, and the hidden button is displayed.

Place the mouse over the responsible person, and the hidden button is displayed. Click to modify the responsible person, input the name of the new person in charge, and click**Confirm** to submit.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267298817_en-US.png)

## More {#section_atk_vc4_r2b .section}

Option **More** includes the following options: Partition operations logs, Last verification results, and Copy rules.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267308818_en-US.png)

-   **Partition operations logs**: Displays a record of all the rule settings for the current partition expression.
-   **Last verification results**: Redirects to the the task query interface where you can view the running results under the current partition expression. You can also check the historical results.
-   **Copy rules**: You can copy the currently set rules into the target expression, and the transmissions can be synchronized.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16396/15368267308821_en-US.png)


For more information about template rules supported by ODPS data source, see [Template rule](intl.en-US/User Guide/Data quality/Template rule.md#).

