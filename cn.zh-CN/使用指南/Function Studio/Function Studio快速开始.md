# Function Studio快速开始 {#concept_izh_1x1_rfb .concept}

下面以一个简单的例子来说明如何使用Function Studio开发UDF函数并发布到DataWorks。

## 开始前 {#section_ivq_qz1_rfb .section}

需要对当前用户做一些基本信息录入，其中包含用户名、Email， 对应git服务商的SSH信息。

1.  设置git ssh key和git config

    点击菜单栏的**设置**标签，如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773332978_zh-CN.jpg)

    点击SSH KEY弹出设置框如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773332979_zh-CN.png)

    分别配置好用户阿里云Code服务中的SSH key、以及Git Config中User&Email点击**保存**退出。


## 新建工程 {#section_jdl_k1b_rfb .section}

新建工程有两种方式：新建工程或从git导入工程，入口在菜单栏的工程菜单项里，如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773332981_zh-CN.png)

**新建工程**，输入工程名称和描述。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773332982_zh-CN.png)

目前我们支持JAVA和Python两种语言。

**从git导入工程**

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432983_zh-CN.png)

Function Studio支持直接导入git工程，只支持http格式（支持将SSH方式转化为http方式）。

## UDF开发 {#section_pvz_jbb_rfb .section}

工程创建好后会自动生成一个框架代码，支持新建UDF/UDAF/UDTF，我们以新建UDF为例。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432984_zh-CN.png)

点击**新建** \> **UDF**，在弹出框中输入类名，点击**确认**，统会为您自动生成一些代码，生成的文件内容如下。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432985_zh-CN.png)

我们通过实现evaluate方法里的内容来实现UDF的逻辑。

## UDF调试（目前只支持JAVA） {#section_vl2_1fb_rfb .section}

-   新建main函数

    Function Studio目前支持通过main函数调用UDF进行在线调试，如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432986_zh-CN.png)

-   设置Debug配置

    点击右上角**config**入口，进入配置页面:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432987_zh-CN.png)

    只需要选择刚才建立的main函数，其他的信息都是自动生成的。

    -   Main class：选择需要调试的入口函数，必填
    -   VM options：需要启动的JVM参数，非必填
    -   Program Variables：启动参数，非必填
    -   JRE：目前只允许JDK1.8
    -   PORT：需要开放的HTTP端口，UDF/UDAF/UDTF类的项目这个配置非必填
    -   机器：机器规格
    -   开启HOTCODE：是否需要支持hotcode
-   启动debug

    先在Lower的evaluate函数上打上断点，然后启动debug。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432988_zh-CN.png)

    启动成功之后就可以进行正常的调式。可通过step into进入到UDF方法内，并查看变量值。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432989_zh-CN.png)


## UDAF的调试 {#section_kbf_m3b_rfb .section}

UDAF的调试比UDF稍微复杂一些，您需要自己构造相关数据，并且使用warehouse来模拟odps的数据。warehouse下会保存相关表的Schema以及Data。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432991_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432992_zh-CN.png)

然后编写相关测试的main函数。

首先初始化warehouse，然后调用相关的UDAF进行测试。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432993_zh-CN.png)

## UDTF的调试 {#section_ktp_t5t_xfb .section}

UDTF 与UDAF的调试大致类似，我们已经在初始化工程中构造了UDTF的测试类。如图所示，直接执行该类既可以模拟真实数据对UDTF进行调试。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432995_zh-CN.png)

然后点击**执行**，程序如果没有抛异常则表示运行通过。

## UDF发布 {#section_j4h_xwt_xfb .section}

-   提交资源到DataWorks开发环境

    工程区右上角有一个发布标识，点击弹出如下界面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432996_zh-CN.png)

    点击**提交资源至DataWorks开发环境**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773432997_zh-CN.png)

    填好相应信息，点击**确认**即开始发布，发布完成后会给出资源在DataWorks中的定位链接。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773532998_zh-CN.png)

    复制链接再打开即可定位到该资源

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773532999_zh-CN.png)

-   提交函数到DataWorks开发环境

    点击工程区右上角的发布标识，点击**提交函数到DataWorks开发环境**，在弹出框里将相应信息填写完整。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533002_zh-CN.png)

    点击**确认**即开始发布，发布成功后会同时给出资源和函数的链接。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533003_zh-CN.png)

    复制链接打开函数，在函数详情页里有链接可前往Function Studio进行函数编辑。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533010_zh-CN.png)

    **说明：** 发布后的资源和函数是在开发环境的，需要在任务发布里将资源和函数发布到线上才能在线上使用。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533011_zh-CN.png)


## Map Reduce功能开发 {#section_u4h_225_xfb .section}

工程创建好后，会自动生成框架代码，支持新建Map Reduce任务。已有的WordCount示例代码，下面着重说明从0开始如何测试和发布。

-   新建工程

    在指定MaxCompute工程空间\(cdo\_datax\)下创建项目\(wordcountDemo\)，工程模板选择UDFJAVA Project。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533012_zh-CN.png)

