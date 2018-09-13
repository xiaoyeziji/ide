# Publish an API {#concept_f13_n33_r2b .concept}

[API Gateway](https://www.alibabacloud.com/product/api-gateway) is an API hosting service that provides full life cycle management covering API release, management, O&amp;M, and sales. It provides you with a simple, fast, low-cost, and low-risk method to implement microservice aggregation, frontend-backend isolation, and system integration, and opens functions and data to partners and developers.

API Gateway provides permission management, traffic control, access control, and metering services. The service makes it easy for you to create, monitor, and secure APIs. Therefore, we recommend that you publish the APIs that have been created and registered in Data Service to API Gateway. Data Service and API Gateway are connected, which allows you to publish APIs to API Gateway easily.

## Publish APIs to API Gateway {#section_q3n_r33_r2b .section}

**Note:** To release an API, you must first activate the API Gateway service.

After activating API Gateway, you can click **Publish**in the Actions column of the API service list to release the API to API Gateway. The system automatically registers the API to API Gateway during the publish process. The system creates a group in API Gateway with the same name as the API group and releases the API to the group.

After the release, you can go to the API Gateway console to view the API information. You can also set the throttling and access control functions in API Gateway.

If your API needs to be called by your application, you must create an application in API Gateway, authorize the API to the application, and encrypt the signature call using the AppKey and AppSecret. For more information, see [API Gateway help documentation](https://www.alibabacloud.com/help/doc-detail/29490.htm). At the same time, the API gateway also provides the SDK in the mainstream programming language, you can quickly integrate your API into your own applications, for more information, please refer to the [SDK download and user's guide](https://www.alibabacloud.com/help/doc-detail/56930.htm).

## Publish APIs to Alibaba Cloud API Marketplace {#section_cww_dj3_r2b .section}

After your APIs from Data Service have been published to API Gateway, you can then publish them to Alibaba Cloud API Marketplace. This is an easy way to achieve financial gains for your company.

Before selling the API to the Ali cloud API market, first of all, it is necessary to enter the Ali cloud market as a service provider.

**Note:** Select to enter API Marketplace as shown in the following figure. Note: only enterprise users are allowed to enter Alibaba Cloud API Marketplace.

## Procedure {#section_xfs_pj3_r2b .section}

1.  Enter the Ali cloud service provider platform.
2.  Click **commodity management** \> **publish the merchandise** and select the access type as the **API service**.
3.  Select the API grouping that you want to list \(one grouping corresponds to one API commodity \).
4.  Configure commodity information and submit audit.

Once your product has been successfully published to Alibaba Cloud API Marketplace, users can purchase it worldwide.

