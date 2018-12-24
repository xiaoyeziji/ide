# SQL Component node {#concept_s2b_53p_p2b .concept}

## Procedure {#section_qhm_djp_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15389902597651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **SQL Component Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15389902607778_en-US.png)

3.  To improve the development efficiency, data task developers can use components contributed by project members and tenant members to create data processing nodes.

    -   Components created by members of the local project are located under Project Components.
    -   Components created by tenant members are located under Public Components.
    When create a node, set the node type to the **SQL Component node** type, and specify the name of the node.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15389902607777_en-US.png)

    Specify parameters for the selected component.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15389902607779_en-US.png)

    Enter the parameter name, and set the parameter type to Table or String.

    Specify three get\_top\_n parameters in sequence.

    Specify the following input table for the parameters of the Table type: test\_project.test\_table.

4.  Node scheduling configuration.

    Click the **Scheduling Configuration** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit a node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in a production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


## Upgrade the version of an SQL Component node. {#section_tmq_qdq_p2b .section}

After the component developer release a new version, the component users can choose whether to upgrade the use instance of the existing component to the latest version of the used component.

With the component version mechanism, developers can continuously upgrade components and component users can continuously enjoy the improved process execution efficiency and optimized business effects after upgrade of components.

For example, user A uses the v1.0 component developed by user C, and the component owner C upgrades the component to V.2.0. After the upgrade, user A can still use the v1.0 component, but will receive the upgrade reminder. After comparing the new code with the old code, user A finds that the business effects of the new version are better than those of the old version, and therefore can determine whether to upgrade the component to the latest version.

To upgrade an SQL Component node developed based on the component template, you only need to select Upgrade, check whether parameter settings of the SQL Component node are still effective in the new version, make some adjustments based on the instructions of the new version component, and then submit and release the node like a common SQL Component node.

## Interface functions {#section_h3p_32q_p2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15389902607780_en-US.png)

The interface features are described below:

|No.|Feature|Description|
|:--|:------|:----------|
|1|Save|Click it to save settings of the current component.|
|2|Steal lock Edit|Click it to steal lock edit the node if you are not the owner of the current component.|
|3|Submit|Click it to submit the current component to the development environment.|
|4|Publish Component|Click it to publish a universal global component to the entire tenant, so that all users in the tenant can view and use the public component.|
|5|Resolve Input and Output Parameters|Click it to resolve the input and output parameters of the current code.|
|6|Precompilation|Click it to edit custom and component parameters of the current component.|
|7|Run|Click it to run the component locally in the development environment.|
|8|Stop Run|Click it to stop a running component.|
|9|Format|Click it to sort the current component code by keyword.|
|10|Parameter Settings|Click it to view the component information, input parameter settings, and output parameter settings.|
|11|Version|Click it to view the submission and release records of the current component.|
|12|Reference Records|Click it to view the use record of the component.|

