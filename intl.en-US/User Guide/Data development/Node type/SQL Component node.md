# SQL Component node {#concept_s2b_53p_p2b .concept}

## Procedure {#section_qhm_djp_p2b .section}

1.  Right-click the **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15525323457651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **SQL Component Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15525323457778_en-US.png)

3.  To improve the development efficiency, the data task developers can use components contributed by project and tenant members to create data processing nodes.

    -   Components created by members of the local project are under Project Components.
    -   Components created by tenant members are located under Public Components.
    When you create a node, set the node type to **SQL Component node**, and specify the node name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15525323457777_en-US.png)

    Specify parameters for the selected component.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15525323457779_en-US.png)

    Enter the parameter name, and set the parameter type to Table or String.

    Specify the three get\_top\_n parameters in sequence.

    Specify the following input table for the Table type: test\_project.test\_table parameters.

4.  Node scheduling configuration.

    Click the **Scheduling Configuration** on the right of the node task editing area to go to the Node Scheduling Configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

5.  Submit a node.

    After completing the configuration, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node in the development environment.

6.  Publish a node task.

    For more information about the operation, see Publish management.

7.  Test in a production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/O&M Center/Task list/Cyclic task.md#).


## Upgrade the SQL component node version {#section_tmq_qdq_p2b .section}

After the component developer releases a new version, the component users can choose whether to upgrade the used instance of the existing component to the latest used component version.

With the component version mechanism, developers can continuously upgrade components and component users can continuously enjoy the improved process execution efficiency and optimized business effects after upgrading the components.

For example, user A uses the v1.0 component developed by user B, and user B upgrades the component to V.2.0. User A can still use the v1.0 component after the upgrade, but will receive an upgrade reminder. After comparing the new code with the old code, user A finds that the business effects of the new version are better than that of the old version, and therefore can determine whether to upgrade to the latest version of the component.

You can easily upgrade an SQL component node based on the component template, by selecting Upgrade. After checking whether the SQL component node parameter settings are effective in the new version, and then make some adjustments based on the new version component instructions, and then submit and release the node similar to a common SQL component node.

## Interface functions {#section_h3p_32q_p2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16297/15525323457780_en-US.png)

The interface features are described below:

|No.|Feature|Description|
|:--|:------|:----------|
|1|Save|Saves the current component settings.|
|2|Steal lock edit|Steals lock edit of the node if you are not the owner of the current component.|
|3|Submit|Submit the current component in the development environment.|
|4|Publish component|Publish a universal global component to the entire tenant, so that all users in the tenant can view and use the public component.|
|5|Resolve input and output parameters|Resolve the input and output parameters of the current code.|
|6|Precompilation|Edit the custom and component parameters of the current component.|
|7|Run|Run the component locally in the development environment.|
|8|Stop run|Stop a running component.|
|9|Format|Sort the current component code by keyword.|
|10|Parameter settings|View the component information, input parameter settings, and output parameter settings.|
|11|Version|View the submission and release records of the current component.|
|12|Reference records|View the usage record of the component.|