-   项目开发

    在mapred包下，已存在WordCount的MapReduce示例代码。示例代码的功能是对输入表中的单词做次数统计，将统计结果写入到输出表中，输入输出分别是两个表。这里对MapReduce功能开发不做进一步介绍，可参考[MapReduce](../../../../cn.zh-CN/用户指南/MapReduce/概要/MapReduce概述.md#)计算文档。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533013_zh-CN.png)

-   项目调试

    **说明：** 

    MapReduce项目目前不能在Function Studio中进行debug，需要将代码发布到DataWork中进行逻辑验证。所以，Function Studio目前只支持两个功能 ：

    1.  编码
    2.  编译打包
    发布到DataWorks开发环境。之后需要跳转到DataWorks中进行多项手工操作。

-   项目发布

    项目发布将经历以下步骤：

    1.  Function Studio编译打包代码发布到DataWorks开发环境。
    2.  DataWorks建立MapReduce节点进行测试。
    3.  测试通过，将资源包，数据表，节点分别发布到DataWorks正式环境。
    以下详述各个步骤：

    1.  Function Studio发布包到DataWorks开发环境。
        -   点击**发布**按钮，选择**提交资源到DataWorks开发环境**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533014_zh-CN.png)

        -   弹出发布对话框，配置发布选项

            发布jar包的目标业务空间，需要和后续建立的DataWorks计算节点要在一个空间中，这里是DataX数据同步\(cdo\_datax\)。

            资源名称可以自定义，在后续的计算节点脚本中要被引用到。

            名称可以取的和上一次发布一致，选择强制更新会覆盖。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773533015_zh-CN.png)

        -   点击**确认**，编译发布到DataWorks开发环境

            信息提示窗口会输出本次编译发布成功还是失败。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633016_zh-CN.png)

            至此：Function Studio的编译发布结束。

    2.  在DataWorks的开发环境建立计算节点测试。
        -   打开DataWorks同名空间，创建ODPS MR类型节点

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633017_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633018_zh-CN.png)

        -   计算节点需要写入以下固定脚本

            目前脚本的一些变量还需要被手工替换成相关jar包信息

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633019_zh-CN.png)

            **说明：** 

            我们会使用在Function Studio中发布的jar包的信息来替换这段脚本中的信息，生成最后的代码。

            jar -resources：FunctionStudio发布的jar包的名字。

            -classpath：jar包在DataWorks上的路径。

            包含main函数的入口类全类名 main函数参数空格分隔 ``

        -   在**资源**中找到在FunctionStudio中发布的jar包，查看信息，来替换脚本中的相关信息。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633026_zh-CN.png)

            jar包的名称为WordCountDemo\_1.0.0.jar，对应脚本中的-resource。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633030_zh-CN.png)

            路径的名称在查看历史版本中：`http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493`对应脚本中的-classpath。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633040_zh-CN.png)

            最后的脚本：

            ```
            # 手工将刚才从发布jar包中的信息填入脚本, 脚本完成 jar -resources WordCountDemo_1.0.0.jar -classpath http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493 com.alibaba.DataWorks.mapred.WordCount wordcount_demo_input wordcount_demo_output
            ```

        -   任务还需要创建测试表和测试数据

            表结构和测试数据和业务相关

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633042_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773633043_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733044_zh-CN.png)

            数据准备就绪，在开发环境运行脚本，开发环境成功运行。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733045_zh-CN.png)

            至此，WordCount在开发环境的测试全部完成。WordCount的计算节点，jar包，输入输出表都在开发环境，需要分别上线。

    3.  将节点，数据表，资源jar包发布到线上，做线上测试。

        提交计算节点代码

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733046_zh-CN.png)

        发布配置

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733047_zh-CN.png)

        到发布界面做jar包和节点的线上发布。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733048_zh-CN.png)

        勾选提交的jar包和节点，发布到线上。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733049_zh-CN.png)

        将数据表也发布到线上。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733050_zh-CN.png)

        最后，在运维中心对MapReduce任务进行线上测试。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773733051_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833052_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833053_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833054_zh-CN.png)

        日志显示成功运行。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833055_zh-CN.png)

        成功的从0建立MapReduce任务，并且在线上环境运行成功。

        总结：Function Studio可以编码和编译发布代码到DataWorks节点，DataWorks需要手工操作生成计算节点，在开发环境和生产环境分别运行。


## 更多功能 {#section_kds_2q5_xfb .section}

**git管理**

新建的工程可以通过菜单栏里版本里的**初始化&关联远程仓**库和git进行关联，关联完成后可以进行一些常规的git操作。操作入口有：

1.  菜单栏的**版本**
2.  左侧导航栏的**版本管理**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833058_zh-CN.png)

    鼠标移到文件上会出现**+**，相当于git add，**√**是commit&push，点击**...**可以拉取和推送。

3.  底边栏的**master**入口

    可以关联远程和本地分支，可以创建分支。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833060_zh-CN.png)


**协同编辑**

Function Studio支持邀请多人协同编辑同一工程同一文件。在右侧栏有一个**share**入口，可以邀请队友。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833061_zh-CN.png)

共享的工程在**我参与的**工程列表中。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833062_zh-CN.png)

多人同时编辑同一个工程同一个文件的效果。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/154399773833063_zh-CN.png)

