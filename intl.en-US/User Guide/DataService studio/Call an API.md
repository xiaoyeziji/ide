# Call an API {#concept_ig4_lk3_r2b .concept}

This section describes how to call an API after this API is released on API Gateway.

API Gateway provides API authorization and the SDK for calling APIs. You can authorize yourself, your associates, or third parties to use APIs. If you want to call an API, perform the following operations.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16413/15476195808815_en-US.png)

## Three elements for calling an API {#section_gdj_xk3_r2b .section}

To call an API, you need the following three elements:

-   API: the API that you are about to call, which is clearly defined by the API parameters.
-   app: Identity that you use to call the API. The AppKey and AppSecret are provided to authenticate your identity.
-   Permission relationship between the API and app: When an app needs to call an API, the app must have the permission of this API. This permission is granted through authorization.

## Procedure {#section_gnx_zk3_r2b .section}

1.  Get the API documentation

    The acquisition method varies according to the channel that you use to obtain the API. It is generally divided into API services purchased from the data market and not required to purchase, two ways are actively authorized by the provider. For more information, see [get API documentation](https://www.alibabacloud.com/help/doc-detail/42740.htm).

2.  Create a project

    The app is the identity that you use to call an API. Each app has a set of AppKey and AppSecret, which are equivalent to an account and a password. For more information, see [creating an application](https://www.alibabacloud.com/help/doc-detail/29488.htm).

3.  Get the permission

    Authorization means granting an app the permission to call an API. Your app must be authorized first to call an API.

    The authorization method varies according to the channel that you use to obtain the API. For more information, see [obtaining authorization](https://www.alibabacloud.com/help/doc-detail/29489.htm).

4.  Call API

    You can directly use the multi-language call sample provided by API Gateway Console, or use a self-compiled HTTP or HTTPS request to call the API. For more information, see [calling the API](https://www.alibabacloud.com/help/doc-detail/29490.htm).


