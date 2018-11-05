# Android SDK 集成指南 {#concept_onv_rdb_r2b .concept}

本文将为您介绍数加端采集（App Usertrack） Android SDK的使用方式。

App Usertrack Android SDK是数加面向移动开发者提供的Android平台下的数据统计与监控服务。通过该SDK，开发者可以在自己的APP中便捷地进行数据埋点，监控日常的业务数据与网络性能数据，并通过数加控制台界面观察对应的数据报表展现。另外，您后续可以通过设置自定义的数据解析规则实现定制化的数据图表展现。

您可以通过获取工程源码获得移动数据分析服务的使用示例，详情请参见man\_android\_demo项目。

## 安装Mobile Analytics Android SDK {#section_mnc_ct1_r2b .section}

**手动集成SDK**

SDK目录结构

```
OneSDK
|-- libs
|-- |-- jniLibs
|   |   |-- armeabi
|   |   |   |-- libMotu.so                  
|   |   |   |-- libtnet-3.1.1.so               
|   |   |-- armeabi-v7a
|   |   |   |-- libMotu.so
|   |   |   |-- libtnet-3.1.1.so
|   |   |-- x86
|   |   |   |-- libMotu.so
|   |   |   |-- libtnet-3.1.1.so  
|   |-- alicloud-android-sdk-man-2.0.0.aar  -移动数据分析主功能包
|   |-- fastjson-1.1.41.sec01.jar          
|   |-- tlog_adapter-1.1.0.1.jar
|   |-- tnet-jni-3.1.1.jar
|   |-- ut-analytics-6.5.1.1.aar
|   |-- utdid4all-1.5.0.jar
```

SDK集成

-   手动复制目录下的jniLibs到目录src/main。
-   在build.gradle配置中添加以下配置项。

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


## 应用程序初始化 {#section_txp_k51_r2b .section}

您在使用App Usertrack Analytics Android SDK进行数据统计与监控前，需要对SDK的上下文进行权限声明、传递应用上下文、访问控制等初始化配置，其中权限声明在AndroidManifest.xml文件中进行。

**权限声明及配置AppKey、AppSecret**

以下是Mobile Analytics Android SDK所需要的Android权限及配置AppKey、AppSecret，请把这些权限配置到您的AndroidManifest.xml文件，否则，SDK将无法正常工作。

```
...
<meta-data android:name="com.alibaba.app.appkey" android:value="YourAppKey"></meta-data>
<meta-data android:name="com.alibaba.app.appsecret" android:value="YourAppSecret"></meta-data>
</application>
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
<uses-permission android:name="android.permission.GET_TASKS"></uses-permission>
<uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
<uses-permission android:name="android.permission.READ_SETTINGS"/>
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

**初始化及参数设置示例**

在Application的实现类中，添加初始化SDK的代码。

App Usertack Analytics Android SDK初始化部分的接口如下所示：

```
public class YourApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
    /* 【注意】建议您在Application中初始化MAN，以保证正常获取MANService*/
    // 获取MAN服务
    MANService manService = MANServiceProvider.getService();
    // 打开调试日志，线上版本建议关闭
    // manService.getMANAnalytics().turnOnDebug();
     // 设置渠道（用以标记该app的分发渠道名称），如果不关心可以不设置即不调用该接口，渠道设置将影响控制台【渠道分析】栏目的报表展现。如果文档3.3章节更能满足您渠道配置的需求，就不要调用此方法，按照3.3进行配置即可；
    manService.getMANAnalytics().setChannel("某渠道");
    // MAN初始化方法之一，从AndroidManifest.xml中获取appKey和appSecret初始化
    manService.getMANAnalytics().init(this, getApplicationContext());
    // MAN另一初始化方法，手动指定appKey和appSecret
    // String appKey = "******";
    // String appSecret = "******";
    // manService.getMANAnalytics().init(this, getApplicationContext(), appKey, appSecret);
    // 通过此接口关闭页面自动打点功能，详见文档4.2
    manService.getMANAnalytics().turnOffAutoPageTrack();
    // 若AndroidManifest.xml 中的 android:versionName 不能满足需求，可在此指定
    // 若在上述两个地方均没有设置appversion，上报的字段默认为null
    manService.getMANAnalytics().setAppVersion("3.1.1");
    }
}
```

**配置渠道信息**

您可以在AndroidManifest.xml中配置您的渠道信息，只需要将`<YOUR CHANNEL ID>`替换为您的渠道信息即可。

**说明：** SDK执行初始化时会自动获取AndroidManifest.xml中的字段，并填充渠道字段。初始化完成后，如果同时调用了setChannel方法，则以setChannel方法中的参数为准。

```
<application ...
  <meta-data
    android:name="ALIYUN_MAN_CHANNEL"
        android:value="<YOUR CHANNEL ID>" >
    </meta-data>
