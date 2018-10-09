# 如何实现在DataWorks上使用MaxCompute表资源？ {#concept_xct_5r3_kfb .concept}

当前DataWorks不支持直接使用图形界面上传MaxCompute表资源。如果您想在DataWorks上使用MaxCompute表资源，具体操作步骤如下。

1.  首先在DataWorks数据开发页面上传一个File类型资源，资源名称与表名称相同，本例中为`userlog3.txt`，注意不要勾选**上传为ODPS资源**。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22738/153907426913442_zh-CN.png)
2.  完成上传后，通过MaxCompute CLI客户端添加MaxCompute表资源。本例中添加资源命令如下：

    ```
    add table userlog3 -f;
    ```

3.  完成添加后，在DataWorks上使用MaxCompute表资源的时候，直接用上传的资源即可。

