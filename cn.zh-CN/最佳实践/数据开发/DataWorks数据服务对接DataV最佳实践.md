# DataWorks数据服务对接DataV最佳实践 {#concept_sv4_hjn_4gb .concept}

DataV通过与DataWorks数据服务的对接，可使用DataWorks数据服务开发数据API，快速在DataV中调用API并展现MaxCompute的数据分析结果。

MaxCompute是阿里巴巴集团自主研究的快速、完全托管的TB/PB/EB级数据仓库解决方案。当今社会数据收集的方式不断丰富，行业数据大量积累，导致数据规模已增长到传统软件行业无法承载的海量级别。MaxCompute服务于批量结构化数据的存储和计算，已经连续多年稳定支撑阿里全部的离线分析业务。

过去，如果您想要通过DataV展示海量数据的分析结果，需要自建一套离线数据计算自动导入MySQL的任务流程，这个过程非常繁琐，且成本很高。而现在通过DataWorks为您提供的**数据集成** \> **数据开发** \> **数据服务**的全链路数据研发平台，并结合MaxCompute可快速搭建企业数仓。

DataWorks数据服务提供了快速将数据表生成API的能力，通过可视化的向导模式操作，无需代码便可快速生成API，然后通过DataV调用API并在大屏中展示数据分析结果，高效实现数仓的开发和数据的展示。

## 数据服务对接DataV产生背景 {#section_g5b_ljn_4gb .section}

本文为您介绍如何实现DataWorks数据服务与DataV联合进行API开发，并通过大屏可视化展现数据分析结果。

## 前提条件 {#section_hf5_w4n_4gb .section}

要想实现DataWorks数据服务与DataV的对接，您需提前准备好数据源，并开通[DataV服务](../../../../../cn.zh-CN/产品简介/什么是DataV数据可视化.md#)。

## 新建数据源 {#section_vt3_gsn_4gb .section}

数据服务支持丰富的数据源类型，如下所示。

-   关系型数据库：RDS/DRDS/MySQL/PostgreSQL/Oracle/SQL Server
-   分析型数据库：AnalyticDB（ADS）
-   NoSQL数据库：TableStore（OTS）/MongoDB
-   大数据存储：Lightning（MaxCompute）

1.  登录DataWorks控制台，单击对应项目后的**进入数据服务**。
2.  在服务开发页面，单击新建按钮，选择**新建数据源**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838177_zh-CN.png)

3.  进入**数据集成** \> **同步资源管理** \> **数据源**页面，单击右上角的**新增数据源**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838180_zh-CN.png)

    本文将以Lightning数据源为例，通过Lighning数据源可以直接实时查询MaxCompute中的数据。

    **说明：** Lightning目前是内测阶段，需要单独申请才能开通，您也可以加入数据服务用户群（钉钉群号21993540）咨询Lightning服务的开通事项。

4.  单击**Lightning**，填写新增Lightning数据源对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838185_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**Lightning Endpoint**|[Lightning的连接信息](../../../../../cn.zh-CN/用户指南/交互式分析 (Lightning)/ 通过JDBC连接服务/配置JDBC连接.md#)。|
    |**Port**|默认值为443。|
    |**MaxCompute项目名称**|填写MaxCompute的项目名称。|
    |**AccessKey ID/AccessKey Secret**|访问密匙（AccessKeyID和AccessKeySecret），相当于登录密码。|
    |**JDBC扩展参数**|JDBC扩展参数中的sslmode=require&prepareThreshold=0是默认且不可删除的，否则会无法连接。|

5.  单击**测试连通性**，测试通过后，单击**完成**。

## 新建API {#section_jz5_ywn_4gb .section}

数据源创建完成后，进入数据服务页面，本文以向导模式生成API为例，为您介绍如何新建API。

1.  选择**服务开发** \> **新建** \> **生成API** \> **向导模式**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838187_zh-CN.png)

2.  填写生成API对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838190_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**API名称**|支持汉字、英文、数字、下划线，且只能以英文或汉字开头，4~50个字符。|
    |**API分组**|API分组是指针对某一个功能或场景的API集合，也是API网关对API的最小管理单元。您可单击**新建API分组**进行新建。|
    |**API Path**|API的存放路径，必须以/开头，如/user。|
    |**协议**|目前仅支持HTTP协议。|
    |**请求方式**|默认选择**GET**请求方式。|
    |**返回类型**|默认返回JSON类型。|
    |**描述**|对API进行简单描述。|

3.  单击**确认**，进入API配置页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997838199_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**选择表**|依次选择数据源类型、数据源名称和数据表名称进行表设置。    -   **数据源类型**：此处选择上一步创建的Lightning数据源。
    -   **数据源名称**：选择对应的数据源名称。
    -   **数据表名称**：选择要查询的MaxCompute表，此处以成交金额表demo\_trade\_amount为例，该表中存储了一个月的成交金额数据。
|
    |**选择参数**|选择表后，会自动展示表的字段列表。然后勾选要作为API请求参数的字段和作为返回参数的字段。**说明：** 本示例中，为了查询成交金额趋势，需要返回所有数据，即日期和成交金额都作为返回参数，不设请求参数。

|

    单击右侧的**返回参数**，设置参数信息。

    **说明：** 如果不设置请求参数，则需要开启**返回结果分页**开关，进行分页查询，以避免单次查询返回数据量过大影响性能。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938205_zh-CN.png)

4.  单击工具栏右边的**测试**，填写API请求参数（由于打开了分页查询开关，系统会自动添加两个分页参数），单击**开始测试**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938206_zh-CN.png)

    您可在测试页面看到API延迟，会发现通过Lightning查询MaxCompute表仅花费了1秒多，比直接通过MaxCompute SQL查询更高效。


