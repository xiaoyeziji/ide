# ODPS MR node {#concept_nxk_gz3_p2b .concept}

MaxCompute supports MapReduce programming APIs. You can use the Java API provided by MapReduce to write MapReduce programs for processing data in MaxCompute. You can create ODPS MR nodes and use them in Task Scheduling.

For how to edit and use the ODPS MR, see the examples in the MaxCompute documentation [WordCount examples](https://www.alibabacloud.com/help/doc-detail/27886.htm).

To use an ODPSMR node, you must first upload and release the resource to be used, and then create the ODPS MR node.

## Create a resource instance {#section_cgz_hbj_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16288/15404466127643_en-US.png)

2.  Right-click **Resource**, and select **Create Resource** \> **jar**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466127720_en-US.png)

3.  Enter the resource name in the  New Resource according to the naming convention, set the resource type to jar, select a local jar package to the uploaded.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466127721_en-US.png)

    **Note:** 

    -   If this jar package has been uploaded on the ODPS client, you must deselect **Uploaded as the ODPS resource. In this upload, the resource will also be uploaded to ODPS**. Otherwise, an error will be reported during the upload process.
    -   The resource name is not necessarily the same as the name of the uploaded file.
    -   Naming convention for a resource name: a string of 1 to 128 characters, including letters, numbers, underlines, and dots. The name is case insensitive. If the resource is a jar resource, the extension is .jar. If the resource is a Python resource, the extension is .py.
4.  Click **Submit** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466127722_en-US.png)

5.  Publish a node task.

    For more information about the operation, see Release management.


## Create an ODPS MR node {#section_gzm_hbj_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15404466127651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **ODPS MR**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466127723_en-US.png)

3.  Edit the node code. Double click the new ODPS MR node and enter the following interface:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466137724_en-US.png)

    Node code editing example:

    ```
    jar -resources base_test.jar -classpath ./base_test.jar com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll
    ```

    The code is described below:

    -   `-resources base_test.jar`: indicates the file name of the referenced jar resource.
    -   `-classpath`: jar package path.
    -   `com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll`: indicates the main class in the jar package that is called during execution. It must be consistent with the main class name in the jar package.
    When one MR calls multiple jar resources, classpath must be written as follows: `-classpath ./xxxx1.jar,./xxxx2.jar`, that is, two paths must be separated by a comma.

4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling    configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

5.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


