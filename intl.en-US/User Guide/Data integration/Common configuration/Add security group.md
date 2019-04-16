# Add security group {#concept_ec4_cj5_q2b .concept}

This topic describes how to add a corresponding security group when you use DataWorks \(formerly known as Data IDE\) in different regions.

To ensure the databases security and stability, you must add IP addresses or IP segments for accessing the database to [Add whitelist](reseller.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#) or security group of the target instance before using certain database instances. This article describes how to add a corresponding security group when you are using DataWorks \(formerly known as Data IDE\) in different regions.

## Add a security group {#section_djc_kj5_q2b .section}

-   If the data synchronization tasks runs on your ECS resource group, you must authorize the ECS resource group by adding the private/public IP address and port to the ECS security group.
-   If the data synchronization tasks run on the default resource group, you should add the security group based on the ECS machine region. For example, if your ECS is in the North China 2 region, you should add the security group of North China 2 \(Beijing\): 2ze3236e8pcbxw61o9y0 and 1156529087455811, as shown in the following table.

    |Region|Authorization object|Account ID|
    |:-----|:-------------------|:---------|
    |China \(Hangzhou\)|sg-bp13y8iuj33uqpqvgqw2|1156529087455811|
    |China \(Shanghai\)|sg-uf6ir5g3rlu7thymywza|1156529087455811|
    |China \(Shenzhen\)|sg-wz9ar9o9jgok5tajj7ll|1156529087455811|
    |Asia Pacific SE 1\(Singapore\)|sg-t4n222njci99ik5y6dag|1156529087455811|
    |Hong Kong|Sg-j6c28uqpqb27yc3tjmb6|1156529087455811|
    |US West 1 \(Silicon Valley\)|sg-rj9bowpmdvhyl53lza2j|1156529087455811|
    |US East 1|sg-0xienf2ak8gs0puz68i9|1156529087455811|
    |China \(Beijing\)|sg-2ze3236e8pcbxw61o9y0|1156529087455811|

    **Note:** The ECS in the VPC environment does not support adding the preceding security groups.


## Add an ECS security group {#section_vzl_bk5_q2b .section}

1.  Log on to the Administration Console of the cloud server ECS.
2.  Select the **Network and Security** \> **Groups** in the left-hand navigation pane.
3.  Select the target region.
4.  Locate the security group for configuring authorization rules, and click the **Configuration Rule** that is listed in action.
5.  Click **Security Groups**, and click **Add Rules**.
6.  Sets the parameters in dialog box.
7.  Click **Confirm**.

