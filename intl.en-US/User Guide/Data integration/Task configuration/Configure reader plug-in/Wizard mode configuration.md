# Wizard mode configuration {#concept_qrh_r2c_p2b .concept}

This topic describes how to configure tasks through the Data Integration Wizard mode.

The steps for task configuration are as follows:

1.  Create a data source.
2.  Create a synchronization task and configure the synchronization task reader.
3.  Configure the synchronization task writer.
4.  Configure the mapping between the synchronization task reader and the synchronization task writer.
5.  Configure the concurrency, transmission rate, dirty data records, resource groups, and other information of the synchronization task.
6.  Configure the scheduling attribute of the synchronization task.

**Note:** The following is an introduction to specific operation step implementation, where each of the following steps directs to the corresponding topic. When you complete a step, to continue onto the next step click the link to return to this topic.

## Create data source {#section_vkd_w2c_p2b .section}

Synchronization tasks support data transmission between different homogenous and heterogeneous data sources. First, register the target data source in Data Integration. Then you can select the data source directly when configuring a synchronization task on Data Integration. For more information on the synchronous data source types supported by Data Integration, see [Supported data sources](intl.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

Confirming the target data source is supported by Data Integration, after you can register the data source in Data Integration. For more information on data source registration, see [Configuring data source information](https://www.alibabacloud.com/help/faq-list/72788.htm).

**Note:** 

-   Data Integration does not support test connectivity for certain data sources. For more information on data source test connectivity, see [Test data source connectivity](intl.en-US/User Guide/Data integration/Data source configuration/Test data source connectivity.md#).
-   Data sources are frequently created locally and cannot be connected without a public network IP address or network. In this case, testing connectivity at the time of the data source configuration might fail. Data Integration supports [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#) to solve network inaccessibility.

## Create a synchronization task and reader {#section_tfn_1kc_p2b .section}

**Note:** This topic mainly describes how to synchronize task configuration in Wizard Mode. Select **Wizard Mode** when creating new synchronization tasks.

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console) as a developer, and click **Data Development** in the corresponding project Action column.
2.  Click **Data Development** in the left-hand navigation pane to open the Business Process navigator.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717611_en-US.png)

3.  Right-click **Business Flow** in the navigation pane to create **Data Integration Node** \> **Data Sync**, and enter the synchronization task's name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717612_en-US.png)

4.  After creating the synchronization task, you can manually configure the reader data source and the target table information for the data synchronization task. For more information on how to select a data source to read from, see [Configuring Reader](https://www.alibabacloud.com/help/faq-list/49806.htm).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717614_en-US.png)


**Note:** Incremental data synchronization is required for many tasks when configuring read-side data sources. You can now obtain relative date in conjunction with [Parameter configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) to complete the requirement to obtain the incremental data.

## Configure the Writer {#section_nz2_jlc_p2b .section}

After the reader data source is configured, you can manually configure the Writer data source and the target table information for the data synchronization task. When you are selecting the data source to write on, see [Configure Writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#).

**Note:** You need to select a Write Mode based on data sources, such as overwrite mode or append mode for most tasks. For students with write control requirements, refer to the [Configure Writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#) documentation to select the Write Mode.

## Configure mapping {#section_h35_qlc_p2b .section}

When you complete the configuration for both read/write, you need to specify a mapping relationship between the read/write end columns, and select **Map of the same name** or **Enable same-line mapping**.

-   Enable same-line mapping: Automatically sets the mapping relationship for the same row of data.
-   Automatic layout: The field order is displayed after the mapping relationship is set.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717615_en-US.png)

**Note:** The field types mapped between columns should be data compatible.

## Task synchronization channel configuration {#section_s3r_zlc_p2b .section}

When the preceding steps are configured, the efficiency configuration is required. The efficiency configuration mainly includes DMU settings, synchronous concurrency number settings, synchronous rate settings, synchronous dirty data settings and synchronize information, such as Resource Group settings.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717616_en-US.png)

Parameters:

-   DMU: The billing unit for data integration.

    **Note:** When you set up a DMU, please note the DMU value limits the maximum number of concurrency. Please configure accordingly.

-   When you configure the Synchronization Concurrency, the data records are separated into several tasks based on the specified reader shard key. These tasks run simultaneously to improve the transmission rate.
-   Synchronous rate: Setting the synchronous rate protects the read-side database from fast extraction speed, and places too much pressure on the source library. It is recommended to throttle the synchronization rate and configure the extraction rate based on the source database configurations.
-   For example, if the source has varchar type data, but is written to a destination column with INT type data. A data conversion exception occurs, and the data cannot be written to the destination column. The dirty data is mainly set to control the synchronized data quality. You should set the number of dirty data based on business requirements.
-   When you configure a synchronization task, you specify the Resource Group in which the task runs. By default, the synchronization task runs on the Default Resource Group. When the project has a tight schedule of resources, you can also expand a scheduled resource by adding a Custom Resource Group. The synchronization task is then specified to run on a Custom Resource Group. For more information, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#) . You can make configurations based on data source network conditions, project scheduling resource conditions, and business importance.

**Note:** When synchronizing data is inefficient, see [Optimizing configuration](intl.en-US/User Guide/Data integration/Task configuration/Optimizing configuration.md#) to optimize your synchronization tasks.

## Scheduling parameters {#section_dhv_xmc_p2b .section}

Use scheduling parameters to filter synchronization task data. The following figure shows how to configure scheduling parameters in the synchronization task.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717617_en-US.png)

In the preceding figure, you can declare a schedule parameter variable in the form of a $\{variable name\}. When the variable declaration is complete, write the initialization value of the variable in the scheduled parameter properties, the value initialized here by the variable is represented with a dollar sign \($\). The content can either be a time expression or a constant.

For example, $\{today\} was written in code, by assigning today = $ \[yyyymmdd\] in the scheduling parameter, you can obtain the current date. For more information on how to add or minus a date, see [Parameter configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

## Using custom schedule parameters in synchronization tasks {#section_scv_jnc_p2b .section}

Declare the following parameters in the code of your synchronization task.

-   bizdate: Obtain business date, and run date-1.
-   cyctime: Obtain the current run time, in the form of yyyymmddhhmiss.
-   DataWorks provides two system default scheduling parameters: bizdate and cycletime.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15516845717618_en-US.png)

## Configure scheduling properties {#section_a4t_rnc_p2b .section}

You can set the synchronization task run cycle, run time, task dependency, and more in the scheduling properties. Because the synchronization task starts the ETL task, there are no upstream nodes. It is recommended to use the project root node for upstream settings.

Save the node after completing the synchronization task configuration , and click Submit.

