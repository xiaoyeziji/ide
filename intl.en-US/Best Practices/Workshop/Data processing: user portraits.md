# Data processing: user portraits {#concept_s4b_mps_s2b .concept}

This article shows you how to process log data that has been collected into MaxCompute through dataworks.

**Note:** Before you begin this experiment, please complete the operation in[Data acquisition: log data upload](reseller.en-US/Best Practices/Workshop/Data acquisition: log data upload.md#).

## Create data tables {#section_ptl_cvt_s2b .section}

You can refer to [Data acquisition: log data upload](reseller.en-US/Best Practices/Workshop/Data acquisition: log data upload.md#) to create data tables.

-   Create ods\_log\_info\_d table
    1.  Right click **Table** in the workshop business flow. Click **Create Table** and enter the table's name ods\_log\_info\_d. You can then click DDL Mode to type in the table creation SQL statements.

        The following are table creation statements:

        ```
        CREATE TABLE IF NOT EXISTS ods_log_info_d (
          ip string comment 'IP address',
          uid STRING COMMENT 'User ID',
          time string comment 'time:yyyymmddhh:mi:ss',
          status string comment 'server return status code',
          bytes string comment 'the number of bytes returned to the Client',
          region string comment 'region,get from IP',
          method string comment 'HTTP request type',
          url string comment 'url',
          protocol string comment 'HTTP Protocol version number',
          referer string comment 'source ures',
          device string comment 'terminal type',
          identity string comment 'Access type crawler feed user unknown'
        )
        PARTITIONED BY (
          dt STRING
        );
        ```

    2.  Click **Submit to Development Environment** and **Submit to Production Environment**.
-   Create dw\_user\_info\_all\_d table

    The method of creating a new report table is identical to that of a table statement as follows:

    ```
    -- Create a copy table
    CREATE TABLE IF NOT EXISTS dw_user_info_all_d (
      uid STRING COMMENT 'User ID',
      gender STRING COMMENT 'Gender',
      age_range STRING COMMENT 'Age range',
      zodiac STRING COMMENT 'Zodiac sign',
      region string comment 'region, get from IP',
      device string comment 'terminal type',
      identity string comment 'Access type crawler feed user unknown',
      method string comment 'HTTP request type',
      url string comment 'url',
      referer string comment 'source url',
      time string comment 'time:yyyymmddhh:mi:ss'
    )
    PARTITIONED BY (
      dt string
    );
    ```

-   Create rpt\_user\_info\_d table

    The following are table creation statements:

    ```
    -- Create a copy table
    Create Table if not exists rpt_user_info_d(
      uid STRING COMMENT 'User ID',
      region string comment 'region, get from IP',
      device string comment 'terminal type',
      pv bigint comment 'pv',
      gender STRING COMMENT 'Gender',
      age_range STRING COMMENT 'Age range',
      zodiac STRING COMMENT 'Zodiac sign'
    )
    PARTITIONED BY (
      dt string
    );
    ```


## Business Flow Design {#section_h5n_bxt_s2b .section}

Open the Workshop Business Flow and drag three ODPS SQL nodes amed as "ods\_log\_info\_d、dw\_user\_info\_all\_d、rpt\_user\_info\_d" into the canvas, n, and configure dependencies.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499175_en-US.png)

## Creating user-defined functions { .section}

1.  Download [ip2region.jar](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/85298/cn_zh/1532163718650/ip2region.jar).
2.  Right-click **Resource**, and select **Create Resource** \> **jar**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499176_en-US.png)

3.  Click **Select File**, select ip2region. jar that has been downloaded locally, and click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499177_en-US.png)

4.  After the resource has been uploaded to dataworks, click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499178_en-US.png)

5.  Right-click a **function** and select **Create Function**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499179_en-US.png)

6.  Enter the function name getregion, select the Business Flow to which you want to belong, and click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499180_en-US.png)

7.  Enter the function configuration in the Registry Function dialog box, specify the class name, description, command format, and parameter description.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499181_en-US.png)

    Parameters:

    -   Function Name: getregion
    -   Class Name: org.alidata.odps.udf.Ip2Region
    -   Resource list: ip2region. Jar
    -   Description: IP address translation area
    -   Command Format: getregion \('IP '\)
    -   Parameter description: IP Address
8.  Click **Save** and **submit**.

## Configure ODPS SQL nodes { .section}

