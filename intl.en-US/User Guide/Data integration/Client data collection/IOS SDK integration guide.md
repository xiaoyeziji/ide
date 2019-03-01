# IOS SDK integration guide {#concept_wbz_jmv_q2b .concept}

This topic describes how to use DTplus data acquisition \(App Usertrack Analytics\) iOS SDK.

App Usertrack Analytics iOS SDK is a type of DTplus data statistics and monitoring service on iOS platform for mobile developers. The SDK enables developers to easily track data in their own apps to monitor daily business data and network performance data, and observe the corresponding data report display on the Alibaba Cloud console. In addition, you can customize the data chart display by setting custom data parsing rules.

You can obtain the usage routine of the Mobile Data Analysis Service by obtaining the source code. For more information, see `man_ios_demo`.

## 2. Install Mobile Analytics iOS SDK {#section_t4j_tmv_q2b .section}

The manually integrated SDK.

Add the packages downloaded to Link Binary with Libraries including:

1.  AlicloudMobileAnalytics.framework
2.  UT.framework
3.  UTDID.framework
4.  CrashReporter.framework
5.  FMDB.framework
6.  Tnet.framework
7.  Reachability.framework

The packages required for loading system:

1.  libz.tbd
2.  Libresolv.tbd
3.  libstdc++.tbd
4.  libsqlite3.tbd
5.  CoreTelephony.framework
6.  SystemConfiguration.framework

## Reference header file {#section_k2z_vnv_q2b .section}

```
#import <AlicloudMobileAnalitics/ALBBMAN.h>
```

**Note:** `Add the property "-ObjC" in Targets -&gt; Build Settings -&gt; Linking -&gt; Other Linker Flags. Otherwise the data cannot be reported.`

## 3. Obtain Mobile Analytics {#section_ecz_p4v_q2b .section}

Before using the App Usertrack Mobile Analytics iOS SDK for data statistics and monitoring, you must obtain the Mobile Analytics before version and channel configuration.

```
// Obtain MAN service
ALBBMANAnalytics *man = [ALBBMANAnalytics getInstance];
// Enable the debug log. We recommend that you disable it for an online version.
// [man turnOnDebug];
// Initialize MAN
[man initWithAppKey:testAppKey secretKey:testAppSecret];
// appVersion is retrieved from CFBundleShortVersionString field of Info.list by default. If the appVersion is not specified, call setAppversion to set it
// If the appVersion is not set in either of the two locations, it is "-"
[man setAppVersion:@"2.3.1"];
// Set the channel (to mark the distribution channel name of this App). If not interested, you may skip this step so that this API is not called. Channel setting affects the report presentation of the Channel Analytics topic in the console.
[man setChannel:@"50"];
```

## Business data statistics {#section_bnt_wfb_r2b .section}

The data statistics accuracy depends on the routine life track of the app being monitored. For example, the number of application startups depends on the reporting policy triggered when the user exits the application normally.

**Login/registered member**

Login member

API:

```
- (void)updateUserAccount:(NSString *)pNick userid:(NSString *)pUserId;
```

Feature: Obtain the logged-on member, and then add a "logged-on member" field to each log.

Call: No

Call time: Called on login.

Note: The UV metric for members logged on to Alibaba Cloud platform that depends on this API.

**Registered member**

API:

```
- (void)userRegister:(NSString *)pUsernick;
```

Features: Generates an event log about the registered member.

Call: No

Call time: Called upon registration.

Note: The User Volume \(UV\) metric for registered members on Alibaba Cloud platform depends on this API.

Sample code

```
ALBBMANAnalytics *man = [ALBBMANAnalytics getInstance];
[man userRegister:@"userNick"];
[man updateUserAccount:@"userNick" userid:@"userId"];
```

**Page event tracking**

The ALBMANPageHitHelper can process ViewController level page event tracking that is available for the page event tracking assistance class, and can automatically complete the Page name.\(By default, the obtained viewcontroller name removes the suffix`Controller`.\) It can also automatically gather statistics on the Source Page and page Stay Time. You can use the following setting to make the page event tracking scheme more flexible:

`ALBBMANPageHitBuilder`.

**Note:** The page management base class for page event tracking points, see section 4.3 for details.

If the app does not have any page tracking point, the parameter

`Active users`

cannot be counted normally.

Page entry

API:

