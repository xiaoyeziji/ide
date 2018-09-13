# FAQ {#concept_fkf_ql3_r2b .concept}

-   Q: Do I have to activate the API gateway?

    A: API Gateway provides the API hosting service. If you plan to open your APIs to other users, the API Gateway service must be activated first.

-   Q: Where can I configure the data sources?

    A: To create a data source, select DataWorks \> Data Integration \> Data Sources. After the configuration, Data Service automatically reads the data source information.

-   Q: What is the difference between a wizard-created API and a script-created API?

    A: The script mode provides more powerful functions. For more information, see [Create an API by writing a script](intl.en-US/User Guide/DataService studio/Generate API/Generate API in Script Mode.md#).

-   Q: What is an API group in Data Service? Is it the same as an API group in API Gateway?

    A: An API group contains several APIs in a certain scenario. It is the minimum unit. In a word, the two are equivalent. When you publish an API group from Data Service to API Gateway, the gateway automatically creates an API group with the same name.

-   Q: How can I configure an API group appropriately?

    A: Typically, an API group includes APIs that provide similar functions or solve a specific issue. For example, the API for querying weather by city name and the API for querying weather by latitude and longitude can be put into an API group named "weather query".

-   Q: How many API groups can be created?

    A: An Alibaba Cloud acocunt can create up to 100 API groups.

-   Q: In what situations do I have to enable API response output pagination?

    A: By default, an API outputs up to 500 records. To output more records, enable API response output pagination. When no API request parameters have been set, the API may output a large number of records, and the API response output pagination is automatically enabled.

-   Q: Do APIs created by Data Source support POST requests?

    A: Currently, a created API supports only the GET request.

-   Q: Does Data Service support HTTP?

    A: Currently, Data Service does not support HTTP. HTTP may be supported in later versions.


