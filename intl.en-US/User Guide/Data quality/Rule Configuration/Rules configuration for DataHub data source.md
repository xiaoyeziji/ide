# Rules configuration for DataHub data source {#concept_zty_sxh_r2b .concept}

This article describes how to configure the DataHub data source.

Go to Operation center, you can create new data sources.  You can configure the Endpoint of DataHub, data source name, AccessKeyID, and AccessKeySecret to create a connection string.  After this, you can query on DQC. 

## Choose data source {#section_lgc_zxh_r2b .section}

1.  Click **Rules configuration** at the left navigation;
2.  Select **DataHub data source**, you can see all topics in this data source.

    Select DataHub data source, you can see all topics in this data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/15368265398777_en-US.png)


## Monitor rules configuration {#section_qp2_myh_r2b .section}

1.  Select a specific topic, click **Configure monitoring rules** to enter monitor rules page.

    You can also navigate to **My Subscription** \> **DataHub data source** \> **Topic name** to enter the subscribed topic quickly.

2.  Click **create rule**, and the datahub data source can now only create a template rule with a data type of cut-off monitoring.

    Configurations:

    -   **Alarm frequency**: You can set how often to alarm, there are 10 minutes, 30 minutes, 1 hour, 2 hours four options.
    -   **Orange threshold**: in minutes, you can only enter an integer, and you must be less than the red threshold.
    -   **Red threshold**: in minutes, you can only enter an integer, which must be greater than the orange threshold.
3.  When the settings are complete, click **Save** to add the rules that you created to the topic.

