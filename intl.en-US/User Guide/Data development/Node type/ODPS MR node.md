# ODPS MR node {#concept_nxk_gz3_p2b .concept}

This topic describes the ODPS MR node functions. The MaxCompute supports MapReduce programming APIs. You can use the Java API provided by MapReduce to write MapReduce programs for processing data in MaxCompute. You can create ODPS MR nodes and use them in Task Scheduling.

For more information about how to edit and use the ODPS MR, see the examples in the MaxCompute documentation [WordCount examples](https://www.alibabacloud.com/help/doc-detail/27886.htm).

To use an ODPS MR node, you must upload and release the resource for usage, and then create the ODPS MR node.

## Create a resource instance {#section_cgz_hbj_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16288/15525308337643_en-US.png)

2.  Right-click **Resource**, and select **Create Resource** \> **JAR**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525308337720_en-US.png)

3.  Enter the resource name in Create Resource according to the naming convention, and set the resource type to JAR, and then select a local JAR package.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525308337721_en-US.png)

    **Note:** 

    -   If this JAR package has been uploaded to the ODPS client, you must deselect **Upload to ODPS**. Otherwise, an error will be reported during the upload process.
    -   The resource name is not always the same as the uploaded file name.
    -   The resource name can be 1 to 128 characters in length, and include letters, numbers, underscores \(\_\), and periods \(.\). It is case insensitive. The resource file extension is .jar if the resource is a JAR resource, and .py for a python resource.
4.  Click **Submit** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525308337722_en-US.png)

5.  Publish a node task.

    For more information about the operation, see Release management.


## Create an ODPS MR node {#section_gzm_hbj_p2b .section}

1.  Right-click the **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15525308347651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **ODPS MR**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525308347723_en-US.png)

3.  Edit the node code. Double click the new ODPS MR node and enter the following interface:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525308347724_en-US.png)

    The node code editing example as follows:

    ```
    jar -resources base_test.jar -classpath ./base_test.jar com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll
    ```

    The code description as follows:

    -   The code `-resources base_test.jar` indicates the file name of the referenced JAR resource.
    -   The code `-classpath`is the JAR package path.
    -   The code `com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll` indicates the main class in the JAR package is called during execution. It must be consistent with the main class name in the JAR package.
    When one MR calls multiple JAR resources, the classpath must be written as follows: `-classpath ./xxxx1.jar,./xxxx2.jar`, that is, two paths must be separated by a comma \(,\).

4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the Node Scheduling  Configuration page. For more information about node scheduling configuration, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

5.  Submit the node.

    After completing the configuration, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node in the development environment.

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/O&M Center/Task list/Cyclic task.md#).