</application>
```

## 业务数据统计 {#section_e5j_dx1_r2b .section}

**会员账号信息埋点**

-   用户注册埋点

    您在注册成功之后，可使用userRegister完成用户注册埋点。

    ```
    MANService manService = MANServiceProvider.getService();
    // 注册用户埋点
    manService.getMANAnalytics().userRegister("usernick");
    ```

-   用户登录埋点

    用户登录埋点如下所示：

    ```
    MANService manService = MANServiceProvider.getService();
    // 用户登录埋点
    manService.getMANAnalytics().updateUserAccount("usernick", "userid");
    ```

-   用户注销埋点

    用户注销埋点如下所示：

    ```
    // 用户注销埋点
    manService.getMANAnalytics().updateUserAccount("", "");
    ```


**页面埋点**

App Usertrack Analytics SDK默认会自动采集Android 4.0及以上系统的Activity页面，如果不需要自动采集可使用下面方法关闭自动页面打点。打开页面自动埋点时，默认页面名称为class.getSimpleName\(\)并去除Activity后缀。

```
// 关闭自动打点
MANService manService = MANServiceProvider.getService();
manService.getMANAnalytics().turnOffAutoPageTrack();
```

为满足Android 4.0以下系统的页面采集需求，您可按照下文说明进行手动埋点。

**说明：** 如果APP中没有进行页面埋点，活跃用户参数不能正常统计。

-   手动Activity页面埋点

    在Activity的onReasume以及onPause中分别加入pageAppear和pageDisAppear代码，建议下述代码可以在一个基类中做，让其它所有的activity类都继承这个基类，便可完成所有子类页面埋点。代码示例如下所示：

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

-   给Activity页面增加属性统计

    假设要开发一个购物APP，您想要知道在购物APP的宝贝展示页面都展示了哪些商品。此时您在采集的页面（GoodsDetails页面）记录上增加一个宝贝属性即可，如item\_id=xxxxxx。代码示例如下所示：

    ```
    // 这句话要在一个页面的onPause 之前任何位置调用都可以
    Map<String, String> lMap = new HashMap<String, String>();
    lMap.put("item_id", "xxxxxx");
    MANService manService = MANServiceProvider.getService();
    manService.getMANPageHitHelper().updatePageProperties(lMap);
    ```

    结果日志如下所示：

    ```
    11-06 16:10:32.088: I/cache_log(10879): UT:...||2001||-||-||3191||item_id=xxxxxx
    ```

    -   2001：页面事件的ID。
    -   3191：页面展示的时长。
    -   item\_id=xxxxxx：即属性。
-   页面基础埋点使用

    上述是针对Activity级别的页面埋点，只需简单地调用pageAppear和pageDispear即可完成埋点对相应的数据上报，上报数据包括页面名称、来源页面名称、页面停留时间和额外属性设置等。

    如果需要对非Activity页面，如Fragment进行埋点，或者需要灵活把控页面埋点上报信息，如修改上报页面信息、对页面停留时长做特殊处理等，可以使用页面基础埋点的方式。

    获取基础页面打点对象，如下所示：

    ```
    // 传入参数为页面名称
    MANPageHitBuilder pageHitBuilder = new MANPageHitBuilder(String pageName);
    ```

    设置来源页面名称，如下所示：

    ```
    pageHitBuilder.setReferPage(String referPageName);
    ```

    设置页面停留时间，如下所示：

    ```
    pageHitBuilder.setDurationOnPage(long duration);
    ```

    设置页面属性，如下所示：

    ```
    pageHitBuilder.setProperty(String key, String value);
    pageHitBuilder.setProperties(Map<String, String> properties);
    ```

    构造页面埋点log数据，如下所示：

    ```
    pageHitBuilder.build();
    ```


## 性能数据统计 {#section_jhd_yz1_r2b .section}

APP性能直接决定了用户体验效果，同时也一直是业务开发人员容易忽视的问题。Mobile Analytics提供了丰富的性能监控API方便用户全方位监控自己的APP运行状态。

**基本网络性能统计**

App Usertrack Analytics针对传统APP的网络性能统计进行了封装，方便用户埋点，获取网络层面的常见指标，包括的指标如下所示：

-   请求的整体响应时间
-   APP整体带宽利用率
-   网络异常统计

在进行正确地数据埋点上报后，您可以在阿里云控制台Mobile Analytics监控页面观察您的APP的相应性能指标情况。

**开始网络请求**

App UserTrack Analytics对网络性能的统计以一次网络请求为基本单位，您需要在这次网络请求的流程中埋入一些记录点，让Mobile Analytics统计这次请求的相关数据，上报到数据分析中心。

对某个网络请求的统计通过`MANNetworkPerformanceHitBuilder`完成，在发起网络请求前，您需要调用如下代码，记录此次网络请求的Host信息和请求Method，并标记网络请求开始。

```
MANNetworkPerformanceHitBuilder networkPerfBuilder = new MANNetworkPerformanceHitBuilder("taobao.com", "GET");
/* 打点记录网络请求开始 */
networkPerfBuilder.hitRequestStart();
```

此外，Mobile Analytics Android SDK还提供了可以附加额外信息的接口。

```
/* 附带额外需要上报信息 */
networkPerfBuilder.withExtraInfo("name1", "value1");
```

**说明：** Host必须符合标准Hostname格式，否则会被忽略。Method必须是GET或POST，否则默认为GET，目前仅支持这两个值。

前台展示会用这些信息中的Host、Method作为维度。

**网络请求正常结束，统计请求响应时间**

网络请求正常结束后，您还需要做一次埋点，来标记该次网络请求的结束。接口如下：

```
networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);
```

类型为int的loadBytes参数是为了让您传入该次网络请求的总下载数据量，以便Mobile Analytics Android SDK打点记录。

在您调用这个函数后，整个网络性能的埋点流程就结束了，该次网络请求的相关数据会上报到数据分析平台，分析汇总后，您即可在自己配置的的MaxCompute表中看到数据。

**网络请求异常结束，上报网络异常**

网络请求无法避免异常情况，在埋点时，如果抛出异常打断了整个网络请求的流程，那么您需要对此做额外处理，否则将导致打点数据紊乱。

上报方法为：您在抓获异常以后选择默认网络异常类型或者自定义网络异常类型的任意一种方式来标记这次请求发生了异常。

**默认网络异常类型**

|异常类型或HttpResponseCode|对应接口|
|:--------------------|:---|
|400<=HttpResponseCode<500|buildHttpCodeClientError4XX\(\)|
|500<=HttpResponseCode|buildHttpCodeServerError5XX\(\)|
|MalformedURLException|buildMalformedURLException\(\)|
|InterruptedIOException|buildInterruptedIOException\(\)|
|SocketTimeoutException|buildSocketTimeoutException\(\)|
|IOException|buildIOException\(\)|

使用说明：

```
// 如果您需要上报其它异常，请按照上表替换buildIOException为相应接口
MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
    .withExtraInfo("error_url", url.toString())
    .withExtraInfo("other_info", "value");
