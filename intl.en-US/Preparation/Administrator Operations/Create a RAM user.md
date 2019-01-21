# Create a RAM user {#concept_j4w_qnp_r2b .concept}

If you are the only user of this product, skip this section. To create your own account, see [Alibaba Cloud Account Preparations](reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#). To invite other users to use this product, you can create and manage RAM user accounts through the Resource Access Management \(RAM\) console.

## Create a sub-account {#section_v3r_xxp_r2b .section}

1.  Log on to the Alibaba Cloud DTplus Console with the primary account. Choose **RAM** \> **Indentities** \> **Users**.
2.  Click **Create User**.
3.  Enter the user information.
4.  Click **OK** to complete the user creation.

## Enable a RAM user to log on the console {#section_cyg_nyp_r2b .section}

1.  After the RAM user is created, logs on to the **RAM** \> **Users** page with your primary account, and click RAM user account.
2.  Click **Modify Logon Settings** and **Console Password Logon**, enter the logon password of this RAM account.

## Create an AccessKey for a RAM user {#section_z4g_vyp_r2b .section}

The AccessKey facilitates smooth operation of the tasks created in DataWorks. Therefore, a primary account must create an AccessKey for the RAM user, or allow RAM account users to create and manage their own AccessKey information.

-   **Create an AccessKey for a RAM user**
    1.  Use your primary account to log on to the **RAM** \> **Indentities** \> **Users** page, and click RAM user account.
    2.  Click **Create AccessKey**, and enter the verification code sent to your mobile phone. The AccessKey is created for the RAM user account.

        **Note:** The AccessKeySecret of the RAM user will appear only once. Please record the AccessKeySecret and keep it confidential. If you lose it, create a new one.

-   **Authorize the RAM users to create and manage their own AccessKeys**
    1.  The main account enters the **RAM** \> **Indentifies** \> **Settings** page, click **update RAM User Security Settings** from the Actions column.
    2.  Select RAM User Security Settings tab, and select **Manage AccessKey** \> **allowed**, click **OK** to complete settings.

        If this option is not selected, by default the RAM user cannot create nor manage AccessKey. After **allow the user to manage their own AccessKey**, RAM user account logon to Alibaba cloud, you can create the AccessKey in the console.

        **Note:** It is important to run the private AccessKey. Keep the created AccessKey of the primary account or the RAM account secure and ensure its scope of use. If the AccessKey is leaked, timely disable it and create a new one.


## Authorize a RAM account {#section_i12_y5z_jfb .section}

If you want to create projects with your RAM account, you may need **AliyunDataWorksFullAccess** policy permission.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16176/154804175913331_en-US.png)

## Delivery RAM account to other users {#section_sng_tzp_r2b .section}

**Note:** RAM users belong to a primary account. Thus, RAM users do not have resources and billing systems of their own. The operation cost of a DTplus RAM user is borne by the primary account. Fees incurred by operations of RAM users in Alibaba Cloud products are uniformly paid through the primary account to which these RAM users belong. Therefore, the primary account views the RAM user log on link and its own enterprise alias \(default domain\), and provide this information to a RAM user.

Log on to **RAM** \> **Overview**, and click **Update default domain** to change the enterprise alias \(default domain\).

When a RAM account is delivered to its users, you must provide the following details:

-   Logon link of RAM users.
-   Enterprise alias \(default domain\) of primary account.
-   User name and password of RAM users.
-   AccessKey ID and AccessKeySecret of a RAM user.
-   Confirm the primary account authorized your RAM user Enable Console Logon permission.
-   Confirm the primary account authorized your RAM user Manage AccessKeys permission.

