# Add security group {#concept_ec4_cj5_q2b .concept}

This article describes how to add a corresponding security group when you are using DataWorks \(formerly Data IDE\) in different regions.

To ascertain the security and stability of databases, you must add the IP addresses or IP segments used for accessing the database to the [Add whitelist](intl.en-US/User Guide/Data Integration/Common configuration/Add whitelist.md#) or security group of the target instance before using certain database instances. This article describes how to add a corresponding security group when you are using DataWorks \(formerly Data IDE\) in different regions 

## Add a security group {#section_djc_kj5_q2b .section}

-   If your data synchronization tasks run on your own ECS resource group, you should authorize your ECS resource group by adding its private/public IP and port to the ECS security group.
-   If your data synchronization tasks run on the default resource group, you should add your security group based on your ECS machine region. For example, if your ECS is North China 2, you should add the security group based on North China 2 \(Beijing \): 2ze3236e8pcbxw61o9y0 and 1156529087455811, as shown in the following table.

    |Region|Authorization object|Account ID|
    |:-----|:-------------------|:---------|
    |China \(Hangzhou\)|sg-bp13y8iuj33uqpqvgqw2|1156529087455811|
    |China \(Shanghai\)|sg-uf6ir5g3rlu7thymywza|1156529087455811|
    |China \(Shenzhen\)|sg-wz9ar9o9jgok5tajj7ll|1156529087455811|
    |Singapore|sg-t4n222njci99ik5y6dag|1156529087455811|
    |Hong Kong|Sg-j6c28uqpqb27yc3tjmb6|1156529087455811|
    |US \(Silicon Valley\)|sg-rj9bowpmdvhyl53lza2j|1156529087455811|
    |China \(Beijing\)|sg-2ze3236e8pcbxw61o9y0|1156529087455811|

    **Note:** ECS in VPC environment does not support adding the above security groups.


## Add an ECS security group {#section_vzl_bk5_q2b .section}

1.  Log on to the Administration Console of the cloud server ECS.
2.  Select the **network and security** \> **groups** in the left-hand navigation bar.
3.  Select the target region.
4.  Locate the security group where you want to configure authorization rules, and click the **configuration rule** that is listed in the action.
5.  Click **Security Groups** and click **Add Rules**.
6.  Sets the parameters in the pop-up dialog box.
7.  Click **Confirm**.

