# 调用API {#concept_ig4_lk3_r2b .concept}

本文将为您介绍API发布到API网关后，如何进行调用。

API网关提供了API授权、API调用SDK。您可以将API授权给自己或企业内的人员使用，也可将API授权给第三方使用。如果您想调用API，需要进行下述操作。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16413/15368070808815_zh-CN.png)

## 调用API的三要素 {#section_gdj_xk3_r2b .section}

您要调用API，需要以下三个基础条件。

-   API：您即将要调用的API，明确API参数定义。
-   应用APP：作为您调用API时的身份，有AppKey和AppSecret用于验证您的身份。
-   API和APP的权限关系：APP想调用某个API，需要具有该API的权限，这个权限通过授权的功能来建立。

## 操作步骤 {#section_gnx_zk3_r2b .section}

1.  获取API文档。

    根据您获取API的渠道不同，获取方式略有差异。一般分为从数据市场购买的API服务和不需购买，由提供方主动授权两种方式。详情请参见[获取API文档](https://www.alibabacloud.com/help/doc-detail/42740.htm)。

2.  创建应用。

    应用APP是您调用API服务时的身份。每个APP有一组Key和Secret，您可以理解为账号密码。详情请参见[创建应用](https://www.alibabacloud.com/help/doc-detail/29488.htm)。

3.  获取权限。

    授权是指授予APP调用某个API的权限。您的APP需要获取API的授权才能调用该API。

    由于获取API的渠道不同，建立授权的方式也不同。详情请参见[获取授权](https://www.alibabacloud.com/help/doc-detail/29489.htm)。

4.  调用API。

    您可以直接用API网关控制台为您提供的多语言调用示例来测试调用，可以自行编辑HTTP（S）请求来调用API。详情请参见[调用API](https://www.alibabacloud.com/help/doc-detail/29490.htm)。


