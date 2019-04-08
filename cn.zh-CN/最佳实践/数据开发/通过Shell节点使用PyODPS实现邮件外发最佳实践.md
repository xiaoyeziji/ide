# 通过Shell节点使用PyODPS实现邮件外发最佳实践 {#concept_dzb_p3z_ghb .concept}

本文将为您介绍如何通过Shell节点结合自定义资源组的方法来实现邮件外发的需求。

## 背景信息 {#section_vx5_y3z_ghb .section}

DataWorks PyODPS节点和Python脚本并不相同， PyODPS节点主要用于和MaxCompute交互进行数据分析处理，节点中不支持使用某些第三方包或外发邮件，将无法满足从MaxCompute拉取数据进行邮件外发的场景需求，您可通过Shell任务+自定义资源组的方法实现。

## 操作步骤 {#section_mk4_pjz_ghb .section}

1.  准备自定义资源组。

    登录DataWorks控制台，进入调度资源列表页面添加自定义资源组。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931643311_zh-CN.png)

2.  安装自定义资源组。

    对资源组进行服务器初始化安装，详情请参见[新增任务资源](../../../../../cn.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931643317_zh-CN.png)

3.  准备邮件外发Python脚本。

    自定义资源组安装完成后，需要准备邮件外发Python脚本的环境。邮件外发涉及的Python模块有ODPS、smtplib、email，python2.7版本中默认有smtplib和email，自定义资源组中仅需执行`pip install pyodps`安装PyODPS。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931643321_zh-CN.png)

4.  PyODPS安装完成后，即可进行Python脚本开发操作。如果有已经准备好的Python脚本，可以复制到自定义资源组下进行脚本测试。

    以从MaxCompute表中读取数据，然后进行邮件外发为例，代码如下。

    ```
    import smtplib
    
    from email.mime.text import MIMEText
    
    from odps import ODPS
    
    mail_host = '<yourHost>' #邮箱服务地址
    
    mail_username = '<yourUserName>' #登录用户名
    
    mail_password = '<yourPassWord>'  #登录用户密码
    
    mail_sender = '<senderAddress>' #发件人邮箱地址
    
    mail_receivers = ['<receiverAddress>']  #收件人邮箱地址
    
    mail_content=""        #邮件内容
    
    o=ODPS('access_key','access_secretkey','default_project_name',endpoint='maxcompute_service_endpoint')
    
    with o.execute_sql('query_sql').open_reader() as reader:
    
               for record in reader:
    
                       mail_content+=str(record['column_name'])+' '+record['column_name']+'\n'
    
    message = MIMEText(mail_content,'plain','utf-8')
    
    message['Subject'] = 'mail test'
    
    message['From'] = mail_sender
    
    message['To'] = mail_receivers[0]
    
    try:
    
               smtpObj = smtplib.SMTP_SSL(mail_host+':465')
    
               smtpObj.login(mail_username,mail_password)
    
               smtpObj.sendmail(
    
                   mail_sender,mail_receivers,message.as_string())
    
               smtpObj.quit()
    
               print('mail send success')
    
    except smtplib.SMTPException as e:
    
               print('mail send error',e)
    ```

5.  脚本测试完成后，登录DataWorks控制台，进入数据开发页面[创建Shell节点](../../../../../cn.zh-CN/使用指南/数据开发/节点类型/SHELL节点.md#)。

    下述示例创建的Shell节点名称为shell\_test，/usr/bin/env python为执行`which python`获取的Python可执行文件存放的路径，/root/send\_mail.py为测试Python脚本在自定义资源组上的全路径，此脚本需要对admin用户开放可读可执行的权限。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931743430_zh-CN.png)

6.  关联上游节点。

    如果Shell节点需要和数据同步节点或SQL节点进行关联，可以单击页面右侧的**调度配置**进行调度依赖配置，并需要配置上游节点和Shell节点的调度时间，完成后提交节点即可。示例中依赖的上游SQL节点为sql\_test，sql\_test的节点输出为muxing\_test\_2018.sql\_test。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931743435_zh-CN.png)

    **说明：** 此处以SQL节点作为依赖上游节点示例， 您可根据自身需求选择其他节点类型。

7.  节点提交后，进入**运维中心** \> **周期任务**页面，修改Shell节点的调度资源组为自定义资源组。Shell节点提交后调度资源组会是默认资源组，需要修改为自定义资源组。如下图所示，修改shell\_test节点的调度资源组为shell\_test自定义资源组。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931743441_zh-CN.png)

8.  测试关联任务。

    进入**运维中心** \> **周期任务**页面进行业务流程测试。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/150965/155468931743442_zh-CN.png)


至此，您可以通过Shell任务和自定义资源组实现邮件外发，如果需要将MaxCompute中的数据以csv等格式的附件发送，可以在Python脚本中使用相关模块，实现更丰富的功能。

