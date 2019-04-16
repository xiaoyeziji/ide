# Alibaba Cloud Account Preparations {#concept_rdn_pnp_r2b .concept}

Alibaba Cloud DTplus products uses [RAM](https://www.alibabacloud.com/help/doc-detail/28627.htm) primary account and RAM user logon system.

-   Alibaba Cloud account \(or the primary account\) has ownership of Alibaba Cloud resources and is the main account for metering and billing resource usage. The primary account allows you to create Resource Access Management \(RAM\) users for your enterprise to manage and authorize these RAM user accounts.
-   Created and managed by the RAM primary account, the RAM user has no resources, and cannot perform metering and billing. All RAM users are controlled and paid by their primary account.

Therefore, you must have an Alibaba Cloud account to manage RAM users through RAM before using DTplus products.

## Register an Alibaba Cloud account {#section_fkf_jrp_r2b .section}

If you have not registered an Alibaba Cloud account yet, go to the [Alibaba Cloud Official Website](https://www.alibabacloud.com/) and click **Free Account**. This takes you to the Alibaba Cloud account registration page, where you can create a new Alibaba Cloud account.

**Note:** The Alibaba Cloud system recognizes the primary account you created is a resource-consuming account with high-level permission. Keep your account name and password safe. Change your password periodically and do not lend your account to others.

## Perform real-name registration on the new Alibaba Cloud account {#section_zpd_qrp_r2b .section}

You must perform real-name registration on your Alibaba Cloud account before you purchase and use Alibaba Cloud products. If you have not completed real-name registration yet, go to Real-name Registration to go through the process. To save you any trouble in the future, be sure to complete the real-name registration.

If you are an enterprise user, we strongly recommend you complete the enterprise-level authentication for greater convenience.

## Create an AccessKey {#section_zjv_srp_r2b .section}

To guarantee smooth processing of tasks in DataWorks, you must create an AccessKey. Unlike the account name and password you enter to log on, an AccessKey is primarily used for access permission verification between various Alibaba Cloud products. An AccessKey comprises an AccessKey ID and an AccessKey Secret.

1.  Log on to the Alibaba Cloud official website, and click **AccessKeys** under the Username in the upper-right corner to enter the AccessKey Management page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16174/15514201968934_en-US.png)

2.  Click **Create an AccessKey** in the upper-right corner, and click **Agree and Create** in the dialog window that appears to create an AccessKey.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16174/15514201968935_en-US.png)

3.  When an AccessKey is created, the system automatically jumps to the AccessKey Management page, where you can view the status of the AccessKey and disable or delete it.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16174/15514201968936_en-US.png)

    A disabled AccessKey will affect services that use this AccessKey and cause errors. Therefore, if you change your AccessKeys, you must promptly make the corresponding changes for the products and services that use the affected AccessKeys.

    **Note:** The AccessKey is critical to the account. Keep the AccessKey ID and AccessKey Secret safe. Do not reveal this information to others. If the information may have been leaked, immediately disable and update your AccessKey.