networkPerfBuilder.hitRequestEndWithError(errorInfo);
```

**自定义网络异常类型**

如果默认网络异常类型无法满足您的需求，您可以自定义网络异常类型，但是网络异常类型编码要在1001<=YourErrorCode<=1010范围内。如果不进行前端配置将以错误编码（10xx）的形式进行展示，如果在前端配置错误编码对应的信息，则会展示您所配置的信息。

使用说明：

```
MANNetworkErrorInfo errorInfo  = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
    .withExtraInfo("error_url", url.toString());
networkPerfBuilder.hitRequestEndWithError(errorInfo);
```

这个接口的errorInfo参数是为了让您上报这次异常的一些您认为有价值的信息。

**说明：** 一旦您调用了`MANNetworkPerformanceHitBuilder`的`networkPerfBuilder.hitRequestStart();`接口，您需要确认代码在正常情况下执行到`networkPerfBuilder.hitRequestEndWithLoadBytes(loadBytes);`，或在异常请求下执行到`networkPerfBuilder.hitRequestEndWithError(errorInfo);`，否则此次网络请求的相关信息将上报失败。

**调用Tracker的send方法上报打点数据**

使用builder进行各个阶段的打点后，需要获取到MANTracker将它的数据上报，如下所示：

```
manService.getMANAnalytics().getDefaultTracker().send(networkEvent); // 调用Track的send方法把此次打点的数据上报
```

代码示例如下所示。

```
MANService manService = MANServiceProvider.getService();
String urlString = "http://www.aliyun.com";
MANNetworkPerformanceHitBuilder networkPerformanceHitBuilder = new MANNetworkPerformanceHitBuilder("www.aliyun.com", "GET");
try {
    URL url = new URL(urlString);
    byte[] buf = new byte[64 * 1024];
    // 打点记录请求开始
    networkPerformanceHitBuilder.hitRequestStart();
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    // 建立连接
    conn.connect();
    // 开始获取响应内容
    int responseCode = conn.getResponseCode();
    long totalBytes = 0;
    int len = 0;
    if (responseCode == 200) {
        // 读尽响应内容
        InputStream in = conn.getInputStream();
        while ((len = in.read(buf)) != -1) {
            totalBytes += len;
        }
        in.close();
    }
    // 附带额外需要上报信息
    networkPerformanceHitBuilder.withExtraInfo("name1", "value1");
    // 打点标记请求结束
    networkPerformanceHitBuilder.hitRequestEndWithLoadBytes(totalBytes);
} catch (IOException e) {
    e.printStackTrace();
    // 按照文档5.1.3的说明选择默认网络异常类型或者自定义网络异常类型来上报网络异常
    MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
            .withExtraInfo("error_url", urlString)
            .withExtraInfo("other_info", "value");
    // MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
    //        .withExtraInfo("error_url", urlString)
    //        .withExtraInfo("other_info", "value");
    // 打点，记录出错情况
    networkPerformanceHitBuilder.hitRequestEndWithError(errorInfo);
}
// 上报网络性能事件打点数据
manService.getMANAnalytics().getDefaultTracker().send(networkPerformanceHitBuilder.build());
```

**高级网络性能统计**

如果您对网络指标有更细粒度的要求，并且已经进行了网络性能埋点，此时如果按照本章进行操作，便可以增加TCP建连时间及首字节响应时间的埋点。

-   TCP建连时间

    如果您希望统计该次网络请求TCP建连的耗时，那么在发起网络请求以后，建连成功时，您需要调用以下接口标记建连成功。

    ```
    /*打点记录建连成功*/
    networkPerfBuilder.hitConnectFinished();
    ```

    这个函数一般在`httpURLConnection.connect()`方法或`httpClient.execute()`方法执行完毕时调用。

-   请求的首字节响应时间

    如果您希望统计该次网络请求的首字节响应时间，您需要调用以下接口标记首字节到达。

    ```
    networkPerfBuilder.hitRecievedFirstByte();
    ```

    这个时间点一般是在`httpURLConnection.getResponseCode()`方法或`HttpResponse.getStatusLine.getStatusCode()`方法执行完毕后。

    增加TCP建连时间及首字节响应时间埋点后的示例代码，如下所示：

    ```
    MANService manService = MANServiceProvider.getService();
    String urlString = "http://www.aliyun.com";
    MANNetworkPerformanceHitBuilder networkPerformanceHitBuilder = new MANNetworkPerformanceHitBuilder("www.aliyun.com", "GET");
    try {
        URL url = new URL(urlString);
        byte[] buf = new byte[64 * 1024];
        // 打点记录请求开始
        networkPerformanceHitBuilder.hitRequestStart();
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        // 建立连接
        conn.connect();
        // 打点记录建连时间
        networkPerformanceHitBuilder.hitConnectFinished();
        // 开始获取响应内容
        int responseCode = conn.getResponseCode();
        // 打点记录首包时间
        networkPerformanceHitBuilder.hitRecievedFirstByte();
        long totalBytes = 0;
        int len = 0;
        if (responseCode == 200) {
            // 读尽响应内容
            InputStream in = conn.getInputStream();
            while ((len = in.read(buf)) != -1) {
                totalBytes += len;
            }
            in.close();
        }
        // 附带额外需要上报信息
        networkPerformanceHitBuilder.withExtraInfo("name1", "value1");
        // 打点标记请求结束
        networkPerformanceHitBuilder.hitRequestEndWithLoadBytes(totalBytes);
    } catch (IOException e) {
        e.printStackTrace();
        // 选择默认网络异常类型或者自定义网络异常类型来上报网络异常
        MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildIOException()
                .withExtraInfo("error_url", urlString)
                .withExtraInfo("other_info", "value");
        // MANNetworkErrorInfo errorInfo = MANNetworkErrorCodeBuilder.buildCustomErrorCode(1001)
        //        .withExtraInfo("error_url", urlString)
        //        .withExtraInfo("other_info", "value");
        // 打点，记录出错情况
        networkPerformanceHitBuilder.hitRequestEndWithError(errorInfo);
    }
    // 上报网络性能事件打点数据
    manService.getMANAnalytics().getDefaultTracker().send(networkPerformanceHitBuilder.build());
    ```

-   请求的整体响应时间 （请参见基本网络性能统计）
-   APP整体带宽利用率 （请参见基本网络性能统计）
-   网络异常统计 （请参见基本网络性能统计）

**自定义性能事件埋点**

自定义性能事件埋点可满足您针对定制化的性能事件进行实时监控的需求，例如Cache的读写耗时，私有网络协议的RPC耗时，客户端算法的耗时等。在进行正确的客户端埋点后，您可通过阿里云控制台进行数据的多维度实时监控。

自定义性能事件包含以下内容：

-   事件名称（event\_label），只能为字母、数字和下划线组成（必选）。
-   事件从开始到完成消耗的时长（必选）。
-   事件所携带的属性（可选）。

**说明：** 自定义性能事件用于监控用户事件的时间开销，请参见下述代码实例正确传递参数。

```
String labelKey = "fibonacci";
MANCustomPerformanceHitBuilder performanceHitBuilder = new MANCustomPerformanceHitBuilder(labelKey);
// 记录自定义性能事件起始时间
performanceHitBuilder.hitStart();
fibonacci(1);
// 记录自定义性能事件结束时间
performanceHitBuilder.hitEnd();
// 设置时长方法2
// long timeCost = 0;
// performanceHitBuilder.setDuration(timeCost);
performanceHitBuilder.withExtraInfo("EXTRA_INFO_KEY1", "EXTRA_INFO_VALUE")
        .withExtraInfo("EXTRA_INFO_KEY2", "EXTRA_INFO_VALUE");
