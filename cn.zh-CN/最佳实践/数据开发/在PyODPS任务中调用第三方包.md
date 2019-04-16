# 在PyODPS任务中调用第三方包 {#concept_xrr_sfx_mfb .concept}

本文介绍如何使用DataWorks PyODPS类型任务节点调用单文件第三方包。

1.  首先您需要在**数据开发** \> **业务流程** \> **资源**中右键新建Python类型资源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613917_zh-CN.png)

2.  在弹框中输入新建的Python资源名称，完成创建。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613918_zh-CN.png)

3.  在您新建的Python资源内粘贴您要引用的第三方包的代码，举例如下。

    ```
    
    # import os
    # print os.getcwd()
    # print os.path.abspath('.')
    # print os.path.abspath('..')
    # print os.path.abspath(os.curdir)
    
    def printname():
        print 'test2'
    print 123
    ```

    完成代码后请点击**提交**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613919_zh-CN.png)

4.  在您的业务流程内新建一个PyODPS类型节点。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613921_zh-CN.png)

5.  在节点内输入引用第三方包的代码并测试，举例如下。

    ```
    
    ##@resource_reference{"test2.py"}
    
    import sys 
    import os
    sys.path.append(os.path.dirname(os.path.abspath('test2.py'))) #将资源引入工作空间
    import test2 #引用资源
    test2.printname() #调用方法
    ```

    请注意下图中框内代码，用于引用业务流程中您之前建立的test2.py资源，请不要遗漏。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613923_zh-CN.png)

6.  完成上述步骤后，点击**运行**测试您的代码。您可以在下方的日志中查看到您的运行结果。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23998/154157356613924_zh-CN.png)

