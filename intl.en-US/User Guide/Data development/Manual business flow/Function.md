# Function {#concept_brf_tbl_q2b .concept}

## Register the UDF {#section_cx2_wbl_q2b .section}

MaxCompute supports the UDFs. For more information, see [UDF overview](https://www.alibabacloud.com/help/doc-detail/27866.htm). 

DataWorks provides the visual GUI to register functions for replacing the ODPS command line add function.

Currently, the Python and Java APIs support implementation of UDFs. To compile a UDF program, you can upload the UDF code by [Adding resources](intl.en-US/User Guide/Data development/Resource.md#) and then register the UDF.

## UDF registration procedure {#section_vrv_bcl_q2b .section}

1.  Click **Manual Business Flow** in the left-side navigation pane, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15367338997961_en-US.png)

2.  In the offline Java environment, edit the program, compress the program into a jar package, create a jar resource, and submit and release the program.

    Alternatively, create a Python resource, compile and save the Python code, and then submit and release the code. For more information,see [Create Resources](intl.en-US/User Guide/Data development/Resource.md#).

3.  Select **Function** \> **Create Function**, enter the configuration of the new function, click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16317/15367338997995_en-US.png)

4.  Edit the function configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16317/15367338997996_en-US.png)

    -   Class Name: name of the main class that implements the UDF. When the resource is Python, the typical style of writing is: Python resource name.Class name \('.py' is not needed in the resource name\).
    -   Resources: Name of the resource in the second step, if there are multiple resources, separate them using commas.
    -   Description: UDF description. It is optional.
5.  Submit the job.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Release a node task

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).


