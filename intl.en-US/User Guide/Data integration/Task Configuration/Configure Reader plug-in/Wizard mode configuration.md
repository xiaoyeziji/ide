# Wizard mode configuration {#concept_qrh_r2c_p2b .concept}

This article will show you how to configure tasks through the Data Integration wizard mode.

The steps for task configuration are as follows:

1.  Create data source
2.  Create a synchronization task and configure the synchronization task reader.
3.  Configure the synchronization task writer.
4.  Configure the mapping between the synchronization task reader and the synchronization task writer.
5.  Configure the concurrency, transmission rate, dirty data records, resource groups, and other information of the synchronization task.
6.  Configure the scheduling attribute of the synchronization task.

**Note:** You will be introduced to the specific implementation of the operation steps, each of the following steps jumps to the corresponding guidance document, after completing the current step, click the link back to this article to continue to the next step.

## Create data source {#section_vkd_w2c_p2b .section}

Synchronization tasks support data transmission between various homogenous data sources and heterogeneous data sources. First, register the target data source at Data Integration. Then you can select the data source directly when configuring a synchronization task on Data Integration.

After confirming that the target data source is supported by Data Integration, you can register the data source at Data Integration. For detailed data source registration, see [configuring data source information](https://www.alibabacloud.com/help/faq-list/72788.htm).

**Note:** 

-   For some data sources Data Integration does not support test connectivity. For more information on data source test connectivity, see [Data Source testing connectivity](reseller.en-US/User Guide/Data Integration/Data source configuration/Data Source testing connectivity.md#).
-   Many times, data sources are created locally and cannot be connected without a public network IP or network. In this case, testing connectivity at the time of configuration of the data source fails directly. Data Integration supports [Add scheduling resources](reseller.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#) to solve this type of network inaccessibility.

## Create a synchronization task and the reader {#section_tfn_1kc_p2b .section}

**Note:** This article mainly introduces you to the synchronization task configuration in wizard mode. Select **wizard mode** when creating new synchronization tasks.

1.  Enter the [DataWorks management console](https://partners-intl.aliyun.com) as a developer, and click **Data Development** in the corresponding project action bar.
2.  Click **Data Development** in the left-hand menu bar to open the Business Process navigator.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337611_en-US.png)

3.  Right-click **Business Flow** in the navigation bar, create **Data Integration node** \> **Data Sync**, and enter the synchronization task's name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337612_en-US.png)

4.  After the synchronization task is created, you can continue to manually configure the reader data source and the target table information for the data synchronization task. When you are selecting a data source to read from, see [configuring reader](https://www.alibabacloud.com/help/faq-list/49806.htm).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337614_en-US.png)


**Note:** Many tasks require incremental synchronization of data when configuring read-side data sources, you can now get the relative date in conjunction with [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) to complete the requirement to get the incremental data.

## Configure the writer {#section_nz2_jlc_p2b .section}

After the reader data source is configured, you can continue to manually configure the writer data source and the target table information for the data synchronization task. When you are selecting the data source to write on, see [configuring writer](reseller.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) writer.md#).

**Note:** For most tasks, you need to select a write mode based on data sources, such as overwrite mode or append mode. For students with write control requirements, refer to the [Configuring writer](reseller.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) writer.md#) documentation to choose the write mode properly.

## Configure mapping {#section_h35_qlc_p2b .section}

When the configuration of both the Read and Write side is complete, you need to specify a mapping relationship between the read and write end columns, and you can choose **Map of the same name** or **Enable Same-line Mapping**.

-   Enable Same-line Mapping: automatically sets the mapping relationship for the same row of data.
-   Automatic Layout: After the mapping relationship is set, the field ordering display is displayed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337615_en-US.png)

**Note:** The field types mapped between columns should be data compatible.

## Channel {#section_s3r_zlc_p2b .section}

When the above steps are configured, the efficiency configuration is required. Efficiency configuration mainly includes DMU settings, synchronous concurrency number settings, synchronous rate settings, synchronous dirty data settings and synchronizing information such as resource group settings.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337616_en-US.png)

Parameters:

-   DMU: the unit of charge for Data Integration.

    **Note:** When setting up a DMU, be aware that the value of the DMU limits the value of the maximum number of concurrency, please configure properly.

-   When you configure Synchronization Concurrency, the data records are split into several tasks based on the specified reader splitting key. These tasks run simultaneously to improve the transmission rate.
-   Synchronous rate: Setting the synchronous rate protects the read-side database from excessive extraction speed, put too much pressure on the source library. It is recommended to throttle the synchronization rate and configure the extraction rate properly based on source database configurations.
-   For example, if the source has varchar type data but is written to a destination column having int type data, a data conversion exception occurs and the data cannot be written to the destination column. The dirty data is mainly set to control the quality of synchronized data. You should set an appropriate number of dirty data based on your business requirements.
-   When you configure a synchronization task, you specify the resource group in which the task runs, default runs on the Default Resource Group. When the project has a tight schedule of resources, you can also expand a scheduled resource by adding a Custom Resource Group, the synchronization task is then specified to run on a Custom Resource Group. See [Add scheduling resources](reseller.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#) for more information. You can make a reasonable configuration based on data source network conditions, project scheduling resource conditions, and business importance.

**Note:** When synchronizing data is not efficient, see [Optimizing configuration](reseller.en-US/User Guide/Data Integration/Task Configuration/Optimizing configuration.md#) to optimize your synchronization tasks.

## Scheduling parameters {#section_dhv_xmc_p2b .section}

You often need to use scheduling parameters to filter your data in synchronization tasks, you will be shown below how to configure scheduling parameters in the synchronization task.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337617_en-US.png)

As shown in the figure above, you can declare a schedule parameter variable in the form of a $ \{variable name, when the variable declaration is complete, write the initialization value of the variable in the scheduled parameter properties, the value initialized here by the variable is identified by $, the content can be either a time expression or a constant.

For example, $ \{today\} was written in code, by assigning today = $ \[yyyymmdd\] in the scheduling parameter, you can get the date of the day, to add-minus to a date, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

## Using custom schedule parameters in synchronization tasks {#section_scv_jnc_p2b .section}

All you need to do in the synchronization task is declare the following parameters in your code.

-   bizdate: Get to the business date, run date-1.
-   cyctime: gets the current run time, in the form of yyyymmddhhmiss.
-   Dataworks provides two system default scheduling parameters, bizdate and cycletime.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16216/15389864337618_en-US.png)

## Configure scheduling Properties {#section_a4t_rnc_p2b .section}

In the scheduling properties, you can set the synchronization task run cycle, run time, task dependency, and so on. Since the synchronization task is the beginning of the ETL job, there are no upstream nodes, at this point, it is recommended to use the project root node as upstream.

After completing the configuration of the synchronization task, save the node and submit.