-   Configure ods\_log\_info\_d Node
    1.  Double-click the ods\_log\_info\_d node to go to the node configuration page and write the processing logic.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376499182_en-US.png)

        The SQL logic is as follows:

        ```
        INSERT OVERWRITE TABLE ods_log_info_d PARTITION (dt=${bdp.system.bizdate})
        SELECT ip
          , uid
          , time
          , status
          , bytes -- use a custom UDF to get a locale over IP
          , getregion (ip) as region -- the request difference is divided into three fields through the regular
          , regexp_substr(request, '(^[^ ]+ )') AS method
          , regexp_extract(request, '^[^ ]+ (.*) [^ ]+$') AS url
          , regexp_substr(request, '([^ ]+$)') AS protocol -- get more precise urls with regular clear refer
          , regexp_extract(referer, '^[^/]+://([^/]+){1}') AS referer -- Get terminal information and access form through agent
          , CASE
        WHEN TOLOWER(agent) RLIKE 'android' THEN 'android'
            WHEN TOLOWER(agent) RLIKE 'iphone' THEN 'iphone'
            WHEN TOLOWER(agent) RLIKE 'ipad' THEN 'ipad'
            WHEN TOLOWER(agent) RLIKE 'macintosh' THEN 'macintosh'
            WHEN TOLOWER(agent) RLIKE 'windows phone' THEN 'windows_phone'
            WHEN TOLOWER(agent) RLIKE 'windows' THEN 'windows_pc'
            ELSE 'unknown'
          END AS device
          , CASE
            WHEN TOLOWER(agent) RLIKE '(bot|spider|crawler|slurp)' THEN 'crawler'
            WHEN TOLOWER(agent) RLIKE 'feed'
            OR regexp_extract(request, '^[^ ]+ (.*) [^ ]+$') RLIKE 'feed' THEN 'feed'
            WHEN TOLOWER(agent) NOT RLIKE '(bot|spider|crawler|feed|slurp)'
            AND agent RLIKE '^[Mozilla|Opera]'
            AND regexp_extract(request, '^[^ ]+ (.*) [^ ]+$') NOT RLIKE 'feed' THEN 'user'
            ELSE 'unknown'
          END AS identity
          FROM (
            SELECT SPLIT(col, '##@@')[0] AS ip
            , SPLIT(col, '##@@')[1] AS uid
            , SPLIT(col, '##@@')[2] AS time
            , SPLIT(col, '##@@')[3] AS request
            , SPLIT(col, '##@@')[4] AS status
            , SPLIT(col, '##@@')[5] AS bytes
            , SPLIT(col, '##@@')[6] AS referer
            , SPLIT(col, '##@@')[7] AS agent
          FROM ods_raw_log_d
          WHERE dt = ${bdp.system.bizdate}
        ) a;
        ```

    2.  Click **Save**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509183_en-US.png)

-   Configure dw\_user\_info\_all\_d Node
    1.  Double-click the dw\_user\_info\_all\_d node to go to the node configuration page and write the processing logic.

        The SQL logic is as follows:

        ```
        INSERT OVERWRITE TABLE dw_user_info_all_d PARTITION (dt='${bdp.system.bizdate}')
        SELECT COALESCE(a.uid, b.uid) AS uid
          , b.gender
          , b.age_range
          , b.zodiac
          , a.region
          , a.device
          , a.identity
          , a.method
          , a.url
          , a.referer
          , a.time
        FROM (
          SELECT *
          FROM ods_log_info_d
          WHERE dt = ${bdp.system.bizdate}
        ) a
        LEFT OUTER JOIN (
          SELECT *
          FROM ods_user_info_d
          WHERE dt = ${bdp.system.bizdate}
        ) b
        ON a.uid = b.uid;
        ```

    2.  Click **Save**.
-   Configure a rpt\_user\_info\_d Node
    1.  Double-click the fig node to go to the node configuration page and write the processing logic.

        The SQL logic is as follows:

        ```
        INSERT OVERWRITE TABLE rpt_user_info_d PARTITION (dt='${bdp.system.bizdate}')
        SELECT uid
          , MAX(region)
          , MAX(device)
          , COUNT(0) AS pv
          , MAX(gender)
          , MAX(age_range)
          , MAX(zodiac)
        FROM dw_user_info_all_d
        WHERE dt = ${bdp.system.bizdate}
        GROUP BY uid;
        ```

    2.  Click **Save**.

## Submitting Business Flows { .section}

1.  Click **Submit** to submit the node tasks that have been configured in the Business Flow.
2.  Select the nodes that need to be submitted in the Submitdialog box, and check the **Ignore Warnings on I/O Inconsistency**, click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509186_en-US.png)


## Running Business Flows {#section_inw_mpt_s2b .section}

1.  Click **Run** to verify the code logic.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509187_en-US.png)

2.  Click **Queries** in the left-hand navigation bar.
3.  Select **New** \> **ODPS SQL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16421/15512376509169_en-US.png)

4.  Write and execute SQL statements, Query Task for results, and confirm data output.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509188_en-US.png)

    The query statement is as follows:

    ```
    --- View the data in the data box
    select * From glaswhere dt ''business day'' limit 10;
    ```


## Publishing Business Flow {#section_ptt_vb5_s2b .section}

After the Business Flow is submitted, it indicates that the task has entered the development environment, but the task of developing an environment does not automatically schedule, so the tasks completed by the configuration need to be published to the production environment \(before publishing to the production environment, test this task code \).

1.  Click **Publish** To Go To The publish page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509189_en-US.png)

2.  Select the task to publish and click **Add To Be-Published List**.
3.  Enter the list of pending releases, and click **Pack and publish all**.
4.  View published content on the Publish Package List page.

## Run tasks in production { .section}

1.  After the task has been published successfully, click **Operation center**.
2.  Select Workshop Business Flows in the **Task List**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509193_en-US.png)

3.  Right-click the workshop\_start node in the DAG graph and select **Patch Data** \> **Current and downstream nodes**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509194_en-US.png)

4.  Check the task that needs to fill the data, enter the business date, and click **OK**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509195_en-US.png)

    When you click **OK**, you automatically jump to the patch data task instance page.

5.  Click **Refresh** until the SQL task runs successfully.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16422/15512376509196_en-US.png)


## Next step {#section_fwr_pd5_s2b .section}

Now that you 've learned how to create SQL tasks, how to handle raw log data, you can continue with the next tutorial. In this tutorial, you will learn how to set up data quality monitoring for tasks completed by your development, ensures the quality of tasks running. For more information, see[Data quality monitoring](reseller.en-US/Best Practices/Workshop/Data quality monitoring.md#) .

