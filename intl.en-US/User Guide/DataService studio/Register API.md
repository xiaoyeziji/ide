# Register API {#concept_r1l_kf3_r2b .concept}

This section describes how to register an API.

You can register currently available APIs in Data Service. These APIs can be managed and published to API Gateway together with APIs created based on data tables. Currently, you can only register RESTful APIs supporting GET, POST, PUT, and DELETE requests and content types form,JSON,and XML.

## Configure the API basic information {#section_o12_1g3_r2b .section}

1.  You can go to the registration API page by selecting the **Register API** in the API Service list.
2.  Configure the API basic information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16409/15368069538806_en-US.png)

    Parameters:

    -   Protocol: Only HTTP is supported.
    -   Backbround Service Host: Enter the host of the API. The host must start with http:// or https://, and cannot contain the path.
    -   Backbround Service Path: Enter the path of the API. Put parameter names in brackets, for example, /user/\[userid\].

        If a parameter is defined in the path, the system automatically adds the parameter in the path to the request parameter list in the second step of the API registration wizard.

    -   API path: The alias of the background service path. It allows an API for the background service host and path to register as multiple APIs.

        Parameters defined in Backbround Service Path must also be defined in brackets in API Path.

    -   Request method: The options include GET, POST, PUT, and DELETE. Different request methods correspond to different subsequent configuration items.
    -   Return Type: Select JSON or XML.
3.  After providing the API basic information, click **Next** to go to the API parameter configuration page.

## Configure API parameters { .section}

After configuring the basic API information, you can configure the API parameters. including the request parameters, response example, and error code of the API.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16409/15368069538807_en-US.png)

-   Request Parameters:
    -   Parameter location: The options include Path, Header, Query, and Body. Different request methods support different optional parameter locations. You can select the options as required.
    -   Constant parameters: The parameters that have the fixed values and are invisible to callers. The constant parameters do not need to be input during API calling. However, the background service always receives the defined constant parameters and their values. Constant parameters are applicable if you want to fix the value of a parameter or hide the parameters to the callers.
-   Request Body is required only when the request mode is POST or PUT. You can enter the desc The content types of the request body include JSON and XML.

    **Note:** If the request body is defined in the request body definition and the body location parameter is defined in the request parameter definition, the body location parameter is invalid. The request body is applied.

-   You can enter a normal example or an exception example for API callers to refer to when writing the return parse code.
-   Enter the common errors and solutions in API calling. This enables API callers to troubleshoot and solve these errors.

    **Note:** To ensure that the API is easily used by the callers, provide complete API parameter information if possible, especially the parameter sample values, default values, and response examples.


## API Testing {#section_d5j_nb3_r2b .section}

After completing configuration of API parameters, you can start the API test.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15368069538797_en-US.png)

Set parameters and click **Start Test** to send the API request online. The API request details and response are displayed on the right. If the test fails, read the error message carefully and make the appropriate adjustments to test your API again.

You need to note the settings for the normal return example during the configuration process. When testing an API, the system automatically generates exception examples and error codes. However, normal response examples are not automatically generated. After the test succeeds, you need to click **Save as Normal Response Sample** to save the current test result as the normal response sample. If sensitive data is included in the response, you can manually edit it.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15368069538799_en-US.png)

**Note:** 

-   Normal response examples provide an important reference value for the API callers. Specify an example if possible.
-   The API calling delay is the delay of the current API request, which is used to evaluate the API performance. If the latency is too high, you may consider optimizing your database.

After completing the API test, click **Finish**. The data API is successfully created.

