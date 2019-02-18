# 实现RAM子账号仅从特定IP登录DataWorks最佳实践 {#concept_cpn_hdg_vgb .concept}

在数据开发过程中，部分对权限控制较为严格的用户要求RAM子账号仅能通过公司内某个特定IP进行登录。本文为您介绍如何实现RAM子账号仅从特定本地IP登录DataWorks。

## 前提条件 {#section_ywq_zx2_4gb .section}

您需要首先参考[准备RAM子账号](../../../../../cn.zh-CN/准备工作/管理员使用云账号/准备RAM子账号.md#)完成RAM子账号创建，并且给子账号授权。权限 **AliyunDataWorksFullAccess**为系统默认权限，无法修改，您需要额外创建一个自定义授权策略。

## 配置自定义权限策略 {#section_hmd_c2g_vgb .section}

登录[RAM控制台](../../../../../cn.zh-CN/产品简介/什么是 RAM？.md#)，在**权限策略管理**一栏点击**新建授权策略**，进入编辑页面，本例中权限名称为**dataworksIPlimit1**。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155047464238914_zh-CN.png)

选择脚本配置，输入您的自定义权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155047464238925_zh-CN.png)

自定义权限完整内容如下图所示，其中，"acs:SourceIP"后接IP即为您允许访问DataWorks的IP，本例中为100.1.1.1/32。完成输入后，点击**确定**创建授权。

```language-json
{
      "Version": "1",
      "Statement":
        [{
          "Effect": "Deny",
            "Action": ["dataworks:*"],
            "Resource": ["acs:dataworks:*:*:*"],
            "Condition":
             {
                "NotIpAddress":
                 {
                    "acs:SourceIp": "100.1.1.1/32"
                  }
              }
         }]
}
```

## 添加自定义权限 {#section_ptd_dlg_vgb .section}

在RAM访问控制台，**人员管理** \> **用户**处选择您需要控制的RAM子账号，点击**添加权限**。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155047464238926_zh-CN.png)

选择**自定义权限策略**，将您刚创建的自定义权限添加到**已选择**，点击**确定**。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155047464238928_zh-CN.png)

## 结果验证 {#section_ymz_21f_4gb .section}

使用不同于100.1.1.1/32的地址登录DataWorks控制台，发现登录失败。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155047464238934_zh-CN.png)

