# Version {#concept_i3s_m3z_p2b .concept}

A version is a submission and release record of the current node, where each submission generates a new version. You can check the related status, change type, and release remarks as required to facilitate operations on the node.

**Note:** Only a submitted node has the version information.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16305/15519376287932_en-US.png)

-   File ID: The current node ID.
-   Version: A new version is generated for each release. The first release is V1, the second modification is V2, and so on.
-   Submitter: The operator who submits and releases the node.
-   Submission time: The version release time. If a version is submitted and then released, the release time covers the submission time. By default, the last release time of the operation is recorded.
-   Change type: The operation history of the current node. It is set to Added if the node is first released, and set to Modified if the node is modified.
-   Status: The operation status record of the current node.
-   Remarks: Changes the description of the current node when submitted. It facilitates other personnel to locate the related version when operating the node.
-   Action: You can select Code and Roll Back in this column.
    -   View code: Click it to view the version code and precisely search for a record version to be roll back.
    -   Roll back: Click it to roll back the current node to a previous version as required. You must submit the node for release again after roll back.
-   Compare: Click it to compare the code and parameters of two versions.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16305/15519376287933_en-US.png)

    Click **View Details** to go to the details page and compare the code and scheduling attribute changes.

    **Note:** Only two versions can be compared. You cannot compare only one or more than three nodes.


