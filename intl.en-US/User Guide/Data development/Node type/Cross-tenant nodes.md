# Cross-tenant nodes {#concept_ygj_hkc_kfb .concept}

Cross-tenant nodes are typically used to associate nodes from different tenants. Cross-tenant nodes are divided into sender nodes and recipient nodes.

## Prerequisites {#section_cmk_zkc_kfb .section}

A sender node and its recipient node must use the same CRON expression. You can choose **Schedule** \> **Scheduling Mode** to view the CRON expression, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22651/155080614013390_en-US.png)

## Create a node {#section_fvp_klc_kfb .section}

1.  On the **Data Studio** page, right-click **Control**, and choose **Create Control Node** \> **Cross-Tenant Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22651/155080614013391_en-US.png)

    Enter a name in the dialog box and click **Submit**.

2.  Complete the configuration for the node. Set the node type to **Send**or **Receive**. Authorize a target workspace and a target Alibaba Cloud account. This example sets the node type to Send. Therefore, you need to enter the workspace and account that are authorized by the recipient node. Save and submit the node after you have completed the node configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22651/155080614013392_en-US.png)

    Follow the same procedure to create a control node under the recipient's account and workspace. Set the type of this node to Receive. Afterward, information about available sender nodes will appear. You must also set the timeout timer. The timeout timer restarts when the recipient node starts running.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22651/155080614013393_en-US.png)

    The sender node sends a message to the message center, and starts running after the message is successfully delivered. The recipient node continuously pulls messages from the message center. The recipient node starts running when it successfully pulls a message within the timeout period.

    If the recipient node does not receive a message within the timeout period, the recipient node is not created. The timeout of a message can be set up to 24 hours.

    Example:

    On October 8, 2018, a periodically created instance was successfully run and a message was sent to the message center. If you create a retroactive instance for the recipient node with the business date set to October 7, 2018, the recipient node is displayed.