```
- (void)pageAppear:(UIViewController *)pViewController;
```

Features: Records certain status information when you enter the page, but does not send the log. Must be used with pageDisappear.

Parameters: The UIViewController or its subclass pointer.

Call: Yes. The API must be called when page tracking is required.

Call time: The viewDidAppear parameter.

Note: This parameter must be used with pageDisappear.

Page left

API:

```
- (void)pageDisAppear:(UIViewController *)pViewController;
```

Set page extension parameters

API:

```
- (void)updatePageProperties:(UIViewController *)pViewController properties:(NSDictionary *)pProperties;
```

Feature: Set page extension parameters.-

Call: No

Call time: Before calling pageDisAppear.

Sample code

```
// Enter the page
[[ALBBMANPageHitHelper getInstance] pageAppear:self];
// Set page event extension parameters
NSDictionary *properties = [NSDictionary dictionaryWithObject:@"pageValue" forKey:@"pageKey"];
[[Albbmanpagehithelper getinstance] updatepageproperties: Self properties: properties];
// Leave the page
[[ALBBMANPageHitHelper getInstance] pageDisAppear:self];
```

**Page events**

The ALBBMANPageHitBuilder class is used to generate page event logs. Section 4.2 describes page tracking can be completed by pageAppear and pageDisAppear because this method is only designed for UIViewController page level scenario. For example, if you need to make a UIView page, you can use ALBBMANPageHitBuilder for tracking page events. That is, you acquire related page event information, such as the refer page and the Time on Page. Construct a log map of page events using ALBBMANPageBuilder, which is finally sent and uploaded using a send API of an ALBBMANTracker tracking instance.

ALBBMANTracker is a tool for reporting tracking data. Event classes similar to ALBBMANPageHitBuilder describes all report events using ALBBMANTracker later. You can obtain the ALBBMANTracker as follows:

```
// Get default scanner instance albbmantracker * tracker = [[albbmananalytics getinstance] getdefaulttracker];
```

Tracking data is reported using ALBBMANTracker. You can set/delete the global field of the reported data. The set global field can be viewed in all reported logs as follows:

```
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
// Set the global field
[tracker setGlobalProperty:@"globalKey1" value:@"globalValue1"];
// Delete the global field
[tracker removeGlobalProperty:@"globalKey1"];
```

Set the page name

API:

```
- (void)setPageName:(NSString *)pPageName;
```

Features: The set page name.

Parameters: The page name, where the pPageName cannot be empty.

Call: Yes. This method must be called after the ALBBMANPageHitBuilder instance is created.

Call time: The setPageName must be called after creating the ALBBMANPageHitBuilder. Otherwise, build will return nil.

Note: The basis of page events where the page name must be set.

Setup page refer

API:

```
- (void)setReferPage:(NSString *)pReferPageName;
```

Features: Set the refer page which is the source page of the current page.

Parameters: The refer page name. The refer can be null.

Call: No

Set time on Page:

API:

```
- (void)setDurationOnPage:(long long)durationTimeOnPage;
```

Features: Records the duration from page display to page exit.

Entry: The page stop time.

Call: no

Call time: You need to record the current page stay time.

// Sets page event extension parameters.

API:

```
- (void)setProperty:(NSString *)pKey value:(NSString *)pValue;
```

Feature: Add an extension parameter to a single log.

Input parameter: Neither the key nor the value can be nil. The key cannot be PAGE, EVENTID, ARG1, ARG2, ARG3, ARGS, or build returns nil.

Call : No

Call condition: When you need to add extension parameters to the ALBBMANPageHitBuilder instance.

Assemble a single log map.

API:

```
- (NSDictionary *)build;
```

Function: Build returns the inserted parameters as a map.

Incoming parameters: None

Call: No

Call condition: Create an ALBBMANPageHitBuilder or ALBBMANCustomHitBuilder instance. After inserting business fields, call the build business field to generate a log map, which is sent with the send API of the ALBBMANTracker tracking instance.

Note: The log map returned by the build function must be uploaded with the send API of the tracking reporting tool ALBBMANTracker to complete data hitting.

Sample code

