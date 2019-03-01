# Android SDK integration guide {#concept_onv_rdb_r2b .concept}

This topic describes how to use DTplus data acquisition \(App Usertrack\) Android SDK.

App Usertrack Android SDK is a DTplus data statistics and monitoring service on Android platform for mobile developers. The SDK enables developers to easily track data in apps to monitor daily business data, network performance data, and observe the corresponding data report display in the DTplus console. In addition, you can customize the data chart display by setting custom data parsing rules.

You can obtain an example of Mobile Data Analysis Service from the engineering source code. For more information, see the man\_android\_demo project.

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

Before using the UserTrack app analytics, the Android SDK for data statistics and monitoring, the SDK context must be declared for permission. The application context passed, access control, and other initialization configurations. One of the permissions is declared as an AndroidManifest.xml file.

**3.1 Permission statement, AppKey, and AppSecret configurations**

The following are Android permissions required by the AppKey and AppSecret configurations of Mobile Analytics Android SDK . Ensure these permissions are already configured in AndroidManifest.xml file or SDK errors may occur.

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

**Examples of app initialization and parameter settings**

Add the code to initialize the SDK in the application implementation class.

Initialize the API of App Usertack Analytics Android SDK:

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

You can configure the channel information in AndroidManifest.xml by replacing `&lt;YOUR CHANNEL ID>` with your channel information.

**Note:** When the SDK initialization is implemented, the AndroidManifest.xml field is automatically obtained, and entered in the channel field., When the initialization is completed, if the setChannel method is also called, the parameters in the setChannel method prevail.

```
&lt;application ...
  &lt;meta-data
    android:name="ALIYUN_MAN_CHANNEL"
        android:value="&lt;YOUR CHANNEL ID>" >
    &lt;/meta-data>
&lt;/application>
```

## Business Data Statistics {#section_e5j_dx1_r2b .section}

**Member account information tracking point**

-   User Registration tracking point

    You can use userRegister to complete the user registration tracking after completing user registration.

    ```
    MANService manService = MANServiceProvider.getService();
    // User registration tracking
    manService.getMANAnalytics().userRegister("usernick");
    ```

-   User logon tracking:

    User Login tracking points are as follows:

    ```
    MANService manService = MANServiceProvider.getService();
    // User logon tracking
    manService.getMANAnalytics().updateUserAccount("usernick", "userid");
    ```

-   User logout tracking:

    The user logoff tracking points are as follows:

    ```
    // User logout tracking
    manService.getMANAnalytics().updateUserAccount("", "");
    ```


**Page tracking point**

Note: By default, the App Usertrack Analytics SDK is automatically set to obtain the Activity page of operation systems that are Android 4.0 and later. When the automatic page tracking is enabled, the default page name is class.getSimpleName \(\) and removes the suffix Activity. If automatic acquisition is unnecessary, disable the automatic page hitting with the following methods.

```
// Disable automatic hitting
MANService manService = MANServiceProvider.getService();
manService.getMANAnalytics().turnOffAutoPageTrack();
```

To meet page acquisition requirements in operating systems earlier than Android 4.0, you can perform manual tracking with the following instructions.

**Note:** If no page tracking is available in the App, no statistics can be performed on the Active User parameter.

-   Manual activity page tracking point

    In onreason and onpause of the Acitivity page, add the pageappear and the exact code. To complete all subclassed page tracking points, we recommend you use the following code respectively to complete activities in base class, and allow other activity classes to inherit the base class . The following is a template example:

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

-   Adding attribute statistics to an Activity page

    Assume you want to develop a shopping app, and want to view the items displayed on the baby display page of the shopping app. In other words, we can add an item property to the acquired page \(GoodsDetails page\) records, such as item\_id=xxxxxx. The following is a template example:

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

    -   2001: The page event ID.
    -   3191: The page duration is displayed.
    -   Item\_id = dusk: The property.
-   Using base page tracking point

    The preceding sections focus on page tracking at the Activity level, where data reporting can be completed by calling pageAppear and pageDisapear. The reported data includes the page name, source page name, time-on-page, and additional property settings.

    If you want to track a non-Activity page for example, a Fragment page, or flexibly manage the reported information from page tracking , such as modifying the reported page information. You can use basic page tracking for special processing, such as the time-on-page.

    Obtain the base page management object as follows:

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

    Construct the page tracking point log data as follows:

    ```
    pageHitBuilder.build();
    ```


## Performance data statistics {#section_jhd_yz1_r2b .section}

Developers often overlook how app performance can directly affect user experience. Mobile Analytics provides multiple performance monitoring APIs for you to comprehensively monitor the app status.

**Basic network performance statistics**

App Usertrack Analytics is encapsulated against the network performance statistics for traditional apps, making it easy for users to track data and obtain common metrics at the network level including:

-   Overall response time for the request
-   Overall app bandwidth utilization
-   Network exception statistics

After reporting the statistics from the correct data tracking point, you can view the apps corresponding performance metrics on the Mobile Analytics monitor page on Alibaba Cloud console.

**Starting a network request**

The App UserTrack Analytics performs network performance statistics based on a network request. You must track some record points in the network request process so that Mobile Analytics can collect the request statistics and report it to the data analysis center.

The network request statistics is completed with `MANNetworkPerformanceHitBuilder`. Before initiating a network request, you must call the following codes, record the Host information, and the request method of the network request, and mark the start of the network request:

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

**Note:** The host must be in the standard Hostname format or it will be ignored. The method must be "GET" or "POST". The default method is "GET". Currently, only these two values are supported.

