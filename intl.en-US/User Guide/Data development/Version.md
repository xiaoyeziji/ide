# Version {#concept_i3s_m3z_p2b .concept}

A version is a submission and release record of the current node, each submission generates a new version.  You can check the related status, change type, and release remarks as required to facilitate operations on the node.

**Note:** Only a submitted node has the version information.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16305/15367330897932_en-US.png)

-   File ID: ID of the current node.
-   Version: A new version is generated for each release. The first release is V1, the second modification is V2, and so on.
-   Submitter: Operator who submits and releases the node.
-   Submission Time: Version release time. If a version is submitted and then released, the release time covers the submission time. By default, the last release time of the operation is recorded.
-   Change Type: Operation history of the current node. It is set to Added if the node is first released, and set to Modified if the node is modified.
-   Status: Operation status record of the current node.
-   Remarks: Change description of the current node when it is submitted. It facilitates other personnel to locate the related version when operating the node.
-   Action: You can select Code and Rollback in this column.
    -   Code: Click it to view the version code and precisely search for a record version to be rolled back.
    -   Rollback: Click it to roll back the current node to a previous version as required. You must submit the node for release again after rollback.
-   Compare: Click it to compare the code and parameters of two versions.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16305/15367330897933_en-US.png)

    Click **View Details** to go to the details page and compare the code and scheduling attribute changes.

    **Note:** Only two versions can be compared. One or more than three \(including three\) nodes cannot be compared.


