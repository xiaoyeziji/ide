# Log on to DataWorks through a specific IP address with a RAM user {#concept_cpn_hdg_vgb .concept}

In the process of data development , some users with strict permission control require that the RAM user can only log in through a specific IP address in the company. This article describes how to use a RAM user to log on to Dataworks through a specific local IP address.

## Prerequisites {#section_ywq_zx2_4gb .section}

First, you need to refer to [Create a RAM user](../../../../../reseller.en-US/Preparation/Administrator Operations/Create a RAM user.md#) to complete the creation of RAM user and authorize it. The permission **AliyunDataworksFullAccess** is the default system permission and cannot be modified. You need to create an additional custom authorization policy.

## Configure a custom permission policy {#section_hmd_c2g_vgb .section}

Log on to the [RAM console](../../../../../reseller.en-US/Product Introduction/What is RAM?.md#), click **Create Policy** in the **Policies** column to enter the edit page. In this case, the policy name is **dataworksIPlimit1**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155056624538914_en-US.png)

Select **Script** for the option of configuration mode, and enter your custom policy.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155056624538925_en-US.png)

The complete content of the custom policy is shown in the following figure. "acs: SourceIp" is the IP address that you allow access to Dataworks. In this example, it is 100.1.1.1/32. After entering the information, click **OK** to create the authorization.

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

## Add custom permissions {#section_ptd_dlg_vgb .section}

On the RAM console, click **Identities** \> **Users**, choose the RAM user you want to control, and click**Add Permissions**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155056624538926_en-US.png)

Select **Custom Policy**, add the custom policy you just created to the **Selected**, and click**OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155056624538928_en-US.png)

## Verification {#section_ymz_21f_4gb .section}

Log on to the DataWorks console using an IP addresse different from 100.1.1.1/32 and find that the login failed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/125807/155056624538934_en-US.png)

