# Function {#concept_brf_tbl_q2b .concept}

## Register the UDF {#section_cx2_wbl_q2b .section}

MaxCompute supports UDFs. For more information, see [UDF overview](https://www.alibabacloud.com/help/doc-detail/27866.htm). 

DataWorks provides the visual GUI to register functions for replacing the MaxCompute command line add function.

Currently, the Python and Java APIs supports the implementation of UDF. To compile a UDF program, you can upload the UDF code by [Adding resources](reseller.en-US/User Guide/Data development/Business flow/Resource.md#) and then register the UDF.

## UDF registration procedure {#section_vrv_bcl_q2b .section}

1.  Click **Manual Business Flow** in the left-side navigation pane, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334687961_en-US.png)

2.  In the offline Java environment, you can edit the program, and compress the program into a JAR package. Then create a JAR resource, submit and publish the program.

    You can also create a Python resource. You can compile and save the Python code, and submit the code, and then publish the code. For more information, see [Create Resources](reseller.en-US/User Guide/Data development/Business flow/Resource.md#).

3.  Select **Function** \> **Create Function**, and enter the new function configuration, and then click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16317/15525334687995_en-US.png)

4.  Edit the function configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16317/15525334687996_en-US.png)

    -   Class Name: The name of the main class that implements the UDF. When the resource is Python, the typical writing style is: Python resource name.Class name \('.py' is not required in the resource name\).
    -   Resources: The name of the resource in the second step. If there are multiple resources, separate them with commas \(,\).
    -   Description: The UDF description. It is optional.
5.  Submit the job.

    After the configuration is completed, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task

    For more information about publishing a node task, see [Publish a task](reseller.en-US/User Guide/Data development/Publish management/Publish a task.md#).


