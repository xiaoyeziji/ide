# Branch node {#concept_er5_nzn_fgb .concept}

Branch node is one of the logical control family nodes provided in DataStudio. The branch node can define the **branch logic** and the direction of downstream branches under **different logical conditions**.

## Create a branch node {#section_nm5_bzr_ggb .section}

Branch node is located in the **Control** class directory of new node menu, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335372_en-US.png)

## Define the branch logic {#section_nzg_t1s_ggb .section}

1.  After creating the branch node, jump to the Branch Logic definition page, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335378_en-US.png)

2.  In the **Branch Logic definition** page, you can use **Add Branch** button to define the **Branch Conditions**, **Associated to node output**, and the **Branch describe**, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335380_en-US.png)

    **The parameters are as follows:**

    -   **Branch Conditions**
        -   The branch condition only supports defining logical judgment condition according to the Python comparison operators.
        -   If the value of the running state expression is true, it means that the corresponding branching condition is satisfied, otherwise it is unsatisfactory.
        -   If the parsing error of the running state expression is reported, the running state of the whole branch node will be set to failure.
        -   The branching conditions support using global variables and parameters defined in node context, such as $\{Input\} in the figure, which can be a node input parameter defined in the branching node.
    -   **Associated to node output**
        -   Node output is used to mount dependencies for downstream node of branch node.
        -   When the branch condition is satisfied, the downstream node mounted on the corresponding associated with the node output is selected to run \(also refer to the status of other upstream nodes that the node depends on\).
        -   When the branch condition is not satisfied, the downstream node mounted on the corresponding associated with the node output is not selected to execute, the downstream node is placed in a state that is not running because the branch condition is not satisfied.
    -   **Branch describe**: refer to the description of the branch definition.

        Defining two branches: $\{Input\}==1 and $\{Input\}\>2, as shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335388_en-US.png)

        -   Edit: Click **Edit** button, you can modify the setting branches and the relevant dependencies will also change.
        -   Delete: Click **Delete** button, you can delete the setting branches and the related dependencies will also change.

## Scheduling configuration {#section_x1g_sds_ggb .section}

After defining the branch condition, the output name is automatically added to the node **Output** of the **Schedule**, and the downstream node can rely on the output name to mount. As shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335389_en-US.png)

**Note:** If there is no output record in the scheduling configuration for context dependencies established by wiring, enter it manually.

## Output case - downstream node mounted to branch node {#section_ujy_w2s_ggb .section}

In the downstream node, after adding the branch node as the upstream node, you can define the branch direction under different conditions by selecting the corresponding branch node output. For example, in the business process shown in the figure below,**Branch\_1** and **Branch\_2** are both downstream nodes of the branch node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702335394_en-US.png)

Branch\_1 depends on the output of 'autotest.fenzhi121902\_1', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702435396_en-US.png)

Branch\_2 depends on the output of 'autotest.fenzhi121902\_2', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82217/154804702435399_en-US.png)

## Submit scheduling operation {#section_n1j_chs_ggb .section}

Submit the dispatch to the operation center to run, and the branch node satisfies the condition \(that is depending on 'autotest.fenzhi121902\_1' \).Therefore, the print result of its log is as follows.

-   When the branch condition is satisfied and select the downstream node of the branch to run. You can see the details of the run in **Running Log**.
-   When the branch condition is not satisfied and do not select the downstream node of the branch to run. You can see that the node is set to 'skip' in **Running Log**.

## Addition: supported Python comparison operators {#section_bcw_h3s_ggb .section}

In the table below, we assume that variable a is 10 and variable b is 20.

|Comparison operators|Description|Example|
|--------------------|-----------|-------|
|==|Equal - compare objects for equality.|（a==b）return 'false'|
|!=|Not equal - compare whether two objects are not equal.|（a!=b）return 'true'|
|<\>|Not equal - compare whether two objects are not equal.|（a<\>b）return 'true'. This operator is similar to '!='.|
|\>|Greater than - return whether x is greater than y.|（a\>b）return 'false'|
|<|Less than - return whether x is less than y. All comparison operators return 1 for true and 0 for false. This is equivalent to the special variables True and False, respectively.|（a<b）return 'true'|
|\>=|Greater than or equal to - return whether x is greater than or equal to y.|（a\>=b）return 'false'|
|<=|Less than or equal to - return whether x is less than or equal to y.|（a<=b）return 'true'|

