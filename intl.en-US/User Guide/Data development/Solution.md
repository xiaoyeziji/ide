# Solution {#concept_ykq_3zb_p2b .concept}

This topic describes how to perform the data development mode. The data development mode has been upgraded to the three-level structure comprising of project, solution, and business flow. This data development mode abandons the conventional directory organization mode.

## Project-solution-business flow {#section_k24_vzb_p2b .section}

In the latest versions of DataWorks, the data development mode is upgraded to integrate different types of node tasks based on business types, such as structure better code development facilitation by businesses. In the development process, the development can be implemented across multiple business flows from a wider viewing angle. Based on the three-level structure of the project-solution-business flow, the development process is re-defined to improve the users' development experience.

-   Project: The basic unit for permission organization that is used to control user permissions, such as development and O&M permissions. In the same project, all project members codes can be developed and managed in a collaborative manner.
-   Solution: Users can customize a solution by combining some business flows. Advantages:
    -   A solution contains multiple business flows.
    -   The same business flow can be reused in different solutions.
    -   The immersion development can be implemented for a combined solution.
-   Business flow: An abstract entity of the business, which enables users to organize data code development from the business perspective. A business flow can be reused by multiple solutions. Advantages:
    -   The business flow helps users organize codes from the business perspective. It provides the task type-based code organization mode. It supports multiple levels of sub-directories \(preferentially up to four-levels\).
    -   The entire business flow can be viewed and optimized from the business perspective.
    -   The business flow dashboard is provided to improve the development efficiency.
    -   Release and O&M can be organized based on the business flow.

## Immersion development experiences {#section_gdm_vbc_p2b .section}

You can double-click any created solution to switch from the development area to the solution area. The directory displays only the current solution content. You are provided with a fresh environment, and will not be troubled by other project codes that are irrelevant to the current solution.

1.  Go to the DataStudio page and create a solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15519375017601_en-US.jpg)

2.  Select the business flow for viewing from the created solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15519375017604_en-US.jpg)

3.  Right-click **View All Business Flows** to view nodes of the selected business flow or modify the solution.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16287/15519375017603_en-US.jpg)

4.  Go to another page.

    -   Click **Publish** to go to the Task Publish page. Nodes in the **To Be Released** status under the current solution are displayed on this page.
    -   Click **O&M** to go to the **O&M Center** \> **Periodic Instances** page. By default, periodic instances of all nodes under the current solution are displayed on this page.
    A business flow can be reused by multiple solutions, which allows you to focus on solution development. Other users can edit your referenced business flows, or business flows in other solutions, and implement collaborative development.


