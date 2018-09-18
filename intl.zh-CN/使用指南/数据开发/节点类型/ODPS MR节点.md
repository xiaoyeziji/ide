# ODPS MR节点 {#concept_nxk_gz3_p2b .concept}

MaxCompute提供MapReduce编程接口，您可以使用MapReduce提供的接口（Java API）编写MapReduce程序处理MaxCompute中的数据，您可以通过创建ODPS MR类型节点的方式在任务调度中使用。

ODPS MR类型节点的编辑和使用方法，请参见MaxCompute文档示例[WordCount示例](https://www.alibabacloud.com/help/doc-detail/27886.htm)。

请将需要用到的资源上传并提交发布后，再建立ODPS MR节点。

## 新建资源实例 {#section_cgz_hbj_p2b .section}

1.  右键单击**数据开发**下的**业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16288/15372463537643_zh-CN.png)

2.  右键单击**资源**，选择**新建资源** \> **jar**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15372463537720_zh-CN.png)

3.  按照命名规则在新建资源对话框输入资源名称，并选择资源类型为jar，同时选择需要上传本机的Jar包。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15372463537721_zh-CN.png)

    **说明：** 

    -   如果此Jar包已经在odps客户端上传过，则需要取消勾选**上传为ODPS资源本次上传，资源会同步上传至ODPS中**，否则上传会报错。
    -   资源名称不一定与上传的文件名一致。
    -   资源名命名规范：1到128个字符，字母、数字、下划线、小数点，大小写不敏感，Jar资源时后缀是.jar，Python资源时后缀为.py。
4.  单击**提交**，将资源提交到调度开发服务器端。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15372463537722_zh-CN.png)

5.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理.md#)。


## 新建ODPS MR节点 {#section_gzm_hbj_p2b .section}

1.  右键单击**数据开发**下的**业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15372463537651_zh-CN.png)

2.  右键单击**数据开发**，选择**新建数据开发节点** \> **ODPS MR**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15372463537723_zh-CN.png)

3.  编辑节点代码。双击新建的ODPS MR节点，进入如下界面：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15372463537724_zh-CN.png)

    编辑节点代码示例：

    ```
    jar -resources base_test.jar -classpath ./base_test.jar com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll
    ```

    代码说明如下：

    -   `-resources base_test.jar`：引用到的Jar资源文件名。
    -   `-classpath`：Jar包路径，可通过对资源文件右键**引用资源**获得该地址。

        **说明：** 双击新建的ODPS MR节点，进入ODPS MR节点界面之后引用jar资源。

    -   `com.taobao.edp.odps.brandnormalize.Word.NormalizeWordAll`：执行过程调用Jar中的主类，需与Jar中的主类名称保持一致。
    一个MR调用多个Jar资源时，classpath写法为`-classpath ./xxxx1.jar,./xxxx2.jar`，即两个路径之间用英文逗号分隔。

4.  节点调度配置。

    单击节点任务编辑在区域右侧的**调度配置**，即可进入节点调度配置页面，详情请参见[调度配置](intl.zh-CN/使用指南/数据开发/调度配置/基本属性.md#)模块。

5.  提交节点任务。

    完成调度配置后，单击左上角的**保存**，提交（提交并解锁）到开发环境。

6.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理.md#)。

7.  在生产环境测试。

    具体操作请参见[周期任务](intl.zh-CN/使用指南/运维中心/任务列表/周期任务.md#)。


