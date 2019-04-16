# Resource {#concept_yf4_4dl_q2b .concept}

This topic is an overview of resource in Manual Business Flow. Resource is a unique concept in MaxCompute, which supports uploading and submitting the Manual Business Flow, and must be available if you want to use MaxCompute UDFs or MaxCompute MR.

-   ODPS SQL UDF: After compiling a UDF, you must upload the compiled JAR package to ODPS. When running this UDF, ODPS automatically downloads the JAR package, extracts the user code, and runs the UDF. The process of uploading the JAR package is creating a resource in ODPS. The JAR package is a type of ODPS resource.
-   ODPS MapReduce: After compiling a MapReduce program, you must upload the compiled JAR package as a resource to ODPS. When running a MapReduce job, the MapReduce framework automatically downloads this JAR resource and extracts the user code.

Similarly, you can upload text files, ODPS tables, and various compressed packages, such as .zip, .tgz, .tar.gz, .tar, and .jar as different types of resources to ODPS. Then, you can read or use these resources when running UDFs or MapReduce.

The ODPS provides reading and using resources for APIs. The following types of ODPS resources are available:

-   File
-   Archive: The compression type is identified by the extension in the resource name. The following compressed file types are supported: .zip, .tgz, .tar.gz, .tar, and .jar.
-   JAR: The compiled Java JAR packages.

In DataWorks, you can add a resource by creating a resource. Currently, DataWorks supports the addition of three types of resources in a visual manner, including JAR, Python, and file resources. The created new entries are the same, but the differences are as follows:

-   JAR resource: You need to compile the Java code in the offline Java environment, compress the code into a JAR package, and upload the package as the JAR resource to MaxCompute.
-   Small files: These resources are edited on DataWorks.
-   File resource: When creating file resources, you need to select big files. You can also upload local resource files.

## Create a resource instance {#section_f1m_d2l_q2b .section}

1.  Click **Manual Business Flow** in the left-side navigation pane, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334397961_en-US.png)

2.  Right-click **Resource**, and select **Create Resource** \> **JAR**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16318/15525334398001_en-US.png)

3.  The Create Resource dialog box is displayed. Enter the resource name according to the naming convention, set the resource type to JAR, select a local JAR package to upload, and click Submit to submit the package in the development environment.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525334397721_en-US.png)

    **Note:** 

    -   If this JAR package has been uploaded to the ODPS client, you must deselect **Upload to ODPS resource**. Otherwise, an error will be reported during upload.
    -   The resource name is not necessarily the same as the uploaded file name.
    -   Naming convention for a resource name: A string of 1 to 128 characters, including letters, numbers, underscores \(\_\), and periods \(.\). The name is case insensitive. If the resource is a \(\)wewweJAR resource, the file extension is .jar.
4.  Click **Submit** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15525334397722_en-US.png)

5.  Release a node task

    For more information about the operation, see [Publish a task](reseller.en-US/User Guide/Data development/Publish management/Publish a task.md#).


