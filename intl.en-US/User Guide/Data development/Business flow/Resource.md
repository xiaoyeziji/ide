# Resource {#concept_m54_1mz_p2b .concept}

If you want to use .jar, you need you upload it to the project's resource.

You can upload text files, ODPS tables, and various compressed packages \(such as .zip, .tgz, .tar.gz, .tar, and .jar\) as different types of resources to ODPS. Then, you can read or use these resources when running UDFs or MapReduce.

ODPS provides APIs for reading and using resources. The following types of ODPS resources are available:

-   File
-   Archive: The compression type is identified by the extension in the resource name. The following compressed file types are supported: .zip, .tgz, .tar.gz, .tar, and .jar.
-   Jar: compiled Java jar packages.

On DataWorks, the process of creating a resource is a process of adding a resource. Currently, DataWorks supports addition of three types of resources in a visual manner, including the jar, file resources. The newly created entries are the same. The differences are as follows:

-   Jar resource: You need to compile the Java code in the offline Java environment, compress the code into a jar package, and upload the package as the jar resource to ODPS.
-   Small files: These resources are directly edited on DataWorks.
-   File resource: When creating file resources, you need to select big files. You can also upload local resource files.

**Note:** The resource package to be uploaded can not exceed 30MB.

## Create a resource instance {#section_cf4_snz_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15438882677651_en-US.png)

2.  Right-click **Resource**, and select **Create Resource** \> **jar**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15438882677720_en-US.png)

3.  The Create Resource dialog box is displayed. Enter the resource name according to the naming convention, set the resource type to jar, select a local jar package to the uploaded, and click **OK** to submit the package in the development environment.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15438882677721_en-US.png)

    **Note:** 

    -   If this jar package has been uploaded on the ODPS client, you must deselect **Uploaded as the ODPS resource. In this upload, the resource will also be uploaded to ODPS**. Otherwise, an error will be reported during the upload process.
    -   The resource name is not necessarily the same as the name of the uploaded file.
    -   Naming convention for a resource name: a string of 1 to 128 characters, including letters, numbers, underlines, and dots. The name is case insensitive. If the resource is a jar resource, the extension is .jar.
4.  Click **OK** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15438882677722_en-US.png)

5.  Release a node task.

    For more information about the operation, see [Publish a task](intl.en-US/User Guide/Data development/Publish management/Publish a task.md#).


