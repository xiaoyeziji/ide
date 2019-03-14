# Create instances immediately {#concept_w3k_fkw_tfb .concept}

This topic describes how to immediately create instances from a published node. You can view the instance dependency relationships in the O&M center.

## Instance creation methods {#section_qfy_3kw_tfb .section}

You can choose the following methods to create instances from a published node.

-   **Next day**

    If you choose this method, the nodes published before 23:30 create instances the following day. The nodes published after 23:30-00:00 create instances three days later.

-   **Immediately after publishing**

    If you choose this method, the nodes create instances immediately after they are published.


## Creating an instance for creating nodes after the node is published {#section_z3h_skw_tfb .section}

1.  On the DataStudio page, create a **Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294031405_en-US.png)

2.  Create a node in the created business flow. The following example uses an ODPS SQL node.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294031406_en-US.png)

3.  Double-click the node, edit the code, and click **Schedule** on the right-navigation pane of the page. Then set the instance creation method to **Immediately After Publishing**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294131416_en-US.png)

    **Note:** 

    -   You can publish the node any time. However, both unpublished and published nodes will not create instances during the time period 22:00 to 24:00.
    -   After an Immediately After Publishing Node is published, you must wait 10 minutes to create instances.
    -   If you change the instance creation method from **Next Day** to **Immediately After Publishing** to republish the node. Only the instances that have been run are retained. After the node is republished, it will pend 10 minutes before deleting instances that have not been run, and then create now instances.
    -   An Immediately After Publishing node determines whether to create new instances based on the **CRON Expression**. If the expression changes, the node then creates new instances. Therefore, if you need to republish Immediately After Publishing node to create a new instance, you must change the **CRON Expression** of the node.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294131427_en-US.png)


## Scenarios {#section_a41_fww_tfb .section}

The Immediately After Publishing method typically uses the following scenario: The predecessor node uses the Next Day method to create instances. The successor nodes all use the Immediately After Publishing method to create instances. The following figure shows the dependency relationships between these nodes:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294131417_en-US.png)

This scenario includes the following situations:

1.  **If the upstream and downstream nodes are new nodes added to today, this means the upstream node must wait until the following day to create the first instance:**

    -   **Daily run upstream nodes:** The instance created by the daily run upstream node today does not have an upstream node. If the dependency type is set to custom, the instance created by the upstream node will depend on an instance created by another node.
    -   **Once a minute or hourly run upstream nodes:** If the dependency type is set to upstream-downstream dependency and the upstream node is not once a minute or hourly run node, only the first created instance will not have an upstream node.
    -   **Weekly or monthly run upstream nodes:** If the node dependency type is self-dependent, only the first instance created by this node does not have an upstream node.

        **Note:** A daily run upstream node will not create the first instance until the following day. Therefore, instances created by the downstream nodes today will become independent instances without upstream nodes and cannot be run. If the dependency type of the downstream nodes is set to self-dependent, the instances created the next day will depend on those independent instances. As a result, the task is isolated and cannot be run.

    -   **Conclusion:** When the upstream and downstream nodes are new nodes added to the current day and the dependency type is set to self-dependent, the instance created first will not have an upstream instance. As a result, the task cannot run successfully.
2.  **If the upstream node has created a predecessor instance, and the successor nodes are added Immediately After Publishing nodes:**
    -   **Daily run downstream nodes:** The instance created by this node today will depend on the existing upstream instance. If the dependency type is set to self-dependent, all instances created by this node will have an upstream instance.
    -   **Once a minute or hourly run downstream nodes:**The instance created by this node on the current day will depend on the existing upstream instance. If the dependency type is set to self-dependent, only the first instance does not have an upstream instance.
    -   **Weekly or monthly run downstream nodes:** Despite of the set dependency type, the instance created by this node will depend on the existing upstream instance.
    -   **Conclusion:**To successfully run a self-dependent node , make sure the node can successfully run on the day before.
3.  **If the daily run upstream node has a created instance, and an hourly run upstream node is changed to a daily run node that uses the Immediately After Publishing method:**
    -   **Nodes before modification:** Both the upstream and downstream nodes are hourly run nodes that use the Next Day method.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294131422_en-US.png)

    -   **Update:** The hourly run node that depends on the upstream node is changed to a daily run node that uses the Immediately After Publishing method.
    -   **Instance creation and dependencies after modification:** The dotted line in the preceding figure indicates the time when the node is submitted and republished. The node will delete all instances that are created 10 minutes after the node is republished, and create a new daily run instance. The hourly run successor nodes of the republished node will depend on the newly created daily run instance. If the republished node dependency type is set to self-dependent, the newly created instance will depend on the instance created by the Next Day node.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155253294131425_en-US.png)

    -   **Instances after modification:** Before the node is published, it creates hourly run instances. After the node is republished, it creates daily run instances.
    -   **Conclusion:** The dependencies of the republished node remain unchanged. Only the instance created on the current day is affected.

