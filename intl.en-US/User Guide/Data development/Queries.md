# Queries {#concept_lfk_lzf_q2b .concept}

Temporary query facilitates you to use the editing code, test whether the actual conditions of the local code meets the expectations, and check the code status. Therefore, temporary query does not support submitting, releasing, and setting the scheduling parameters. To use the scheduling parameters, create a node in Data development or Manual business flow.

## Create a folder {#section_e5f_z5g_q2b .section}

1.  Click the **Queries** in the left-hand navigation bar, select **folder**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16313/15368048347948_en-US.png)

2.  Enter the folder name, select the folder directory, and click **Submit**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16313/15368048347949_en-US.png)

    **Note:** A multi-level folder directory is supported. Therefore, you can store the folder in another folder that has been created.


## Create a node {#section_wz4_kvg_q2b .section}

Temporary query only supports the SHELL and SQL nodes.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16313/15368048347950_en-US.png)

Take the new ODPS SQL node as an example, right-click the folder name and select **Create Node** \> **ODPS SQL**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16313/15368048357951_en-US.png)

|No.|Function|Description|
|:--|:-------|:----------|
|1|Save|Click it to save the entered code.|
|2|Steallock Edit|A user other than the node owner can click it to edit the node.|
|3|Run|Click it to run the code locally \(in the development environment\).|
|4|Advanced Run \(with Parameters\)|Click it to run the code of the current node using the parameters configured for the code.**Note:** Advanced Run is unavailable to a Shell node.

|
|5|Stop Run|Click it to stop the code that is being run.|
|6|Reload|Click it to refresh the page, reload, and restore the last saved status. Unsaved content will be lost.**Note:** If the cache has been enabled in the configuration center, a message is displayed after page refreshing, indicating that the unsaved code has been cashed. Select a required version.

|
|7|Format|Click it to sort the current node code by keyword format. It is often used when a row of code is too long.|

