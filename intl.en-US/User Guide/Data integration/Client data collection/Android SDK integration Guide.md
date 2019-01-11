# Android SDK integration Guide {#concept_onv_rdb_r2b .concept}

This document describes how to use DTplus data acquisition \(app Usertrack\) Android SDK.

App Usertrack Android SDK is a kind of DTplus data statistics and monitoring service on Android platform for mobile developers. The SDK enables developers to easily track data in their own apps to monitor daily business data and network performance data, and observe the corresponding data report display in the DTplus console. In addition, you can customize the data chart display by setting custom data parsing rules.

You can obtain an example of the use of the mobile data analytics service by getting the engineering source code, for more information, see the maid project.

## 2. Install Mobile Analytics Android SDK {#section_mnc_ct1_r2b .section}

**Manually integrated SDK**

SDK directory structure

```
OneSDK Problems
|-- libs
|-- |-- jniLibs
| | |-- armeabi
| | | |-- libMotu.so                  
| | | |-- libtnet-3.1.1.so               
| | |-- armeabi-v7a
| | | |-- libMotu.so
| | | |-- libtnet-3.1.1.so
| | |-- x86
| | | |-- libMotu.so
| | | |-- libtnet-3.1.1.so  
| |-- alicloud-android-sdk-man-2.0.0.aar - Main feature package of Mobile Analytics
| |-- fastjson-1.1.41.sec01.jar          
| |-- tlog_adapter-1.1.0.1.jar
| |-- tnet-jni-3.1.1.jar
| -- Ut-analytics-6.5.1.1.aar
| |-- utdid4all-1.5.0.jar
```

SDK integration

-   Manually copy jniLibsfrom the directory to the directory src/main.
-   Add the following configuration item in the build.gradle configuration.

    ```
    android {
      ...
      defaultConfig {
        ...
        ndk {
          moduleName "jniLibs"
          abiFilters "armeabi", "armeabi-v7a", "x86"
        }
      }
    }
    ...
    repositories {
      flatDir {
          dirs 'libs'
      }
    }
    ...
    dependencies {
       ...
      compile 'com.taobao.android:ut-analytics:6.5.1.1@aar'
      compile 'com.aliyun.android:alicloud-android-sdk-man:2.0.0@aar'
    }
    ```


## Application initialization {#section_txp_k51_r2b .section}

Before you use the app UserTrack analytics Android SDK for data statistics and monitoring, the context of the SDK needs to be declared for permission, application context passed, access control, and other initialization configurations, one of the permissions is declared in androidmanifest. in an XML file.

**3.1 Permission statement and AppKey and AppSecret configurations**

The following are the Android permissions required by the Mobile Analytics Android SDK and the AppKey and AppSecret configurations. Make sure these permissions are already configured in the file AndroidManifest.xml. Otherwise the SDK cannot work properly.

