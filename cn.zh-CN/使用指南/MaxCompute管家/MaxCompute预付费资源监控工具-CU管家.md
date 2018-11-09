# MaxCompute预付费资源监控工具-CU管家 {#concept_ey5_rn3_r2b .concept}

MaxCompute管家为系统运维人员提供系统状态、资源组分配和任务监控三个功能，本文将为您介绍Maxcompute管家的使用方法。

## 前提条件 {#section_att_g43_r2b .section}

使用MaxCompute管家前，您需要购买MaxCompute预付费CU资源，适用于60CU以上的用户。

**说明：** CU过小无法发挥计算资源及管家的优势。如果禁用主账号的AK，会导致无法用相应的子账号管理CU管家。

满足上述条件，您即可登录[DataWorks控制台](https://account.alibabacloud.com/login/login.htm)，选择**我的资源** \> **CU管理**。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15417293698825_zh-CN.jpg)

## 系统状态 {#section_mvr_cp3_r2b .section}

您可以通过系统状态了解CU计算资源和存储的消耗情况。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15417293698827_zh-CN.png)

|操作|说明|
|:-|:-|
|**Quota选择**|可以选择所查看的资源组，根据选择的资源组，展示当前资源组的消耗信息和当前存储量。|
|**选择时间段**|可以选择查看所选资源组的时间区间，选择的区间不同，资源组数据展示的粒度不同（计算资源CU每6分钟采集一次/存储每一小时采集一次）。|

## Quota设置 {#section_dgn_mp3_r2b .section}

Quota指资源组。例如您购买了100CU，表示您全部Quota的额度是100CU。您可以通过大数据管家来新建Quota，这样就可以对Quota进行资源分配。运维人员可以很方便地将各个项目的资源隔离，保证重要项目的计算资源充沛。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15417293698829_zh-CN.png)

|操作|说明|
|:-|:-|
|**新建Quota**|新建一个Quota组，建好后可以通过移动项目功能，来将项目移动到Quota下。创建的Quota可以删除，但如果当前Quota下有项目，则无法删除。|
|**修改CU消耗**|建好的Quota支持修改CU最小消耗值。|
|**移动项目**|支持将当前Quota下的项目移动至其他Quota下，新建的Quota即可通过这个移动项目这个功能来做到资源隔离。|
|**删除**|支持删除Quota组，如果当前Quota下有项目，则无法删除。|

**说明：** 计算资源CU升级和降配时，默认Quota组Max、Min会相应变化，其他组不会改变配置。如果降配时，剩余默认Min组小于降配额度，会降配失败。Max为最大分配的资源，Min为最小保障资源。

Quota组配置示例

例如60CU由两个部门使用。

-   资源组独享：【MaxCU,MinCU】，A组【40,40】，B组【20,20】。
-   资源组倾斜：【MaxCU,MinCU】，A组【60,40】，B组【40,20】。

不同Quota组之间的调度顺序

目前不同的Quota组暂不支持调度优先级设置，Quota的使用遵循先到先得、不抢占的原则。例如60CU由两个部门使用，分配如下。

【MaxCU,MinCU】，A组【40,20】，B组【30,10】

假设A组先使用，占用了40CU的资源，则后使用的B组只能使用20CU的资源，此时B组无法抢占A组已抢占的资源。

假设A组在使用一段时间后，释放了40CU中的10CU资源，则此时B组可以占用30CU资源。

## Instance查询 {#section_nmv_qq3_r2b .section}

您可以通过计算任务监控，了解当前任务排队状态，资源被哪些任务抢占，然后对任务进行分析或停止。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15417293698833_zh-CN.png)

可以根据Quota组名称和项目名称两个维度来筛选，精确搜索。

-   **InstanceId**：每个MaxCompute任务都会有一个Instance，通过单击InstanceId可以跳转到Logview页面，查看具体的任务进度。
-   **账号**：运行这个MaxCompute任务的操作人，可以根据这个账号信息找到任务所属的责任人，如果该任务占用太多资源而影响其他任务的运行，可以与该责任人联系，是否停止该任务。

    停止任务的方法请参见[实例操作](https://www.alibabacloud.com/help/doc-detail/27830.htm)中的Kill Instance。

-   **MaxCompute Project**：当前Instance所属的项目名称。
-   **cpu消耗**：当前Quota组实际使用的CPU资源比例。
-   **memory消耗**：当前Quota组实际使用的内存资源比例。
-   **提交时间**：当前Instance的提交时间。
-   **等待时间**：等待运行资源的时长。
-   **操作**：可以在此处查看当前Instance的状态，此处会显示当前状态和历史状态。

