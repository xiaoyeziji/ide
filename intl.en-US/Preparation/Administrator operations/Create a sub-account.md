# Create a sub-account {#concept_j4w_qnp_r2b .concept}

If you are the only user of this product, skip this section. To create your own account, see [Prepare Alibaba Cloud account](intl.en-US/Preparation/Administrator operations/Prepare Alibaba Cloud account.md#). To invite other users to use this product, you can create and manage your sub-accounts through the Resource Access Management \(RAM\).

## Create a sub-account {#section_v3r_xxp_r2b .section}

1.  Log on to the Alibaba Cloud DTplus Console with the primary account. Choose **RAM** \> **Users**.
2.  Click **Create User**.
3.  Enter the user information.
4.  Click **OK** to complete the user creation.

## Enable a sub-account to log on the console {#section_cyg_nyp_r2b .section}

1.  After the sub-account is successfully created, the main account enters the **RAM** \> **Users** page, click **Manage** from the Actions column.
2.  Click **Enable Console Logon** and reset the password, fill in the logon password of this sub-account.

## Create an AccessKey for a sub-account {#section_z4g_vyp_r2b .section}

The AccessKey facilitates smooth operation of the tasks created in DataWorks. Therefore, a primary account must create an AccessKey for the sub-account, or allow sub-account users to create and manage their own AccessKey information by themselves.

-   **Create an AccessKey for a sub-account**
    1.  The main account enters the **RAM** \> **Users** page, click **Manage** from the Actions column.
    2.  Click **Create AccessKey**, and fill the verification code sent to your mobile phone. The AccessKey is created for the sub-account.

        **Note:** The AccessKeySecret of the sub-account appears only once. You must record and keep it confidential for further use. In case you lose it, create a new one.

-   **Allow sub-account users to create and manage their own AccessKeys**
    1.  The main account enters the **RAM** \> **Settings** page, click **User Security Settings** from the Actions column.
    2.  Select User Security Settings tab, and select **allow the user to manage their own AccessKey**, Click **Save Changes** to complete the settings.

        This option is not checked by default, and the subaccount user cannot create and manage his own AccessKey. After **allow the user to manage their own AccessKey**, sub-account login to the Alibaba cloud, you can create the AccessKey on your own in the console.

        **Note:** It is very important to run the key AK. Whether it is the AK of the primary account or the AK of the sub-account, once it is created successfully, be sure to ensure its security and scope of use. If the AccessKey is leaked, timely prohibit it and create a new one.


## Authorize a sub-account {#section_i12_y5z_jfb .section}

If you want to create projects using your sub-account, you may need you authorize it with **AliyunDataWorksFullAccess** policy.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16176/153896543913331_en-US.png)

## Delivery sub-accounts to other users {#section_sng_tzp_r2b .section}

**Note:** Sub-accounts belong to a primary account. Thus, sub-accounts do not have resources and billing system of their own. The operation cost of a DTplus sub-account is borne by the primary account. Fees incurred by operations of sub-accounts in Alibaba Cloud products shall be uniformly paid through the primary account to which these subaccounts belong. Therefore, the primary account views the RAM user log on link and its own enterprise alias, and provide this information to a sub-account.

Log on to **RAM** \> **Settings**, and click **Enterprise Alias Settings** to view enterprise alias.

When a sub-account is delivered to its users, you must provide the following details:

-   Logon link of RAM users.
-   Enterprise alias of primary account.
-   User name and password of sub-account.
-   AccessKey ID and AccessKeySecret of a sub-account.
-   Confirm that the primary account has allowed your sub-account to Enable Console Logon.
-   Confirm that the primary account has allowed your sub-account to Manage AccessKeys.