```
...
&lt;meta-data android:name="com.alibaba.app.appkey" android:value="YourAppKey">&lt;/meta-data>
&lt;meta-data android:name="com.alibaba.app.appsecret" android:value="YourAppSecret">&lt;/meta-data>
&lt;/application>
&lt;uses-permission android:name="android.permission.INTERNET">&lt;/uses-permission>
&lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE">&lt;/uses-permission>
&lt;uses-permission android:name="android.permission.GET_TASKS">&lt;/uses-permission>
&lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/>
&lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE">&lt;/uses-permission>
&lt;uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE">&lt;/uses-permission>
&lt;uses-permission android:name="android.permission.READ_SETTINGS"/>
&lt;uses-permission android:name="android.permission.WRITE_SETTINGS"/>
&lt;uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

**Examples of initialization and parameter setting**

In the Application implementation class, add the code to initialize the SDK.

The initialization API of App Usertack Analytics Android SDK:

```
public class YourApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
    /*  [Note] We recommend that you initialize MAN in Application to ensure the normal access to MANService/
    // Obtain MAN service
    MANService manService = MANServiceProvider.getService();
    // Enable the debug log. We recommend that you disable it for an online version.
    // manService.getMANAnalytics().turnOnDebug();
     // Set the channel (to mark the distribution channel name of this App). If not interested, you may skip this step so that this API is not called. Channel setting affects the report presentation of the Channel Analytics topic in the console. If the method in Section 3.3 can better meet your channel configuration needs, make configurations in accordance with Section 3.3 instead of calling this method;
    manService.getMANAnalytics().setChannel("A channel");
    // One of the MAN initialization methods is getting appKey and appSecret from AndroidManifest.xml
    manService.getMANAnalytics().init(this, getApplicationContext());
    // Another MAN initialization method is to manually specify appKey and appSecret
    // String appKey = "******";
    // String appsecret = "******";
    // manService.getMANAnalytics().init(this, getApplicationContext(), appKey, appSecret);
    // Disable the auto-hitting feature of the page with this API. For details, see 4.2
    manService.getMANAnalytics().turnOffAutoPageTrack();
    // If android:versionName in AndroidManifest.xml fails to meet your requirements, it can be specified here
    // If the app version is not set in either of the two locations, the reported field is null by default
    manService.getMANAnalytics().setAppVersion("3.1.1");
    }
}
```

**Configure channel information**

You can configure your channel information in AndroidManifest.xml by replacing `&lt;YOUR CHANNEL ID>` with your channel information.

**Note:** \[Note\] When SDK initialization is implemented, the field in AndroidManifest.xml is automatically obtained, and then entered in the channel field. When initialization is completed, if the setChannel method is also called, the parameters in setChannel method prevail.

```
&lt;application ...
  &lt;meta-data
    android:name="ALIYUN_MAN_CHANNEL"
        android:value="&lt;YOUR CHANNEL ID>" >
    &lt;/meta-data>
&lt;/application>
```

## Business Data Statistics {#section_e5j_dx1_r2b .section}

**Member account information burial point**

-   User Registration burial point

    After a successful user registration, you can use userRegister to complete the user registration tracking.

    ```
    MANService manService = MANServiceProvider.getService();
    // User registration tracking
    manService.getMANAnalytics().userRegister("usernick");
    ```

-   User logon tracking:

    User Login burial points are as follows:

    ```
    MANService manService = MANServiceProvider.getService();
    // User logon tracking
    manService.getMANAnalytics().updateUserAccount("usernick", "userid");
    ```

-   User logout tracking:

    The user logoff buried points are as follows:

    ```
    // User logout tracking
    manService.getMANAnalytics().updateUserAccount("", "");
    ```


**Page burial point**

Note: App Usertrack Analytics SDK is set to automatically acquire Activity page of Android 4.0 and later by default. If automatic acquisition is unnecessary, disable automatic page hitting using the following methods. When the automatic page tracking is enabled, the default page name is class.getSimpleName \(\). Remove the suffix Activity.

```
// Disable automatic hitting
MANService manService = MANServiceProvider.getService();
manService.getMANAnalytics().turnOffAutoPageTrack();
```

To meet page acquisition needs of the systems lower than Android 4.0, you can perform manual tracking using the following instructions.

**Note:** Note: If no page tracking is available in the App, no statistics can be perform on the Active User parameter.

-   Manual activity page burial point

    In the onreason and onpause of the activity, add the pageappear and the exact code, respectively, the following code is recommended to be done in a base class, let all other activity classes inherit this base class, all subclassed page burial points can be completed. The template sample is as follows:

    ```
    public class BaseActivity extends Activity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
        }
        @Override
        protected void onResume() {
            super.onResume();
            MANService manService = MANServiceProvider.getService();
            manService.getMANPageHitHelper().pageAppear(this);
        }
        @Override
        protected void onPause() {
            super.onPause();
            MANService manService = MANServiceProvider.getService();
            manService.getMANPageHitHelper().pageDisAppear(this);
        }
    }
    ```

-   Adding attribute statistics to an activity page

    Assuming you want to develop a shopping app, you want to know what items are displayed on the baby display page of the shopping app. In simple terms, we can add an item property to the acquired page \(GoodsDetails page\) records, such as item\_id=xxxxxx. The template sample is as follows:

    ```
    // The statement can be called anywhere before the onPause of a page is called
    Map&lt;String, String> lMap = new HashMap&lt;String, String>();
    lMap.put("item_id", "xxxxxx");
    MANService manService = MANServiceProvider.getService();
    manService.getMANPageHitHelper().updatePageProperties(lMap);
    ```

    The sample log is as follows:

    ```
    11-06 16:10:32.088: I/cache_log(10879): UT:...||2001||-||-||3191||item_id=xxxxxx
    ```

    -   2001: ID of the page event.
    -   3191: the length of time the page was displayed.
    -   Item\_id = dusk: that is, the property.
-   Page Base burial point usage

    The previous sections focus on page tracking at activity level. Data reporting can be completed by calling pageAppear and pageDisapear. The reported data includes page name, source page name, time-on-page and additional property settings.

    If you want to track a non-Activity page \(for example, a Fragment page\), or flexibly manage the reported information of page tracking such as modifying the reported page information and specially processing the time-on-page, you can use the basic page tracking.

    Gets the base page management object, as follows:

    ```
    // The input parameter is the page name
    MANPageHitBuilder pageHitBuilder = new MANPageHitBuilder(String pageName);
    ```

    Set the Source Page name as follows:

    ```
    pageHitBuilder.setReferPage(String referPageName);
    ```

    Set the page stop time as follows:

    ```
    pageHitBuilder.setDurationOnPage(long duration);
    ```

    Set page properties as follows:

    ```
    pageHitBuilder.setProperty(String key, String value);
    pageHitBuilder.setProperties(Map&lt;String, String> properties);
    ```

    Construct the page burial point log data as follows:

    ```
    pageHitBuilder.build();
    ```


## Performance data statistics {#section_jhd_yz1_r2b .section}

The performance of apps has a direct influence on user experience, which is easily overlooked by developers. Mobile Analytics provides multiple performance monitoring APIs for you to monitor the status of your apps comprehensively.

**Basic Network Performance Statistics**

App Usertrack Analytics is encapsulated against the network performance statistics for traditional apps, making it easy for users to track data and get common metrics at the network level including:

-   Overall response time for the request
-   Overall app bandwidth utilization
-   Network exception statistics

After correct data tracking reporting, you can observe the corresponding performance metrics of your app on the Mobile Analytics monitor page on Alibaba Cloud console.

**Starting a network request**

App UserTrack Analytics performs network performance statistics based on a network request. You must bury some record points in the network request process so that Mobile Analytics can collect statistics for the request and report it to the data analysis center.

The statistics of a network request is completed with `MANNetworkPerformanceHitBuilder`. Before initiating a network request, you must call the following codes, record the Host information and the request method of this network request, and mark the start of the network request:

```
MANNetworkPerformanceHitBuilder networkPerfBuilder = new MANNetworkPerformanceHitBuilder("taobao.com", "GET");
/ Record the start of a network request by hitting /
networkPerfBuilder.hitRequestStart();
```

In addition, the Mobile Analytics Android SDK provides an API for attaching additional information:

```
/ With additional information to be reported /
networkPerfBuilder.withExtraInfo("name1", "value1");
```

**Note:** \[Note\] The host must be in the standard Hostname format. Otherwise it can be ignored. The method must be "GET" or "POST", and it is "GET" by default. Currently only these two values are supported.

The foreground display uses host and method as dimensions in these messages.

**Network request ends normally, statistical Request Response Time**

After the request is completed, another tracking is required to mark the end of the network request. The API is as follows:

```
networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);
```

The loadBytes parameter of type int is used to input the total volume of downloaded data for the network request for easy hitting record by Mobile Analytics Android SDK.

After calling this function, the whole tracking process of network performance is finished. The data of the network request is reported to the data analysis platform for analyzing and summarizing. Then, you can see the data in the configured MaxCompute table.

**Abnormal End of network request, reporting network exception**

Network request exceptions are inevitable. If an exception is thrown during tracking, which interrupts the flow of the network request, you must handle this problem, otherwise it can cause hitting data disorder.

To report the exception, you can only mark the exceptional request by selecting the default network exception type or the custom network exception type after the exception is captured:

**Default network exception types**

|Exception type or fig|Corresponding Interface|
|:--------------------|:----------------------|
|400&lt;=HttpResponseCode&lt;500|buildHttpCodeClientError4XX\(\)|
|500&lt;=HttpResponseCode|buildHttpCodeServerError5XX\(\)|
|MalformedURLException|buildMalformedURLException\(\)|
|InterruptedIOException|buildInterruptedIOException\(\)|
|SocketTimeoutException|buildSocketTimeoutException\(\)|
|IOException|buildIOException\(\)|

Usage:

```
// If you want to report other exceptions, replace the buildIOException with an appropriate API according to the preceding table
MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
    .withExtraInfo("error_url", url.toString())
    .withExtraInfo("other_info", "value");
networkPerfBuilder.hitRequestEndWithError(errorInfo);
```

**Custom network exception types**

If the default network exception type does not meet your needs, you can customize the network exception type, however, the network exception type encoding is within the range of 1001 &lt;= yourerrorcode &lt;= 1010. If you do not do a front-end configuration, it will be displayed in the form of error code \(10xx\) if the information corresponding to the error code is configured on the front end, the information you configured is displayed.

Usage:

```
MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
    .withExtraInfo("error_url", url.toString());
networkPerfBuilder.hitRequestEndWithError(errorInfo);
```

The errorInfo parameter of this API allows you to report some valuable information about this exception.

**Note:** \[Note\] Once the API `networkPerfBuilder.hitRequestStart();`of `MANNetworkPerformanceHitBuilder`called, you must verify that the code is run in `networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);` under normal conditions, or in `networkPerfBuilder.hitRequestEndWithError(errorInfo);` in case of exceptions. Otherwise, the information of the network request may fail to be reported.

**4.1.4 Call the send method of Tracker to report hitting data**

After hitting for various stages by builder, you must obtain MANTracker to report its data as follows:

```
manService.getMANAnalytics().getDefaultTracker().send(networkEvent); // Call the send method of Tracker to report the hitting data
```

The template sample is as follows:

```
MANService manService = MANServiceProvider.getService();
String urlString = "http://www.aliyun.com";
MANNetworkPerformanceHitBuilder networkPerformanceHitBuilder = new MANNetworkPerformanceHitBuilder("www.aliyun.com", "GET");
try {
    URL url = new URL(urlString);
    byte[] buf = new byte[64 * 1024];
    // Record-by-hitting request starts
    networkPerformanceHitBuilder.hitRequestStart();
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    // Establish the connection
    conn.connect();
    // Start obtaining response content
    int responseCode = conn.getResponseCode();
    long totalBytes = 0;
    int len = 0;
    if (responseCode == 200) {
        // Read all the response content
        InputStream in = conn.getInputStream();
        while ((len = in.read(buf)) ! = -1) {
            Totalbytes + = Len;
        }
        in.close();
    }
    // With additional information to be reported
    networkPerformanceHitBuilder.withExtraInfo("name1", "value1");
    // Mark-by-hitting request ends
    networkPerformanceHitBuilder.hitRequestEndWithLoadBytes(totalBytes);
} catch (IOException e) {
    e.printStackTrace();
    // As described in 5.1.3, select a default network exception type or a custom network exception type to report any network exception.
    MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
            .withExtraInfo("error_url", urlString)
            .withExtraInfo("other_info", "value");
    // MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
    // .withExtraInfo("error_url", urlString)
    // .withExtraInfo("other_info", "value");
    // Record errors by hitting
    networkPerformanceHitBuilder.hitRequestEndWithError(errorInfo);
}
// Report hitting data of network performance events
manService.getMANAnalytics().getDefaultTracker().send(networkPerformanceHitBuilder.build());
```

**Basic Network Performance Statistics**

If you require more fine-grained network metrics and have tracked network performance according to the instructions of 5.1, you can track the TCP connection time and first byte response time by following the operations in this section.

-   TCP build time

    If you want to count the time spent on TCP connection of the network request, after initiating the network request, you must call the following API to mark the successful TCP connection when the connection is established successfully:

    ```
    /Record the successful connection by hitting/
    Networkperfbuilder. hitfinished ();
    ```

    This function is typically called when the `httpURLConnection. connect ()` method or `httpClient. execute ()` method is completed.

-   The first byte response time of the request

    If you want to count the first byte response time of the network request, you must call the following API to mark the arrival of first byte:

    ```
    networkPerfBuilder.hitRecievedFirstByte();
    ```

    The first byte usually arrives after running the `httpURLConnection.getResponseCode()` method or the `HttpResponse.getStatusLine.getStatusCode()` method.

    Example code after increasing TCP build-up time and first-byte response time burial point, as follows:

    ```
    MANService manService = MANServiceProvider.getService();
    String urlString = "http://www.aliyun.com";
    MANNetworkPerformanceHitBuilder networkPerformanceHitBuilder = new MANNetworkPerformanceHitBuilder("www.aliyun.com", "GET");
    try {
        URL url = new URL(urlString);
        byte[] buf = new byte[64 * 1024];
        // Record-by-hitting request starts
        networkPerformanceHitBuilder.hitRequestStart();
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        // Establish the connection
        conn.connect();
        // Record the connection time by hitting
        networkPerformanceHitBuilder.hitConnectFinished();
        // Start obtaining response content
        int responseCode = conn.getResponseCode();
        // Record the time for the first packet by hitting
        networkPerformanceHitBuilder.hitRecievedFirstByte();
        long totalBytes = 0;
        int len = 0;
        if (responseCode == 200) {
            // Read all the response content
            InputStream in = conn.getInputStream();
            while ((len = in.read(buf)) ! = -1) {
                Totalbytes + = Len;
            }
            in.close();
        }
        // With additional information to be reported
        networkPerformanceHitBuilder.withExtraInfo("name1", "value1");
        // Mark-by-hitting request ends
        networkPerformanceHitBuilder.hitRequestEndWithLoadBytes(totalBytes);
    } catch (IOException e) {
        e.printStackTrace();
        // Select a default network exception type or a custom network exception type to report a network exception
        MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
                .withExtraInfo("error_url", urlString)
                .withExtraInfo("other_info", "value");
        // MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
        // .withExtraInfo("error_url", urlString)
        // .withExtraInfo("other_info", "value");
        // Record errors by hitting
        networkPerformanceHitBuilder.hitRequestEndWithError(errorInfo);
    }
    // Report hitting data of network performance events
    manService.getMANAnalytics().getDefaultTracker().send(networkPerformanceHitBuilder.build());
    ```

-   Overall response time for the request \(See 4.1 Basic network performance statistics\)
-   Overall app bandwidth utilization \(See 4.1 Basic network performance statistics\)
-   Network exception statistics \(See 4.1 Basic network performance statistics\)

**Custom performance event burial point**

Custom performance event tracking can be used for real-time monitoring of customized performance events \(such as the time consumed by cache reading and writing, by RPC of private network protocol, and by client algorithm\). After correct client tracking, you can perform multi-dimensional real-time data monitoring using the Alibaba Cloud console.

Custom performance events include the following parts:

-   1. Event name \(event\_label\), which must be a combination of letters, numbers, and underscores \[Required\]
-   The length of time taken from start to finish \(required \).
-   The property carried by the event \(optional \).

**Note:** \[Note\] Custom performance events are used to monitor the time overhead of user events. Make sure to pass the correct parameter by referring to the following code examples.

```
String labelKey = "fibonacci";
MANCustomPerformanceHitBuilder performanceHitBuilder = new MANCustomPerformanceHitBuilder(labelKey);
// Record the starting time of custom performance events
performanceHitBuilder.hitStart();
Fresno (1 );
// Record the ending time of custom performance events
performanceHitBuilder.hitEnd();
// Method 2 for duration setting
// long timeCost = 0;
// performanceHitBuilder.setDuration(timeCost);
performanceHitBuilder.withExtraInfo("EXTRA_INFO_KEY1", "EXTRA_INFO_VALUE")
        .withExtraInfo("EXTRA_INFO_KEY2", "EXTRA_INFO_VALUE");
// Report hitting data of custom performance events
manService.getMANAnalytics().sendCustomPerformance(performanceHitBuilder.build());
```

## 5. Tracking of custom events {#section_qvh_3fb_r2b .section}

Custom event tracking can be used to meet the customization needs of users. Custom performance events include the following parts:

-   1. Event name \(event\_label\)
-   2. Duration of the event from initiation to completion
-   3. Properties of the event
-   4. The page corresponding to the event

for example:

```
// Event name: play_music
MANCustomHitBuilder hitBuilder = new MANHitBuilders.MANCustomHitBuilder("playmusic");
// Use the following API to set the duration: three minutes.
hitBuilder.setDurationOnEvent(3 * 60 * 1000);
// Set the associated page name: Listen
hitBuilder.setEventPage("Listen");
// Set the property: genre_rock
hitBuilder.setProperty("type", "rock");
// Set the property: song title
hitBuilder.setProperty("title", "wonderful tonight");
// Send hitting data of custom events
MANService manService = MANServiceProvider.getService();
manService.getMANAnalytics().getDefaultTracker().send(hitBuilder.build());
```

Custom Event extension parameters can be viewed in the console **customization time** \> **detail data** \> **parameter analysis**, however, before viewing, add the event ID that you want to display in the console in **administration settings** \> **custom Time Management**. To monitor custom events in real time, see section 4.3 "Custom performance event" for details.

## Does the real-time validation data report properly? {#section_dth_mgb_r2b .section}

If you want to verify that the data is reported properly in real time, you can do the following.

1.  1. Open the debug log the SDK:

    ```
    manService.getMANAnalytics().turnOnDebug();
    ```

2.  View the process log in logcat, assuming that your app process name is test\_app, then check the log for the process test\_app: Channel, test\_app: the channel process starts 10 seconds after your app process starts.
3.  3. In the process test\_app:channel, if you see the following log: `[uploadLogs] ,isSendSuccess:true,upload` it indicates that the logs have been uploaded successfully. You can see the related data in your MaxCompute table about five minutes later.

## Reporting H5 data {#section_hcc_xgb_r2b .section}

Without separate SDK, the data of H5 page acquisition can be uploaded by notifying native through JSBridge, and then the corresponding method of MAN is called to report the data.

JavaScript code:

```
// First, the JS Alert message appears. Then, the capture communication is performed on the native end.
Alert ("jsbridge: // custom ");
```

Java overrides `WebChromeClient`the `onAlert` method of the scanner to intercept the agreed scheme, then the different methods are executed according to the corresponding content to achieve the purpose of calling the native burial point.

```
WebView webView = (WebView) findViewById(R.id.h5DemoWebview);
webView.setWebChromeClient(new WebChromeClient(){
    @Override
    public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
         // Determine the format (uri, json, and xml).
        Uri uri = Uri.parse(message);
        if ( uri.getScheme().equals( "jsbridge" ) ) {
            if ( uri.getHost().equals( "custom" ) ) {
                // Event name: play_music
                MANHitBuilders.MANCustomHitBuilder hitBuilder = new MANHitBuilders.MANCustomHitBuilder("playmusic");
                // Use the following API to set the duration: three minutes.
                hitBuilder.setDurationOnEvent(3 * 60 * 1000);
                // Set the associated page name: Listen
                hitBuilder.setEventPage("Listen");
                // Set the property: genre_rock
                hitBuilder.setProperty("type", "rock");
                // Set the property: song title
                hitBuilder.setProperty("title", "wonderful tonight");
                // Send hitting data of custom events
                MANService manService = MANServiceProvider.getService();
                manService.getMANAnalytics().getDefaultTracker().send(hitBuilder.build());
            }
            return true;
        }
        return super.onJsAlert(view, url, message, result);
    }
});
```

## Confused Configuration { .section}

```
-keep class com.alibaba.sdk.android.**{*;}
-keep class com.ut.**{*;}
-keep class com.ta. **{*;}
```