The front end display uses Host and Method as dimensions in these messages.

**Normal network request response time**

After the network request is completed, another tracking point is required to mark the end of the network request. The API is as follows:

```
networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);
```

The loadBytes parameter of int type is used to input the total downloaded data volume of the network request for easy hitting records by Mobile Analytics Android SDK.

After calling this function, the entire tracking process of network performance is completed. The network request data is reported to the data analysis platform for analysis and summary. Then, you can view data in the configured MaxCompute table.

**Reporting network exceptions**

If a network exception occurs during tracking that interrupts the network request flow. You must handle this issue, otherwise it can cause hitting data disorder.

To report captured exceptions, mark the exception request by selecting the default network exception type or the custom network exception type:

**Default network exception types**

|Exception type or fig|Corresponding interface|
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

If the default network exception type does not meet your needs, you can customize network exception types, however, the network exception type encoding is within the range of 1001 &lt;= yourerrorcode &lt;= 1010. If you are not using a front end configuration, it will be displayed as an error code \(10xx\) if the information corresponding to the error code is configured on the front end. The information you configured is displayed.

Usage:

```
MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
    .withExtraInfo("error_url", url.toString());
networkPerfBuilder.hitRequestEndWithError(errorInfo);
```

The API errorInfo parameter allows you to report important exception information.

**Note:** Once you call the API `networkPerfBuilder.hitRequestStart();`of `MANNetworkPerformanceHitBuilder`, you must verify that the code is run in `networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);`. Under normal conditions, or in `networkPerfBuilder.hitRequestEndWithError(errorInfo);` in case of exceptions. Otherwise, the network request information may fail to be reported.

**4.1.4 Call the Tracker send method to report hitting data**

After using builder for hitting data in various phases, you must obtain the MANTracker to report data as follows:

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

**Advanced network performance statistics**

If you require fine-grained network metrics and have used tracking points in network performance, according to instructions in 5.1. You can track the TCP connection time and first byte response time by following operations in this section.

-   TCP build time

    If you want to count the time spent on the network request TCP connections . After initiating the network request, you must call the following API to mark the successful TCP connection when the connection is established successfully:

    ```
    /Record the successful connection by hitting/
    Networkperfbuilder. hitfinished ();
    ```

    This function is typically called when the `httpURLConnection. connect ()` method or `httpClient. execute ()` method is completed.

-   The request first byte response time

    If you want to count the first byte response time of the network request, you must call the following API to mark the arrival of the first byte:

    ```
    networkPerfBuilder.hitRecievedFirstByte();
    ```

    The first byte usually arrives after running the `httpURLConnection.getResponseCode()` method or the `HttpResponse.getStatusLine.getStatusCode()` method.

    Example code after increasing TCP build-up time and first-byte response time tracking point as follows:

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

-   Overall request response time \(See 4.1 Basic network performance statistics.\)
-   Overall app bandwidth utilization \(See 4.1 Basic network performance statistics.\)
-   Network exception statistics \(See 4.1 Basic network performance statistics.\)

**Custom performance event tracking point**

Custom performance event tracking can be used for real-time monitoring of customized performance events, such as time consumed by cache reading and writing, by RPC of private network protocol, and by client algorithm. After correct client tracking, you can perform multi-dimensional real-time data monitoring using the Alibaba Cloud console.

Custom performance events include the following parts:

-   The event name \(event\_label\), which must be a combination of letters, numbers, and underscores \[Required\].
-   The time duration from start to finish \(required\).
-   The property carried by the event \(optional\).

**Note:** Custom performance events are used to monitor the time overhead of user events. Ensure the correct parameter is passed by using the following code examples for reference .

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

Custom event tracking can be used to meet users customization requirements. Custom performance events include the following parts:

-   1. Event name \(event\_label\)
-   2. Event duration from initiation to completion.
-   3. The event properties.
-   4. The page corresponding to the event.

For example:

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

Custom Event extension parameters can be viewed in the console **Customization Time** \> **Detail Data** \> **Parameter Analysis**. Before viewing, you need to add the event ID to display in the console under **Administration Settings** \> **Custom Time Management**. For more information about how to monitor custom events in real time, see section 4.3 "Custom performance event".

## Does the real-time validation data report properly? {#section_dth_mgb_r2b .section}

If you want to verify that the data is reported in real time, you can perform the following.

1.  Open the SDK debug log:

    ```
    manService.getMANAnalytics().turnOnDebug();
    ```

2.  View the process log in logcat, assume your app process name is test\_app, then check the log for the process test\_app: Channel, test\_app: The channel process starts 10 seconds after your app process starts.
3.  In the process test\_app:channel, if you see the following log: `[uploadLogs] ,isSendSuccess:true,upload` it indicates that the logs have been uploaded successfully. You can see the related data in the MaxCompute table about five minutes later.

## Reporting H5 data {#section_hcc_xgb_r2b .section}

The H5 page data aggregation can be uploaded by notifying native through JSBridge, and then call the corresponding MAN method is called to report data.

JavaScript code:

```
// First, the JS Alert message appears. Then, the capture communication is performed on the native end.
Alert ("jsbridge: // custom ");
```

Java overrides the `onAlert` method in the `WebChromeClient`and intercepts the specified scheme. Then Java executes the method based on the corresponding content to achieve the purpose of calling the native tracking point.

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

## Obfuscate configuration { .section}

```
-keep class com.alibaba.sdk.android.**{*;}
-keep class com.ut.**{*;}
-keep class com.ta. **{*;}
```

