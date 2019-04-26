# OSS对象检查 {#concept_189375 .concept}

当下游任务需要依赖该OSS对象何时传入OSS时，可以使用该节点功能。例如同步OSS数据到DataWorks，需要先检测OSS数据文件已经产生，方可进行OSS同步任务。

检测对象：可以检测当前租户下的OSS对象或非当前租户下的OSS对象。

1.  进入数据开发页面，选择**新建** \> **控制** \> **OSS对象检查**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/163479/155627465345563_zh-CN.png)

2.  填写新建节点对话框中的配置，单击**提交**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/163479/155627465345564_zh-CN.png)

3.  新建成功后，对OSS对象检查节点进行配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/163479/155627465345565_zh-CN.png)

    |序号|配置|说明|
    |--|--|--|
    |1|**OSS对象**|此处可手动填写OSS对象的存储路径，路径支持使用调度参数，详情请参见[参数配置](cn.zh-CN/使用指南/数据开发/调度配置/参数配置.md#)。|
    |2|**超时时间**|在超时时间内，每5秒检测该OSS对象是否存在OSS中，如果超出超时时间还未检测到OSS对象存在，则OSS对象检查任务会失败。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/163479/155627465345571_zh-CN.png)

|
    |3|**选择存储地址**|您可以选择以下两种存储地址：     -   **自己的存储**：检测该租户下的OSS对象。
    -   **别人的存储**：检测非当前租户下的OSS对象。
 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/163479/155627465445573_zh-CN.png)

|

    **说明：** 

    -   任务运行时，将会通过MaxCompute访问身份检查OSS对象，请确认OSS Bucket的权限设置，详情请参见[OSS的STS模式授权](../../../../cn.zh-CN/用户指南/外部表/OSS的STS模式授权.md#)。
    -   在开发/生产环境中，将会通过开发/生产环境访问身份访问身份检查OSS对象，请确认OSS Bucket的权限设置。
4.  在RAM中授权MaxCompute访问OSS的权限。

    MaxCompute结合了阿里云的访问控制服务（RAM）和令牌服务（STS）来解决账号的安全问题。

    -   当MaxCompute和OSS的owner是同一个账号时，可以直接在RAM控制台进行一键授权操作。
    -   当MaxCompute和OSS的owner不是同一账号时，可采用以下方式进行授权：
        1.  首先需要在RAM中授权MaxCompute访问OSS的权限。

            创建如AliyunODPSDefaultRole或AliyunODPSRoleForOtherUser的角色，并设置如下策略内容：

            ``` {#codeblock_bz8_u0p_k8q}
            --当MaxCompute和OSS的Owner不是同一个账号
            {
            "Statement": [
            {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
            "Service": [
            "MaxCompute的Owner云账号id@odps.aliyuncs.com"
            ]
            }
            }
            ],
            "Version": "1"
            }
            ```

        2.  授予角色访问OSS必要的权限AliyunODPSRolePolicy。

            ``` {#codeblock_esn_j5h_7n1}
            {
            "Version": "1",
            "Statement": [
            {
            "Action": [
             "oss:ListBuckets",
             "oss:GetObject",
             "oss:ListObjects",
             "oss:PutObject",
             "oss:DeleteObject",
             "oss:AbortMultipartUpload",
             "oss:ListParts"
            ],
            "Resource": "*",
            "Effect": "Allow"
            }
            ]
            }
            --可自定义其他权限
            ```

        3.  将权限AliyunODPSRolePolicy授权给该角色。
5.  进入运维中心页面，查看运行日志。

    如果出现如下日志信息，则说明未检测到OSS对象产生。

    ``` {#codeblock_1oc_uh6_chf}
    <Error>
    
     <Code>NoSuchKey</Code>
    
     <Message>The specified key does not exist.</Message>
    
     <RequestId></RequestId>
    
     <HostId>oss对象</HostId>
    
     <Key>xc/111.txt</Key>
    
    </Error>
    ```


