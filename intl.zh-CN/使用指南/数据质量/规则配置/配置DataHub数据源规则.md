# 配置DataHub数据源规则 {#concept_zty_sxh_r2b .concept}

本文将为您介绍如何配置DataHub数据源。

进入运维中心页面，即可新建数据源。通过配置DataHub的Endpoint、数据源名称、以及AccessKey、AccessID建立数据源连接串。新建后即可在数据质量（DQC）上进行查询。

## 选择数据源 {#section_lgc_zxh_r2b .section}

1.  单击左侧导航栏的**规则配置**，进入规则配置页面。
2.  选择**DataHub数据源**，即可显示当前数据源下所有的Topic。

    您也可通过数据源选择下的搜索功能，快速定位到其他数据源下查看Topic。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/15408928448777_zh-CN.png)


## 配置监控规则 {#section_qp2_myh_r2b .section}

1.  选择需要配置的Topic，单击右侧的**配置监控规则**进入监控规则页面。

    您也可选择**我的订阅** \> **DataHub数据源** \> **Topic名**，快速进入订阅的Topic。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/15408928448778_zh-CN.png)

2.  单击**创建规则**， DataHub数据源目前只可以创建模板规则，数据类型为断流监控。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16395/15408928448783_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**报警频次**|可以设置多久报警一次，有10分钟、30分钟、1小时、2小时四种选择。|
    |**橙色阈值**|以分钟为单位，只可以输入整数，必须小于红色阈值。|
    |**红色阈值**|以分钟为单位，只可以输入整数，必须大于橙色阈值。|

3.  设置完成后，单击**保存**，将创建的规则添加到Topic中。