## 发布API {#section_plf_pf3_pgb .section}

新建API完成后，单击工具栏右侧的**发布**，即可将API发布。

发布完成后，您可以单击顶部导航栏中的**服务管理**查看API详情。

如果您要调用API，可进入**服务管理****API调用**页面，数据服务为您提供简单身份认证（AppCode）和加密签名身份认证（AppKey&AppSecret）两种认证方式，您可以自由选择。下文将为您介绍如何在DataV中进行数据服务API的调用。

## 添加数据服务为数据源 {#section_ink_zg3_pgb .section}

1.  登录DataV控制台。
2.  进入我的数据页面，单击**添加数据**。
3.  填写添加数据对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938233_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**类型**|添加的数据源类型。|
    |**自定义数据源名称**|数据源的显示名称，可以自由命名。|
    |**项目**|DataWorks项目（工作空间）。|
    |**AppKey/AppSecret**|拥有DataWorks数据服务中某一项目访问权限的账号的AppKeyID和AppSecret。**说明：** 您可登录DataWorks数据服务控制台，进入**服务管理** \> **API调用**页面进行查看。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938235_zh-CN.png)

|


## 在大屏中调用数据服务API {#section_jxk_tm3_pgb .section}

1.  进入DataV控制台中的我的可视化页面，单击**新建可视化**。
2.  选择一个模板，单击**创建**，本文以**智能工厂**模板为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938239_zh-CN.png)

    **说明：** 模板中的组件自带了静态数据，下文将以把模板中间的基本折线图改为调用上文创建好的查询成交金额增长趋势的API为例，为您介绍如何在组件中使用数据服务API。

3.  选中基本折线图组件，切换到数据面板，在**数据源类型**中选择**DataWorks数据服务**。
4.  选择刚刚创建的数据源和API，并设置查询参数，本示例将pageSize设置为31，以查询一个月的数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938242_zh-CN.png)

5.  单击**查看数据响应结果**，即可查看API的查询结果。
6.  填写字段映射关系，在x中填写**date**，将日期作为横轴，在y中填写**amount**，将成交金额作为纵轴。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938246_zh-CN.png)

    **说明：** 由上图可见，当前x和y无法匹配到字段。这是因为DataV对数据格式有一定要求，不能识别结构较深的字段，因此需要添加一个数据过滤器，过滤掉不必要的字段，在本例中直接返回rows数组即可。

7.  勾选**使用过滤器**，单击**新建**图标。这里支持编写JS代码对数据结果进行二次过滤和处理，过滤器的data参数为API返回结果JSON对象。

    **说明：** 本示例只需返回API结果中的rows数组，因此您只需输入`return data.data.rows;`，便可在下方预览过滤后的结果，并单击**完成**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938247_zh-CN.png)

    添加过滤器后，字段便会匹配成功。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073997938248_zh-CN.png)

    **说明：** 但此时的折线图并没有正确展示，由于API返回的日期格式与组件默认的格式不一样，因此还需要设置一下折线横轴的日期格式。

8.  切换至配置面板，在**x轴** \> **轴标签**中选择数据种类为**时间型**，数据格式选择本API所返回的格式2016/01/01，即可看见折线图的正常展示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073998038249_zh-CN.png)


至此，便完成了通过数据服务将MaxCompute表生成API，然后在DataV数据大屏中进行展示的所有操作，效果如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120377/155073998038250_zh-CN.png)

## 注意事项 {#section_l2c_qvj_pgb .section}

DataWorks数据服务与DataV进行无缝对接后，则不需要使用DataV中的API数据源去填写一个URL调用API，直接新建一个DataWorks数据服务作为数据源，便可直接选用数据服务中的API，无需每个API都设置AppKey和AppSecret认证信息，且支持通过表单填写API参数，操作快捷方便并安全可靠。

通过数据服务，您可以将MaxCompute中加工好的数据结果，直接在DataV中进行呈现，实现数据开发-数据服务-数据分析展现的全链路开发。在开发过程中，请注意以下事项。

-   DataWorks数据服务向导模式生成API只支持单表简单条件查询，脚本模式支持用户编写查询SQL语句，支持多表关联查询、函数以及复杂条件。您可根据自己的需求灵活选择。
-   [Lightning](../../../../../cn.zh-CN/用户指南/交互式分析 (Lightning)/概述.md#)采用的是PostgreSQL语法，在编写SQL时，需要注意使用PostgreSQL函数，而不是MaxCompute的UDF。目前Lightning仅支持MaxCompute的UDF max\_pt，可用于获取当前最新分区。连接字符串时使用||。
-   Lightning目前只支持秒级查询，并且查询的MaxCompute不宜过大（控制在GB级），尽量将分区作为请求参数，尽量避免扫描过多分区，否则会比较慢。
-   如果您要求毫秒级API查询，则建议采用关系型数据库、NoSQL数据库或AnalyticDB作为数据源。
-   DataV组件要求的数据格式是个数组，数据服务生成的API返回结果是带有错误码的完整JSON，因此要使用过滤器对API结果进行处理。您可以选择在DataV中添加过滤器，也可以选择直接在数据服务配置API时添加过滤器。

    一般来说，对于未分页查询的API，直接返回data数组即可，对于分页查询的API直接返回data.rows数组。

-   若您要在DataV的折线图或柱状图中添加多个系列，DataV一般要求每个系列的数据是一个对象，并通过字段s来区分系列，此时要注意使用过滤器进行格式转换。

