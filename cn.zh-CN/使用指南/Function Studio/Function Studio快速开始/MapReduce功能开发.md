# MapReduce功能开发 {#concept_td3_c1j_5gb .concept}

工程创建完成后会自动生成框架代码，支持新建Map Reduce任务。本文将以WordCount示例代码为例，为您介绍如何从0开始测试和发布。

## 新建工程 {#section_qs2_wfk_5gb .section}

1.  单击顶部导航栏中的**工程**，选择**新建工程**。
2.  在新建工程对话框中填写工程名和描述等信息。

    在指定MaxCompute工程空间（cdo\_datax）下创建项目（wordcountDemo），工程模板选择**UDFJAVA Project**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633012_zh-CN.png)

3.  单击**确定**。

## 项目开发 {#section_tc2_2kk_5gb .section}

在mapred包下，已存在WordCount的MapReduce示例代码。示例代码的功能是对输入表中的单词做次数统计，将统计结果写入到输出表中，输入输出分别是两个表，详情请参见[MapReduce](../../../../../cn.zh-CN/用户指南/MapReduce/概要/MapReduce概述.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633013_zh-CN.png)

## 项目调试 {#section_asw_clk_5gb .section}

MapReduce项目目前不能在Function Studio中进行debug，需要将代码发布到DataWork开发环境，然后跳转到DataWorks中进行逻辑验证。

**说明：** Function Studio目前仅支持编码和编译打包两个功能。

## 项目发布 {#section_d4g_qlk_5gb .section}

1.  Function Studio编译打包代码发布到DataWorks开发环境。
    1.  单击**发布**，选择**提交资源到DataWorks开发环境**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633014_zh-CN.png)

    2.  在发布对话框中填写各配置项。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633015_zh-CN.png)

        |配置|说明|
        |:-|:-|
        |**目标业务空间**|发布Jar包的目标业务空间，需要和后续建立的DataWorks计算节点在一个空间中，这里是DataX数据同步（cdo\_datax）。|
        |**资源**|您可以自定义资源名称，在后续的计算节点脚本中会被引用。|
        |**如果已经存在强制更新**|名称可以和上次发布的一致，勾选如果已经存在强制更新会被覆盖。|

    3.  单击**确认**，编译发布到DataWorks开发环境。

        信息提示窗口会输出本次编译发布成功还是失败。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633016_zh-CN.png)

2.  在DataWorks中创建MapReduce节点进行测试。
    1.  打开DataWorks同名空间，创建ODPS MR类型节点。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234633017_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733018_zh-CN.png)

    2.  计算节点需要写入以下固定脚本，目前脚本的一些变量还需被手工替换成相关Jar包的信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733019_zh-CN.png)

        **说明：** 请使用在Function Studio中发布的Jar包的信息来替换脚本中的信息，生成最后的代码。

        -   jar -resources：FunctionStudio发布的Jar包名称。
        -   -classpath：Jar包在DataWorks中的路径。
        -   包含main函数的入口类全类名main函数参数空格分隔。
    3.  选择相应业务流程下的**资源**，查看在Function Studio中发布的Jar包信息，来替换脚本中的相关信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733026_zh-CN.png)

        -   Jar包的名称为WordCountDemo\_1.0.0.jar，对应脚本中的-resource。
        -   右键单击Jar包，选择**查看历史版本**，即可查看路径的名称`http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493`，对应脚本中的-classpath。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733030_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733040_zh-CN.png)

        最后的脚本：

        ```
        # 手工将刚才从发布jar包中的信息填入脚本, 脚本完成
        jar -resources WordCountDemo_1.0.0.jar
        -classpath http://schedule@{env}inside.cheetah.alibaba-inc.com/scheduler/res?id=106342493
        com.alibaba.dataworks.mapred.WordCount wordcount_demo_input wordcount_demo_output
        ```

    4.  创建测试表和测试数据。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733042_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234733043_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234833044_zh-CN.png)

        准备好数据后，在开发环境运行脚本。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933045_zh-CN.png)

        至此，WordCount在开发环境的测试全部完成。由于WordCount的计算节点、Jar包、输入输出表都在开发环境，所以需要分别发布至正式环境。

3.  将资源包，数据表，节点分别发布到DataWorks正式环境。
    1.  提交计算节点代码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933046_zh-CN.png)

    2.  进行发布配置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933047_zh-CN.png)

    3.  进入发布页面，勾选提交的Jar包和节点进行发布。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933048_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933049_zh-CN.png)

    4.  发布数据表。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933050_zh-CN.png)

    5.  在运维中心对MapReduce任务进行线上测试。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047234933051_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047235033052_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047235033053_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047235033054_zh-CN.png)

        日志显示成功运行。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64980/155047235033055_zh-CN.png)


由此可见，Function Studio可以编码和编译发布代码到DataWorks节点，DataWorks需要手工操作生成计算节点，在开发环境和生产环境分别运行。

