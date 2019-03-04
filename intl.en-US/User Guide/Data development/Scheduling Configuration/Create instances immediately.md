# Create instances immediately {#concept_w3k_fkw_tfb .concept}

You can use this method to create instances from a node immediately after the node has been published. You can then view the instance dependency relationships in the operations center.

## Instance creation methods {#section_qfy_3kw_tfb .section}

You can choose the following methods to create instances from a published node.

-   **Next Day**

    If you choose this method, nodes published before 22:00 create instances the following day. Nodes published after 22:00 create instances the day after the following day.

-   **Immediately After Publishing**

    If you choose this method, nodes create instances immediately after they are published.


## Create a node to create instances immediately after the node is published {#section_z3h_skw_tfb .section}

1.  On the DataStudio page, create a **Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831405_en-US.png)

2.  Create a node in the created business flow. This example uses an ODPS SQL node.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831406_en-US.png)

3.  Double-click the node, edit the code, click **Schedule** on the right side of the page, and set the instance creation method to **Immediately After Publishing**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831416_en-US.png)

    **Note:** 

    -   You can publish the node at any time. However, during the time period from 22:00 to 24:00, the node will not create instances even if it is published.
    -   After an Immediately After Publishing node is published, it must wait 10 minutes to create instances.
    -   If you change the instance creation method from **Next Day** to **Immediately After Publishing** for a node and republish the node, only the instances that have already been run are retained. After the node is republished, it will wait 10 minutes and delete those instances that still have not been run, and then create now instances.
    -   An Immediately After Publishing node determines whether to create new instances based on the **CRON Expression**. If the expression is changed, the node then creates new instances. Therefore, if you need a republished Immediately After Publishing node to create new instances, you must change the **CRON Expression** of the node.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831427_en-US.png)


## Scenarios {#section_a41_fww_tfb .section}

The Immediately After Publishing method is typically used in the following scenario: The predecessor node uses the Next Day method to create instances. The successor nodes all use the Immediately After Publishing method to create instances. The following figure shows the dependency relationships between these nodes:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831417_en-US.png)

This scenario includes the following situations:

1.  **If the predecessor and successor nodes are new nodes added on the current day, which means the predecessor node must wait until the following day to create the first instance:**

    -   **Daily run successor nodes:** The instance created by the daily run successor node on the current day does not have a predecessor instance. If the dependency type is set to custom, the instance created by this successor node can depend on an instance created by another node.
    -   **Hourly or minute by minute run successor nodes:** If the dependency type is set to successor-dependent and the predecessor node is not an hourly or minute by minute run node, only the first created instance does not have a predecessor node.
    -   **Monthly or weekly run successor nodes:** If the dependency type is set to self-dependent, only the first instance created by this node does not have a predecessor instance.

        **Note:** A daily run predecessor node must wait until the following day to create the first instance. Therefore, instances created by the successor nodes on the current day do not have a predecessor instance. These instances become independent and cannot be run. If the dependency type of the successor nodes is set to self-dependent, the instances created on the following day will depend on those independent instances. As a result, the task is split and cannot be successfully run.

    -   **Conclusion:** When the predecessor and successor nodes are new nodes added on the current day and the dependency type is set to self-dependent, the first created instance will not have a predecessor instance. As a result, the task cannot be run successfully.
2.  **If the predecessor node has already created a predecessor instance, and the successor nodes are newly added Immediately After Publishing nodes:**
    -   **Daily run successor nodes:** The instance created by this node on the current day will depend on the existing predecessor instance. If the dependency type is set to self-dependent, all instances created by this node have a predecessor instance.
    -   **Hourly or minute by minute run successor nodes:** The instance created by this node on the current day will depend on the existing predecessor instance. If the dependency type is set to self-dependent, only the first instance does not have a predecessor instance.
    -   **Monthly or weekly run successor nodes:** No matter which dependency type you set, the instance created by this node will depend on the existing predecessor instance.
    -   **Conclusion:** To successfully run a node with the dependency type set to self-dependent, make sure that the node can be successfully run on the previous day.
3.  **If the daily run predecessor node has already created an instance, and an hourly run successor node is changed to a daily run node that uses the Immediately After Publishing method:**
    -   **Nodes before the change:** Both the predecessor and successor nodes are hourly run nodes that use the Next Day method.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831422_en-US.png)

    -   **Update:** The hourly run node that depends on the predecessor node is changed to a daily run node that uses the Immediately After Publishing method.
    -   **Instance creation and dependency relationships after the change:** The dotted line in the preceding figure indicates the time when a node is submitted and republished. The node will delete all instances that are created 10 minutes after the node is republished, and create a new daily run instance. The hourly run successor nodes of the republished node will depend on the newly created daily run instance. If the dependency type of the republished node is set to self-dependent, the newly created instance will depend on the instance created by the Next Day node.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62003/155168959831425_en-US.png)

    -   **Instances after the change:** Before the node is republished, it creates hourly run instances. After the node is republished, it creates daily run instances.
    -   **Conclusion:** The dependency relationships of the republished node remain unchanged. Only the instance created on the current day is affected.

