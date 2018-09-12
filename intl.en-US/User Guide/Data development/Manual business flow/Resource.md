# Resource {#concept_yf4_4dl_q2b .concept}

Resource is a concept unique in ODPS. Resources must be available if you want to use ODPS UDFs or ODPS MR.

-   ODPS SQL UDF: After compiling a UDF, you must upload the compiled jar package to the ODPS. When running this UDF, ODPS automatically downloads the jar package, extracts the user code, and runs the UDF. The process of uploading the jar package is the process that a resource is created in ODPS. The jar package is a type of ODPS resource.
-   ODPS MapReduce: After compiling a MapReduce program, you must upload the compiled jar package as a resource to ODPS. When running a MapReduce job, the MapReduce framework automatically downloads this jar resource and extracts the user code.

Similarly, you can upload text files, ODPS tables, and various compressed packages \(such as .zip, .tgz, .tar.gz, .tar, and .jar\) as different types of resources to ODPS. Then, you can read or use these resources when running UDFs or MapReduce.

ODPS provides APIs for reading and using resources. The following types of ODPS resources are available:

-   File
-   Archive: The compression type is identified by the extension in the resource name. The following compressed file types are supported: .zip, .tgz, .tar.gz, .tar, and .jar.
-   Table: tables in ODPS.
-   Jar: compiled Java jar packages.

On DataWorks, the process of creating a resource is a process of adding a resource. Currently, DataWorks supports addition of three types of resources in a visual manner, including the jar, Python, file resources. The newly created entries are the same, the differences are as follows:

-   Jar resource: You need to compile the Java code in the offline Java environment, compress the code into a jar package, and upload the package as the jar resource to ODPS.
-   Small files: These resources are directly edited on DataWorks.
-   File resource: When creating file resources, you need to select big files. You can also upload local resource files.

## Create a resource instance {#section_f1m_d2l_q2b .section}

1.  Click **Manual Business Flow** in the left-hand navigation bar, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15367339377961_en-US.png)

2.  Right-click **Resource**, select **Create Resource** \> **jar**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16318/15367339378001_en-US.png)

3.  The Create Resource dialog box is displayed. Enter the resource name according to the naming convention, set the resource type to jar, select a local jar package to the uploaded, and click Submit to submit the package in the development environment.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15367339377721_en-US.png)

    **Note:** 

    -   If this jar package has been uploaded on the ODPS client, you must deselect **Uploaded as the ODPS resource. In this upload, the resource will also be uploaded to ODPS**. Otherwise, an error will be reported during the upload process.
    -   The resource name is not necessarily the same as the name of the uploaded file.
    -   Naming convention for a resource name: a string of 1 to 128 characters, including letters, numbers, underlines, and dots. The name is case insensitive. If the resource is a jar resource, the extension is .jar.
4.  Click **Submit** to submit the resource to the development scheduling server.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15367339377722_en-US.png)

5.  Release a node task

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).


