# 配置DataHub数据源规则 {#concept_zty_sxh_r2b .concept}

本文将为您介绍如何配置DataHub数据源。

进入数据集成页面，即可新建数据源。通过配置DataHub的Endpoint、数据源名称、以及AccessKey、AccessID建立数据源连接串。新建后即可进入数据质量（DQC）页面进行规则的配置。

## 选择数据源 {#section_lgc_zxh_r2b .section}

1.  单击左侧导航栏的**规则配置**，进入规则配置页面。
2.  选择**DataHub数据源**，即可显示当前数据源下所有的Topic。

    您也可通过数据源选择下的搜索功能，快速定位到其他数据源下查看Topic。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/15505447968777_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**配置Flink/SLS规则**|在创建数据断流规则前，需要先在Flink中创建项目 。|
    |**topic列表**|DataHub数据源下所有topic的名称，您可在相应topic后进行下述操作。    -   **配置监控规则**：对当前topic创建规则，支持创建模板规则和自定义规则。
    -   **订阅管理**：查看当前topic的订阅人，可以快捷修改订阅人和报警方式，也可以配置钉群报警，这里修改的报警方式对所有订阅人有效。
|
    |**维度表**|对Topic创建自定义规则join时使用。当采集到的数据比较有限，则需要对数据流补齐字段。进行数据分析前，将所需的维度信息进行补全，此时需要在数据质量中对这张维度表进行声明。 |

    DataHub支持AliHBase维表、Lindorm维表、RDS维表、OTS（TableStore）维表、TDDL维表、ODPS维表。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638960_zh-CN.png)

    Flink SQL中没有专门为维表设计的DDL语法，使用标准的create table语法即可。但需要额外增加一行`period for system_time`的声明，此行声明定义了维表的周期，即表明该表是一张会变化的表。

    **说明：** 声明一张维表时，必须指明唯一键，维表join时，on的条件必须包含所有唯一键的等值条件。

3.  在topic列表页面选择需要配置的topic，单击右侧的**配置监控规则**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638961_zh-CN.png)

    配置完成后可单击相应规则后的**修改**进行修改。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638962_zh-CN.png)

4.  配置规则。

    单击监控规则页面右上角的**创建规则**，目前支持**模板规则**和**自定义规则**两种类型。

    -   单击**添加模板规则**，目前模板类型包括**数据延迟**和**数据断流**。
        -   选择模板类型为为**数据延迟**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638963_zh-CN.png)

            |配置|说明|
            |:-|:-|
            |**规则名称**|输入规则名称，最多255个字符。|
            |**字段类型**|默认为表级规则。|
            |**模板类型**|**数据延迟**：记录业务时间字段（支持TIMESTAMP和STRING\(yyyy -MM -dd H dd HH:mm:ss\)两种类型）内，数据产生于流入datahub通道的时间差，超过设定时间立即报警。|
            |**告警记录数阈值**|允许出现数据延迟的数量上限，超过上限触发DQC告警。|
            |**业务时间字段**|Topic中时间字段的字段名称，支持TIMESTAMP和STRING\(yyyy-MM-dd HH:mm:ss\)两种类型。|
            |**橙色阈值**|以秒为单位，仅支持输入整数，且必须小于红色阈值。|
            |**红色阈值**|以秒为单位，仅支持输入整数，且必须大于橙色阈值。|

        -   选择模板类型为**数据断流**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638964_zh-CN.png)

            |配置|说明|
            |:-|:-|
            |**规则名称**|输入规则名称，最多255个字符。|
            |**字段类型**|默认为表级规则。|
            |**模板类型**|**数据断流**：允许在某一时间段内没有数据流入，当超过允许时间，则触发告警。配置数据断流前，请先在Flink中购买服务并创建项目。|
            |**告警频次**|告警频次包括10分钟、30分钟、1小时和2小时四种。|
            |**橙色阈值**|以分钟为单位，仅支持输入整数，且必须小于红色阈值。|
            |**红色阈值**|以分钟为单位，仅支持输入整数，且必须大于橙色阈值。|

    -   如果对DataHub规则有其他的使用方式，可单击添加自定义规则进行创建。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638965_zh-CN.png)

        **说明：** 

        -   select的字段必须是一列且能够与橙色阈值和红色阈值进行数值对比。
        -   自定义规则下，from clause必须包含该topic，且包含此topic中所有的列。
        |配置|说明|
        |:-|:-|
        |**规则名称**|输入规则名称，需在topic内唯一，最多支持20个字符。|
        |**规则脚本**|自定义编写SQL来设定规则，select的结果字段必须唯一。        -   示例一：简单SQL

            ```
select id as a from zmr_tst02;
            ```

        -   示例二：与维表join查询，维表名称test\_dim

            ```
select e.id as eid
from zmr_test02 as e 
join test_dim for system_time as of proctime() as w 
on e.id=w.id
            ```

        -   示例三：两个topic进行join查询，另一个topic名称dp1test\_zmr01

            ```
select count(newtab.biz_date) as aa
from (select o.*
from zmr_test02 as o
join dp1test_zmr01 as p
on o.id=p.id)newtab
group by id.biz_date,biz_date_str,total_price,'timestamp'
            ```

|
        |**橙色阈值**|以分钟为单位，仅支持输入整数，且必须小于红色阈值。|
        |**红色阈值**|以分钟为单位，仅支持输入整数，且必须大于橙色阈值。|
        |**最小告警间隔**|允许告警的最小时间差，以分钟为单位。|
        |**附加文本**|对当前自定义topic的描述。|

    配置完成后，单击**保存**，将创建的规则添加到topic中。

5.  单击**查看日志**，查看当前规则的运行日志。
6.  单击**启动监控**。
7.  单击**订阅管理**，您可在此页面查看、修改当前规则的订阅人，也可修改告警通知方式，对所有订阅人生效。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/155054479638967_zh-CN.png)

    您可将任务报警添加到钉钉群中，支持发送群消息、发送群消息并@所有人两种方式。

    **钉钉群机器人**：先添加一个机器人到钉钉群中，通过详情页获取Webhook，然后将Webhook地址复制到订阅管理中，即可添加成功。