```

ALBBMANPageHitBuilder *pageHitBuilder = [[ALBBMANPageHitBuilder alloc] init];
// Set the refer page
[pageHitBuilder setReferPage:@"pageRefer"];
// Set the page name
[pageHitBuilder setPageName:@"pageName"];
// Set the time-on-page
[pageHitBuilder setDurationOnPage:100];
// Set page event extension parameters
[pageHitBuilder setProperty:@"pagePropertyKey1" value:@"pagePropertyValue1"];
[Pagehitbuilder setproperty: @ "pagepropertykey2" value: @ "pagepropertyvalue2"];
ALBBMANTracker tracker = ALBBMANAnalytics getInstance getDefaultTracker;
// Build and send the log
[tracker send:[pageHitBuilder build]];
```

## Performance data statistics {#section_vgw_ljb_r2b .section}

The app performance affects user experience, which is easily overlooked by developers. Mobile Analytics provides multiple performance monitoring APIs for you to monitor the apps status comprehensively.

**Network performance statistics**

Mobile Analytics is encapsulated against the network performance statistics for traditional apps, making it easy for "one-click tracking" and obtain the key technical metrics at the network level including:

-   Overall request response time
-   Network exception statistics

Interfaces and statistics

The process of a client normal network request can be divided into the following stages: TCP connection, send request, start receiving response data, and complete receiving data . The performance metrics for APIs in different stages: TCP connection time, the first byte response time, and overall response time.

```

- (instancetype)initWithHost:(NSString *)host method:(NSString *)method;
/*
You can set your own properties excluding "/Host/Method/EVENTID/PAGE/ARG1/ARG2/ARG3/ARGS/COMPRESS"
*/
- (void)setProperty:(NSString *)pKey value:(NSString *)pValue;
- (void)setproperties:(NSMutableDictionary *)properties;
/*
Network request hitting starts
*/
- (void)requestStart;
/*
Mark the successful connection by hitting
*/
- (void)connectFinished;
/*
Mark the arrival of the first byte by hitting
*/
- (void)requestFirstBytes;
/*
hitting ends normally
*/
(void)requestEndWithBytes:(long)loadBytes;
/*
Report any errors
*/
(void)requestEndWithError:(ALBBMANNetworkError *)error;
/*
Integrate the hitting logs
*/
(NSDictionary *)build;
```

Host/Method is required for initializing ALBBMANNetworkHitBuilder. The data cannot be reported if nil is inputted. The following is an instance of NSURLConnection:

```

#import 
/**
* @brief 
Synchronous request for network performance tracking
*/
- (void)syncNetworkHit {
NSURL *URL = [NSURL URLWithString:@"http://www.taobao.com/"];
NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:URL
cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
timeoutInterval:30];
// Because the NSURLConnection synchronous API encapsulates the process of TCP connection and first byte arrival, you cannot count the TCP connection time and first byte response time on the synchronous API
ALBBMANNetworkHitBuilder *builder = [[ALBBMANNetworkHitBuilder alloc] initWithHost:URL.host method:[request HTTPMethod]];
// Request start hitting
[builder requestStart];
NSHTTPURLResponse *response;
NSError *error;
NSData *data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];
if (error) {
// For information on custom network exceptions, see section 5.1.2
ALBBMANNetworkError *networkError = [[ALBBMANNetworkError alloc] initWithErrorCode:1001];
// Custom additional information of network exceptions
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
}
if (response.statusCode &gt;= 400 &amp;&amp;response.statusCode &lt; 600) {
// For information on default network exceptions, see section 5.1.2
ALBBMANNetworkError *networkError;
if (response.statusCode &lt; 500) {
networkError = [ALBBMANNetworkError ErrorWithHttpException4];
} else {
networkError = [ALBBMANNetworkError ErrorWithHttpException5];
}
// Additional information of default network exceptions
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
}
}else {
// Request end hitting. The "bytes" is the size of the data downloaded
[builder requestEndWithBytes:data.length];
}
// Build and send the log
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[tracker send:[builder build]];
}
// The asynchronous API is the same with the synchronous API but the former enables statistics of more first bytes
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response {
// First byte response tracking
[_builder requestFirstBytes];
}
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data {
[_mutableData appendData:data];
}
- (void)connectionDidFinishLoading:(NSURLConnection *)connection {
// Request end hitting
[_builder requestEndWithBytes:_mutableData.length];
// Build and send the log
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[tracker send:[_builder build]];
}
- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error {
ALBBMANNetworkError *networkError = [[ALBBMANNetworkError alloc] initWithErrorCode:[NSString stringWithFormat:@"%ld", (long)error.code]];
[_builder requestEndWithError:networkError];
// Build and send the log
ALBBMANTracker tracker = ALBBMANAnalytics getInstance getDefaultTracker;
[tracker send:[_builder build]];
}
//Due to the encapsulation by NSURLConnection, the TCP connection process is invisible and the connection statistics cannot be achieved
//Time. In case of a self-implemented Socket, you can call the following codes to count the TCP connection time when the TCP connection is successful
[builder connectFinished];
```