// 上报自定义性能事件打点数据
manService.getMANAnalytics().sendCustomPerformance(performanceHitBuilder.build());
```

## 自定义事件埋点 {#section_qvh_3fb_r2b .section}

自定义事件埋点可用于满足您的定制化需求。自定义事件包括以下内容：

-   事件名称（event\_label）
-   事件从开始到完成消耗的时长
-   事件所携带的属性
-   事件对应的页面

示例如下：

```
// 事件名称：play_music
MANCustomHitBuilder hitBuilder = new MANHitBuilders.MANCustomHitBuilder("playmusic");
// 可使用如下接口设置时长：3分钟
hitBuilder.setDurationOnEvent(3 * 60 * 1000);
// 设置关联的页面名称：聆听
hitBuilder.setEventPage("Listen");
// 设置属性：类型摇滚
hitBuilder.setProperty("type", "rock");
// 设置属性：歌曲标题
hitBuilder.setProperty("title", "wonderful tonight");
// 发送自定义事件打点
MANService manService = MANServiceProvider.getService();
manService.getMANAnalytics().getDefaultTracker().send(hitBuilder.build());
```

自定义事件扩展参数可在控制台**自定义时间** \> **详细数据** \> **参数分析**中进行查看，但查看之前请在**管理设置** \> **自定义时间管理**中添加要在控制台显示的事件ID。如果您需要对自定义事件进行实时监控，请参见自定义性能事件。

## 实时验证数据是否正常上报 {#section_dth_mgb_r2b .section}

您如果想实时验证数据是否正常上报，可进行以下操作。

1.  打开SDK的debug日志

    ```
    manService.getMANAnalytics().turnOnDebug();
    ```

2.  查看logcat中的进程日志，假设您的APP进程名称为test\_app，那么请查看进程test\_app:channel的日志，test\_app:channel进程会在您的APP进程启动10秒后启动。
3.  如果您在test\_app:channel进程中看到日志`[uploadLogs] ,isSendSuccess:true,upload`，即代表日志上传已经成功，稍后5分钟左右即可在您的MaxCompute表中看到相关数据。

## 上报H5数据 {#section_hcc_xgb_r2b .section}

H5页面采集并没有单独的SDK，依赖native进行上传，通过JSBridge通知给native，然后调用MAN的相应方法，进行数据的上报。

JavaScript代码示例如下：

```
// 这里通过在JS alert消息，然后在native端进行捕获通信
alert( "jsbridge://custom" );
```

Java重写`WebChromeClient`的`onAlert`方法，对于约定好的scheme，进行拦截，然后根据相应内容执行不同的方法，从而达到调用native埋点的目的。

```
WebView webView = (WebView) findViewById(R.id.h5DemoWebview);
webView.setWebChromeClient(new WebChromeClient(){
    @Override
    public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
         // 这里只要自己约定好方式就行，uri、json、xml等等。
        Uri uri = Uri.parse(message);
        if ( uri.getScheme().equals( "jsbridge" )  ) {
            if ( uri.getHost().equals( "custom" )  ) {
                // 事件名称：play_music
                MANHitBuilders.MANCustomHitBuilder hitBuilder = new MANHitBuilders.MANCustomHitBuilder("playmusic");
                // 可使用如下接口设置时长：3分钟
                hitBuilder.setDurationOnEvent(3 * 60 * 1000);
                // 设置关联的页面名称：聆听
                hitBuilder.setEventPage("Listen");
                // 设置属性：类型摇滚
                hitBuilder.setProperty("type", "rock");
                // 设置属性：歌曲标题
                hitBuilder.setProperty("title", "wonderful tonight");
                // 发送自定义事件打点
                MANService manService = MANServiceProvider.getService();
                manService.getMANAnalytics().getDefaultTracker().send(hitBuilder.build());
            }
            return true;
        }
        return super.onJsAlert(view, url, message, result);
    }
});
```

## 混淆配置 { .section}

```
-keep class com.alibaba.sdk.android.**{*;}
-keep class com.ut.**{*;}
-keep class com.ta.**{*;}
```

