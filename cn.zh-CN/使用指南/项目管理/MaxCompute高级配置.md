# MaxCompute高级配置 {#concept_gpt_dn4_1fb .concept}

您可通过阿里云数加平台项目管理模块中的MaxCompute高级配置操作，对当前项目空间的MaxCompute属性进行管理和配置。

单击项目管理左侧导航栏中的**MaxCompute高级配置**，进入MaxCompute高级配置页面，包括**基本设置**和**自定义用户角色**两个模块。

## 基本设置 {#section_gsn_ln4_1fb .section}

基本设置中可设置MaxCompute的常用权限点。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20193/154817924011291_zh-CN.png)

MaxCompute安全设置：涉及底层MaxCompute相关的权限及安全设置。详情请参见 [安全指南](https://help.aliyun.com/document_detail/27937.html)。

-   使用ACL授权：激活/冻结该开关，相当于Owner账号在MaxCompute Project中执行`set CheckPermissionUsingACL=true/false`操作，默认激活。
-   允许对象创建者访问对象：激活/冻结该开关，相当于Owner账号在MaxCompute Project中执行`set ObjectCreatorHasAccessPermission=true/false`操作，默认激活。
-   允许对象创建者授权对象：激活/冻结该开关，相当于Owner账号在MaxCompute Project中执行`set ObjectCreatorHasGrantPermission=true/false`操作，默认激活。
-   项目空间数据保护：默认冻结，激活/冻结该开关，相当于Owner账号在MaxCompute Project中执行`set ProjectProtection=true/false`。
-   子账号服务：激活/冻结该开关，控制子账号可以访问/禁止访问该MaxCompute Project，默认激活。
-   使用Policy授权：激活/冻结Policy授权机制，相当于Owner账号在MaxCompute project中执行`set CheckPermissionUsingPolicy=true/false`操作，默认激活。
-   启动列级别访问控制：项目空间中的LabelSecurity安全机制默认是关闭的，ProjectOwner可以自行开启，相当于Owner账号在MaxCompute Project中执行`Set LabelSecurity=true/false`，默认关闭 。

## 自定义用户角色 {#section_twg_hv4_1fb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20193/154817924011293_zh-CN.png)

-   角色名称：MaxCompute Project中的角色名称。
-   操作：
    -   查看详情：查看当前角色中包含的成员列表，以及当前角色对表/项目的权限。
    -   成员管理：添加/删除当前角色中的成员。
    -   权限管理：设置并管理当前角色对表/项目的权限。
    -   删除：删除当前角色。
-   新增角色：单击**新增角色**后，可根据弹出框进行角色的新增。

    **说明：** 此处自定义角色所设定权限，将与原有系统权限取并集。