**Note:** The current Method field value only supports POST and GET which is case-insensitive.

Exception statistics

If an exception occurs during tracking, and interrupts the network flow request, you must handle this problem, or it can cause hitting data disorder. To solve the problem, you can mark the exception request by selecting the default network exception type, or the custom network exception type after the exception is captured:

Default network exception types

|Exception type|Corresponding interface|Description|
|--------------|-----------------------|-----------|
|400&lt;=HttpResponseCode&lt;500|ErrorWithHttpException4|HTTP status code 4xx Error|
|500&lt;=HttpResponseCode|ErrorWithHttpException5|HTTP status code 5xx Error|
|I/O error|ErrorWithIOInterrupted|IO error requested|
|Timeout error|Errorwithsockettimeout|Connection/request timeout error|

See the preceding case.

Usage:

```

ALBBMANNetworkError *networkError = [ALBBMANNetworkError ErrorWithHttpException4];
// Additional information of network exceptions
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
```

Custom network exception types:

If the default network request exception type cannot meet your requirements, you can customize the network request exception type. The customized network request exception encoding type must be in the range of 1001 <= YouErrorCode <= 1010. It is displayed in the form of the error code \(10xx\) without front-end configuration. If you have configured related error code information in the frontend, the information you configured is displayed.

|Exception type|Corresponding interface|Description|
|--------------|-----------------------|-----------|
|Custom error|initWithErrorCode|Customizable when the default network error is unavailable.|

Usage:

```

Albbmannetworkerror * networkerror = [[albbmannetworkerror alloc] initwitherrorcode: 1001];
// Custom additional information of network exceptions
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
```

## Custom performance event tracking point {#section_spw_tkb_r2b .section}

Custom performance event tracking can be used for real-time monitoring of customized performance events, such as time consumed by cache reading and writing by RPC of private network protocol, and by client algorithm.

Custom performance events include the following parts:

1. Event name \(event\_label\) which must be a combination of letters, numbers, and underscores \(\_\) \[Required\].

2. Event duration from initiation to completion \[Required\].

3. The event properties \[Optional\].

**Note:** Custom performance events are used to monitor the time overhead of user events. Ensure the correct parameter is passed by using the following code examples as reference.

Example:

```

ALBBMANCustomPerformanceHitBuilder *customPerfBuilder = [[ALBBMANCustomPerformanceHitBuilder alloc] init:@"HomeActivityInit"];
// Method 1 of recording event time. Custom performance event starts
[customPerfBuilder hitStart];
...
// Custom performance event ends
[customPerfBuilder hitEnd];
// Set the duration of custom performance event. Method 2
// [customPerfBuilder setDurationIntervalInMillis:1234];
// Set the extension parameters
[customPerfBuilder setProperty:@"Page" value:@"Home"];
// Build and send the log
ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[traker send:[customPerfBuilder build]];
```

## Custom events {#section_oj1_ykb_r2b .section}

The custom event tracking can be used to meet users customization requirement.

**Set the label for custom events**

API:

```
- (void)setEventLabel:(NSString *)pEventId;
```

Features: Differentiates the custom event tags. The same custom events will have the same pEventId.

Parameters: The custom event tag is equivalent to the business ID of a custom event and cannot be null.

Call: No

Call condition: The setEventLabel must be called after creating the ALBBMANCustomHitBuilder. Otherwise, build will return nil.

API:

**5.2 Set the page name for custom events**

API:

```
- (void)setEventPage:(NSString *)pPageName;
```

Features: Sets the page on which the custom event occurs.

Parameters: The page name of the custom event can be null. In this case, the default page name in the log is "UT".

Call: No

Call condition: When a custom event occurs, the page must be specified. The page name is "UT" by default.

**The residence time for custom events**

API:

```
- (void)setDurationOnEvent:(long long)durationOnEvent;
```

