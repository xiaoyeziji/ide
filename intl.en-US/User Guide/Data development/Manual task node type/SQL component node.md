# SQL component node {#concept_jpj_rrm_q2b .concept}

## Procedure {#section_ery_5hb_t2b .section}

1.  Create Business Flow

    Click **Manual Business Flow** in the left-side navigation pane, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15367342077961_en-US.png)

2.  Create an SQL component node

    Right-click **Data Development**, and select **Create Data Development Node** \> **SQL Component Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16325/15367342078127_en-US.png)

3.  To improve the development efficiency, data task developers can use components contributed by project members and tenant members to create data processing nodes.

    -   Components created by members of the local project are located under Project Components.
    -   Components created by tenant members are located under Public Components.
    When create a node, set the node type to the **SQL component node** type, and specify the name of the node.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15367342077777_en-US.png)

    Specify parameters for the selected component.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15367342077779_en-US.png)

    Enter the parameter name, and set the parameter type to Table or String.

    Specify three get\_top\_n parameters in sequence.

    Specify the following input table for the parameters of the Table type: test\_project.test\_table.

4.  Node scheduling configuration.

    Click the **Schedule Configuration** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit a node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).

7.  Test in a production environment.

    For more information about the operation, see [Manual tasks](intl.en-US/User Guide/Operation center/Task list/Manual task.md#).


## Upgrade the version of an SQL component node. {#section_tmq_qdq_p2b .section}

After the component developer release a new version, the component users can choose whether to upgrade the use instance of the existing component to the latest version of the used component.

With the component version mechanism, developers can continuously upgrade components and component users can continuously enjoy the improved process execution efficiency and optimized business effects after upgrade of components.

For example, user A uses the v1.0 component developed by user C, and the component owner C upgrades the component to V.2.0. After the upgrade, user A can still use the v1.0 component, but will receive the upgrade reminder. After comparing the new code with the old code, user A finds that the business effects of the new version are better than those of the old version, and therefore can determine whether to upgrade the component to the latest version.

To upgrade an SQL component node developed based on the component template, you only need to select Upgrade, check whether parameter settings of the SQL component node are still effective in the new version, make some adjustments based on the instructions of the new version component, and then submit and release the node like a common SQL component node.

## Interface functions {#section_h3p_32q_p2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16325/15367342078137_en-US.png)

The interface features are described below:

|No.|Feature|Description|
|:--|:------|:----------|
|1|Save|Click it to save settings of the current component.|
|2|Submit|Click it to submit the current component to the development environment.|
|3|Submit and Unlock|Click it to submit the current node and unlock the node to edit the code.|
|4|Steallock Edit|Click it to steallock edit the node if you are not the owner of the current component.|
|5|Run|Click it to run the component locally in the development environment.|
|6|Advanced Run \(with Parameters\)|Click it to run the code of the current node using the parameters configured for the code.**Note:** Advanced Run is unavailable to a Shell node.

|
|7|Stop Run|Click it to stop a running component.|
|8|Re-load|Click it to refresh the interface and restore the last saved status. Unsaved content will be lost.**Note:** If cache is enabled in the configuration center, after the interface is refreshed, you are notified of the code that is cached but not saved. In this case, select the version that you need.

|
|9|Parameter Settings|Click it to view the component information, input parameter settings, and output parameter settings.|
|10|Attributes|Click it set the owner, description, parameters, and resource group of the node.|
|11|Kinship|Click it to view the map of kinship between SQL component nodes and the internal kinship map of each SQL component node.|
|12|Version|Click it to view the submission and release records of the current component.|

