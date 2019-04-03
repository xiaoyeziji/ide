# Simple mode and standard mode {#concept_z2j_nwp_r2b .concept}

The new version of DataWorks introduces both simple and standard modes, this article introduces you to the differences between simple and standard modes.

## Simple Mode {#section_bkq_wks_s2b .section}

A simple mode refers to a DataWorks project that corresponds to a MaxCompute project and cannot set up a development and Production Environment, you can only do simple data development without strong control over the data development process and table permissions.

The advantage of the simple mode is that the iteration is fast, and the code is submitted without publishing, it will take effect.

The risk of a simple mode is that the development role is too privileged to delete the tables under this project, there is a risk of table permissions.

## Standard Mode {#section_zlj_yks_s2b .section}

Standard mode refers to a DataWorks project corresponding to two MaxCompute projects, which can be set up to develop and produce dual environments, improve code development specifications and be able to strictly control table permissions, the operation of tables in Production Environments is prohibited, and the data security of production tables is guaranteed.

-   All Task edits can be performed only in the Development Environment, and the Production Environment Code cannot be directly modified, reduce the Production Environment code modification entry, as much as possible to ensure the Production Environment code stability.
-   The Development Environment does not turn on task scheduling by default, avoid the development of environmental project cycle operation and production of environmental projects to seize resources, the stability of the operation of Production Environment tasks is better guaranteed.
-   The Production Environment runs with a default production account, all the tables produced by the production account belong to the main account, you need to use production tables during the development process, all of which need to be applied separately, better control of table permissions.

When creating a project, select **project mode** as the standard mode, fill in the project name and project description, the remaining configuration item select the default value.

**Note:** The MaxCompute access identity of the Production Environment cannot be modified to a personal account, otherwise, the data security of the Production Environment cannot be guaranteed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16418/15542614189027_en-US.png)

