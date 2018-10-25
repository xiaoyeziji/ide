# ODPS MR节点 {#concept_nxk_gz3_p2b .concept}

MaxCompute提供MapReduce编程接口，您可以使用MapReduce提供的接口（Java API）编写MapReduce程序处理MaxCompute中的数据，您可以通过创建ODPS MR类型节点的方式在任务调度中使用。

ODPS MR类型节点的编辑和使用方法，请参见MaxCompute文档示例[WordCount示例](https://www.alibabacloud.com/help/doc-detail/27886.htm)。

请将需要用到的资源上传并提交发布后，再建立ODPS MR节点。

## 新建资源实例 {#section_cgz_hbj_p2b .section}

1.  右键单击**数据开发**下的**业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16288/15404466067643_zh-CN.png)

2.  右键单击**资源**，选择**新建资源** \> **jar**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466067720_zh-CN.png)

3.  按照命名规则在新建资源对话框输入资源名称，并选择资源类型为jar，同时选择需要上传本机的Jar包（可以通过 Eclipse 的 Export 功能打包，也可以通过 ant 或其他工具生成）。本例中使用的示例[mapreduce\_example.jar](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/57148/cn_zh/1534313773021/mapreduce-examples.jar)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466067721_zh-CN.png)

    **说明：** 

    -   如果此Jar包已经在odps客户端上传过，则需要取消勾选**上传为ODPS资源本次上传，资源会同步上传至ODPS中**，否则上传会报错。
    -   资源名称不一定与上传的文件名一致。
    -   资源名命名规范：1到128个字符，字母、数字、下划线、小数点，大小写不敏感，Jar资源时后缀是.jar，Python资源时后缀为.py。
4.  单击**提交**，将资源提交到调度开发服务器端。
5.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理/任务发布.md#)。


## 新建ODPS MR节点 {#section_gzm_hbj_p2b .section}

1.  右键单击**数据开发**下的**业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15404466067651_zh-CN.png)

2.  右键单击**数据开发**，选择**新建数据开发节点** \> **ODPS MR**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466067723_zh-CN.png)

3.  编辑节点代码。双击新建的ODPS MR节点，进入如下界面：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16294/15404466077724_zh-CN.png)

    编辑节点代码示例：

    ```
    
    --创建输入表
    CREATE TABLE if not exists jingyan_wc_in (key STRING, value STRING);
    --创建输出表
    CREATE TABLE if not exists jingyan_wc_out (key STRING, cnt BIGINT);
        ---创建系统dual
        drop table if exists dual;
        create table dual(id bigint); --如project中不存在此伪表，则需创建并初始化数据
        ---向系统伪表初始化数据
        insert overwrite table dual select count(*)from dual;
        ---向输入表 wc_in 插入示例数据
        insert overwrite table jingyan_wc_in select * from (
        select 'project','val_pro' from dual 
        union all 
        select 'problem','val_pro' from dual
        union all 
        select 'package','val_a' from dual
        union all 
        select 'pad','val_a' from dual
          ) b;
    -- 引用刚刚上传的Jar包资源，可在资源管理栏中找到该资源，右键引用资源。
    --@resource_reference{"mapreduce-examples.jar"}
    jar -resources mapreduce-examples.jar -classpath ./mapreduce-examples.jar com.aliyun.odps.mapred.open.example.WordCount jingyan_wc_in jingyan_wc_out
    ```

    代码说明如下：

    -   `--@resource_reference{"mapreduce-examples.jar"}` 通过右键资源，选择**引用**自动产生。
    -   `-resources base_test.jar`：引用到的Jar资源文件名。
    -   `-classpath`：Jar包路径，由于已经引用了资源，此处路径统一为./下的Jar包即可。
    -   `com.aliyun.odps.mapred.open.example.WordCount`：执行过程调用Jar中的主类，需与Jar中的主类名称保持一致。
    -   jingyan\_wc\_in：MR的输入表名称，已在上述代码中提前建立好。
    -   jingyan\_wc\_out：MR的输出表名称，已在上述代码中提前建立好。
    一个MR调用多个Jar资源时，classpath写法为`-classpath ./xxxx1.jar,./xxxx2.jar`，即两个路径之间用英文逗号分隔。

4.  节点调度配置。

    单击节点任务编辑在区域右侧的**调度配置**，即可进入节点调度配置页面，详情请参见[调度配置](intl.zh-CN/使用指南/数据开发/调度配置/基本属性.md#)模块。

    **说明：** 您也可以直接点击**运行**按钮测试运行。

5.  提交节点任务。

    完成调度配置后，单击左上角的**保存**，提交（提交并解锁）到开发环境。

6.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理/任务发布.md#)。

7.  在生产环境测试。

    具体操作请参见[周期任务](intl.zh-CN/使用指南/运维中心/任务列表/周期任务.md#)。