Features: Set the residence time so it is similar to the time-on-page in ALBBMANPageHitBuilder for custom events.

Parameters: Customize event stay time.

Call:No

Call condition: When the Time to Stay for custom events need to be logged.

**// Set custom event extension parameters**

API:

```
- (void)setDurationOnEvent:(long long)durationOnEvent;
```

Feature: Add an extension parameter to a single log.

Parameter: Neither the key nor the value can be nil. The key cannot be PAGE, EVENTID, ARG1, ARG2, ARG3, or ARGS, or build will return nil.

Call: No

Call condition: When the call time is required to add extension parameters to the ALBBMANCustomHitBuilder instance.

**Sample code**

```

ALBBMANPageHitBuilder *pageHitBuilder = [[ALBBMANPageHitBuilder alloc] init];
// Set the tag for custom event
[customBuilder setEventLabel:@"test_event_label"];
// Set the page name for custom events
[customBuilder setEventPage:@"test_Page"];
// Set the residence time for custom events
[customBuilder setDurationOnEvent:12345];
// Set custom event extension parameters
[customBuilder setProperty:@"ckey0" value:@"value0"];
[customBuilder setProperty:@"ckey1" value:@"value1"];
[customBuilder setProperty:@"ckey2" value:@"value2"];
ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
// Build and send the log
NSDictionary *dic = [customBuilder build];
[traker send:dic];
```

You can go to Customer Event &gt; Detailed Data &gt; Parameter Analytics in the console to view the customer event extension parameter, but you need to add the event ID for displaying under Management Setting&gt; Custom Event Management beforehand. To monitor custom events in real time, see section 4.3 "Custom performance event" for details.

## 6. How to verify if data is reported successfully in real time {#section_vrf_vlb_r2b .section}

Follow these steps:

1.  Open the debug log in the SDK:

    ```
    
    ALBBMANAnalytics man = ALBBMANAnalytics getInstance;
    [man turnOnDebug];
    ```

2.  If you see the following logs in the console output window:

    ```
    
    _uploadThroughTnetWithStream:logs:] line:502 
    Upload successful :{
    allLength = 25;
    dataLen = 17;
    errorcode = 0;
    flag = 40;
    recvData = "{\"config\":{}}";
    type = 2;
    version = 2;
    }
    The total number of uploaded logs:1
    ```


It indicates that the logs have been uploaded successfully. You can see the related data in the MaxCompute table about 5 minutes later.

## 7. Data acquisition on H5 page {#section_kdr_nmb_r2b .section}

Without separate SDK, the H5 page acquisition data can be uploaded by notifying native through the JSBridge, and then the corresponding method of MAN is called to report data.

**Sample code**

Custom event reporting on H5 page:

JavaScript code:

```

// Initiate a request by iframe, and then perform the capture communication on the native end.
var iframe = document.createElement("IFRAME");
// Determine whether it is a normal request or the H5 communication by custom scheme.
// You can add parameters to the URL, and then perform the parameter parsing on the native end.
iframe.setAttribute("src", "jsbridge://custom");
document.documentElement.appendChild(iframe);
iframe.parentNode.removeChild(iframe);
iframe = null;
```

Objective-c

```

- (BOOL) webView:(UIWebView *)webView
shouldStartLoadWithRequest:(NSURLRequest *)request
navigationType:(UIWebViewNavigationType)navigationType
{
Nsurl * url = [request URL];
NSString *scheme = [url scheme];
// js communication
if ( [@"jsbridge" isEqualToString:scheme] ) {
if ( [@"custom" isEqualToString:url.host] ) {
ALBBMANPageHitBuilder *pageHitBuilder = [[ALBBMANPageHitBuilder alloc] init];
// Set the tag for custom event
[customBuilder setEventLabel:@"test_event_label"];
// Set the page name for custom events
[customBuilder setEventPage:@"test_Page"];
// Set the residence time for custom events
[customBuilder setDurationOnEvent:12345];
// Set custom event extension parameters
[customBuilder setProperty:@"ckey0" value:@"value0"];
[customBuilder setProperty:@"ckey1" value:@"value1"];
[customBuilder setProperty:@"ckey2" value:@"value2"];
ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
// Build and send the log
NSDictionary *dic = [customBuilder build];
[traker send:dic];
}
return NO;
}
return YES;
}
```

