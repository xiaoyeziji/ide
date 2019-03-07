# Resource {#concept_m54_1mz_p2b .concept}

This topic describes how to create, upload, reference, and download resources.

If you want to use .jar, you need to upload the file to the project resource. You can upload text files, MaxCompute tables, and various compressed package formats, including .zip, .tgz, .tar.gz, .tar, and .jar as different types of resources to MaxCompute. Then, you can read or use these resources while running UDFs or MapReduce.

MaxCompute provides APIs for reading and using resources. The following types of MaxCompute resources are available:

-   File
-   Archive: The compression type is identified by the extension in the resource name. The following compressed file types are supported: .zip, .tgz, .tar.gz, .tar, and .jar.
-   Jar: The compiled Java jar packages.

In DataWorks, to create a resource you need to add a resource . Currently, DataWorks supports adding three resource types in a visual manner, including the .jar and file resources. The newly created entries are the same. but the differences are as follows:

-   Jar resource: You need to compile the Java code in the offline Java environment, compress the code into a jar package, and upload the package as the jar resource to ODPS.
-   Small files: These resources are directly edited on DataWorks.
-   File resource: Select a large file when you create file resources. You can also upload local resource files.

**Note:** The resource package for upload cannot exceed 30 MB.

## Create a resource instance {#section_cf4_snz_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15519378117651_en-US.png)

2.  Right-click **Resource**, and select **Create Resource** \> **Jar**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15519378117720_en-US.png)

3.  The Create Resource dialog box is displayed. Enter the resource name according to the naming convention, and set the resource type to jar. Select a local jar package for upload, and click **OK** to submit the package in the development environment.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15519378117721_en-US.png)

    **Note:** 

    -   If this Jar package has been uploaded to the MaxCompute client, you must deselect **Uploadto MaxCompute**. Otherwise, an error will occur during the upload process.
    -   The resource name is not always the same as the uploaded file.
    -   The naming convention for a resource name: A string can contain 1 to 128 characters, including letters, numbers, underscores \(\_\), and periods \(.\). The name is case insensitive. If the resource is a JAR resource, the extension is .jar.
4.  Click **OK** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15519378117722_en-US.png)

5.  Release a node task.

    For more information about operations, see [Publish a task](reseller.en-US/User Guide/Data development/Publish management/Publish a task.md#).


