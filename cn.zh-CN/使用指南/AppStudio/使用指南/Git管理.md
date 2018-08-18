# Git管理 {#concept_as5_pl2_v2b .concept}

本文将为您介绍VCS-Git在IDE中的应用场景和实践（均以阿里云Code作为Git服务的提供商）。

IDE中集成了通用的Git版本管理系统，目前支持[阿里云Code](http://code.aliyun.com/)和[GitHub](https://github.com/)。

## 新建工程关联Git系统 {#section_bmh_flf_v2b .section}

1.  新建工程。

    IDE中新建工程，操作如下：

    ![](images/9669_zh-CN.gif)

    新创建的工程是没有关联Git服务的，如果需要Git服务，需指定关联当前项目至自己的Git仓库。

2.  用户基本信息录入。

    关联Git操作之前，需要对当前用户做一些基本信息录入，其中包含用户名、Email以及对应Git服务商的SSH。

    ![](images/9670_zh-CN.gif)

    将上述操作中最后生成的ssh public key录入阿里云Code或GitHub系统中。以录入阿里云为例，操作如下。

    ![](images/9671_zh-CN.gif)

    信息录入完成后，IDE即可直接关联远程的Git仓库。

3.  新建Git仓库。

    ![](images/9672_zh-CN.gif)

4.  关联Git仓库。

    ![](images/9673_zh-CN.gif)

    关联完成后，在IDE的左侧导航栏增加版本控制标签![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605389674_zh-CN.png)， 激活对应的菜单栏版本下拉列表中的其它选项功能。单击**推送**，把本地代码推送到远程仓库。


## Git操作面板、菜单栏 {#section_s3f_pql_v2b .section}

Git相关的操作均集成在左侧的Git面板，以及顶端的版本控制菜单栏中。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605389675_zh-CN.png)

## Git控制面板 {#section_i5r_zql_v2b .section}

文件在编辑过程中，Git控制面板会动态更新状态。

![](images/9676_zh-CN.gif)

Git控制面板中可以完成基本的git add/rm/commit/revert等大部分操作。

## Git基本操作 {#section_zvd_ndm_v2b .section}

变动文件在Git面板中已列表形式显示，每个文件的显示包含文件名、路径、以及右侧支持的基本操作。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399773_zh-CN.png)

如上图显示，橙色框中包含了可操作按钮，以及文件标识（icon）。

-   **源代码管理**

    您可在此处进行commit、refresh、pull和push等操作。

    -   commit操作：选择![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399680_zh-CN.png)下的commit&push操作。
    -   refresh操作：单击![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399681_zh-CN.png)，刷新当前控制面板内容，相当于执行git status，并刷新界面。
    -   pull/push操作：单击![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399682_zh-CN.png)，根据自己需求选择**拉取**或**推送**。
-   **暂存的更改**
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399683_zh-CN.png)：放弃所有修改，相当于执行git reset。
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399684_zh-CN.png)：文件数量标签。
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399685_zh-CN.png)：显示文件更改。
-   **更改**
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399686_zh-CN.png)：放弃所有更改。
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399687_zh-CN.png)：将所有文件添加至缓存区，相当于执行git add。
    -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399684_zh-CN.png)：文件数量标签。
    -   显示条目包含的操作如下：
        -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399686_zh-CN.png)：放弃修改（revert）。
        -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399687_zh-CN.png)：暂存更改（add）。
        -   ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399688_zh-CN.png)文件改动标识（Modified）。

commit/push操作示例

-   示例一

    ![](images/9692_zh-CN.gif)

-   示例二

    ![](images/9693_zh-CN.gif)


**说明：** 

-   Git客户端逻辑一致，需要用户主动调用push，本地的代码才会推送至远程仓库。
-   与push同理，IDE中不会主动pull远程代码，需要用户主动单击拉取操作维护本地与远程的代码统一。

## Branch管理 {#section_l4q_tfm_v2b .section}

打开分支管理弹窗。

![](images/9694_zh-CN.gif)

窗口下边状态栏中显示当前的Branch名称，单击后即可弹出Branch管理窗口。

## 新建本地分支 {#section_i3g_cgm_v2b .section}

![](images/9695_zh-CN.gif)

分支创建后，会自动切换至新创建分支。

## 创建/切换/合并分支 {#section_um2_ggm_v2b .section}

![](images/9696_zh-CN.gif)

**说明：** 新创建的本地分支，可直接推送至远程，远程分支名与本地一致。

## 使用Diff页面解决merge conflict {#section_vp4_ngm_v2b .section}

![](images/9697_zh-CN.gif)

## Git菜单栏 {#section_pfx_qgm_v2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17729/15345605399698_zh-CN.png)

-   版本菜单栏中的功能大部分可通过Git控制面板操作。
-   目前只能通过菜单栏查看日志。

