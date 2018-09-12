# Solution {#concept_ykq_3zb_p2b .concept}

The data development mode has been upgraded to the three-level structure\(project-solution-business flow\), and the traditional directory organization mode is no longer used.

## Project-solution-business flow {#section_k24_vzb_p2b .section}

In the new version of DataWorks, the data development mode is upgraded to integrate different types of node tasks based on business types. Such a structure better facilitates code development by business. In the development process, development can be implemented across multiple business flows from a wider viewing angle. Based on the three-level structure of project-solution-business flow, the development process is re-defined to improve users' development experience.

-   Project: It is the basic unit for permission organization and used to control user permissions, such as development and O&M permissions. In the same project, all codes of project members can be developed and managed in a collaborative manner.
-   Solution: Users can customize a solution by combining some business flows. Advantages:
    -   A solution contains multiple business flows.
    -   The same business flow can be reused in different solutions.
    -   Immersive development can be implemented for a combined solution.
-   Business flow: It is an abstract entity of business, which enables users to organize data code development from the business point of view. A business flow can be reused by multiple solutions. Advantages:
    -   The business flow helps users better organize codes from the business point of view. It provides the task type-based code organization mode. It supports multiple levels of sub-directories \(preferentially up to four levels\).
    -   The entire workflow can be viewed and optimized from the business point of view.
    -   The business flow dashboard is provided to improve the development efficiency.
    -   Release and O&M can be organized based on the business flow.

## Immersive development experience {#section_gdm_vbc_p2b .section}

You can double-click any created solution to switch from the development area to the solution area. The directory displays only the content of the current solution. You are provided with a fresh environment, and will not be troubled by other codes of the project that are irrelevant to the current solution.

1.  Go to the DataStudio page and create a solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15367305777601_en-US.jpg)

2.  Select the business flow to be viewed from the created solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15367305777604_en-US.jpg)

3.  Right-click **View All Business Flows** to view the nodes of the selected business flow or modify the solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15367305777603_en-US.jpg)

4.  Go to another page.

    -   Click **Publish** to go to the Task Publish page. Nodes in the **To be released** state under the current solution are displayed on this page.
    -   Click **O&M** to go to the **O&M Center** \> **Periodic Instances** page. Periodic instances of all nodes under the current solution are displayed by default on this page.
    A business flow can be reused by multiple solutions. You only need to immerse yourself in development of your own solutions. Other users can directly edit business flows referenced by you in other solutions or business flows, implementing collaborative development.


