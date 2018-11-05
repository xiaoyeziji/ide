# IOS SDK 集成指南 {#concept_wbz_jmv_q2b .concept}

本文档将为您介绍数加端数据采集（App Usertrack Analytics）IOS SDK的使用方式。

App Usertrack Analytics IOS SDK是数加面向移动开发者提供的IOS平台下的数据统计与监控服务。通过该SDK，开发者可以在自己的APP中便捷地进行数据埋点，监控日常的业务数据与性能数据，并通过阿里云控制台界面观察对应的数据报表展现。另外，用户可以通过设定自定义的数据解析规则实现定制化的数据图表展现。

您可以通过获取工程源码获得移动数据分析服务的使用例程，具体参考`man_ios_demo`项目即可。

## 安装Mobile Analytics iOS SDK {#section_t4j_tmv_q2b .section}

手动集成SDK

将下载的包加至Link Binary With Libraries，包括：

-   AlicloudMobileAnalytics.framework
-   UT.framework
-   UTDID.framework
-   CrashReporter.framework
-   FMDB.framework
-   Tnet.framework
-   Reachability.framework

加载系统必须依赖的包：

-   libz.tbd
-   libresolv.tbd
-   libstdc++.tbd
-   libsqlite3.tbd
-   CoreTelephony.framework
-   SystemConfiguration.framework

## 引用头文件 {#section_k2z_vnv_q2b .section}

```
#import <AlicloudMobileAnalitics/ALBBMAN.h>
```

**说明：** 请在应用的**targets** \> **Build Settings** \> **linking** \> **Other Linker Flags**加上ObjC属性，否则将导致数据无法正常上报。

## 获取Mobile Analytics服务 {#section_ecz_p4v_q2b .section}

在您使用App Usertrack Mobile Analytics iOS SDK进行数据统计与监控前，您需要首先获取Mobile Analytics服务，然后可以进行版本和渠道的配置。

```
// 获取MAN服务
ALBBMANAnalytics *man = [ALBBMANAnalytics getInstance];
// 打开调试日志，线上版本建议关闭
// [man turnOnDebug];
// 初始化MAN
[man initWithAppKey:testAppKey secretKey:testAppSecret];
// appVersion默认从Info.list的CFBundleShortVersionString字段获取，如果没有指定，可在此调用setAppversion设定
// 如果上述两个地方都没有设定，appVersion为"-"
[man setAppVersion:@"2.3.1"];
// 设置渠道（用以标记该app的分发渠道名称），如果不关心可以不设置即不调用该接口，渠道设置将影响控制台渠道分析栏目的报表展现。
[man setChannel:@"50"];
```

## 业务数据统计 {#section_bnt_wfb_r2b .section}

数据统计的准确性依赖被监控APP的常规生命轨迹，比如应用启动次数依赖于用户正常退出应用触发的上报策略。

-   登录会员
    -   接口：

        ```
        - (void)updateUserAccount:(NSString *)pNick userid:(NSString *)pUserId;
        ```

    -   功能：获取登录会员，然后给每条日志添加登录会员字段。
    -   是否必须调用：否。
    -   调用时机：登录时调用。
    -   备注：阿里云平台上的登录会员UV指标依赖该接口。
-   注册会员

    -   接口：

        ```
        - (void)userRegister:(NSString *)pUsernick;
        ```

    -   功能：产生一条注册会员事件日志。
    -   是否必须调用：否。
    -   调用时机：注册时调用。
    -   备注：阿里云平台上注册会员指标依赖该接口。
    代码示例：

    ```
    ALBBMANAnalytics *man = [ALBBMANAnalytics getInstance];
    [man userRegister:@"userNick"];
    [man updateUserAccount:@"userNick" userid:@"userId"];
    ```

-   页面埋点

    使用ALBBMANPageHitHelper可以进行ViewController级别的页面埋点，ALBBMANPageHitHelper为页面埋点辅助类，可以自动完成页面名称（默认获取ViewController名称并去除后缀），来源页面和页面停留时间的统计，如需使用更为灵活的页面埋点方案，可使用`ALBBMANPageHitBuilder`页面打点基础类进行页面埋点。

    **说明：** 如果APP中没有进行页面埋点，`活跃用户`参数不能正常统计。

-   进入页面
    -   接口：

        ```
        - (void)pageAppear:(UIViewController *)pViewController;
        ```

    -   功能：记录页面进入时的一些状态信息，但不发送日志，和pageDisappear配合使用。
    -   入参：UIViewController或其子类指针。
    -   是否必须调用：需要对页面埋点时调用。
    -   调用时机：viewDidAppear。
    -   备注：必须和pageDisappear搭配使用。
-   退出页面

    接口：

    ```
    - (void)pageDisAppear:(UIViewController *)pViewController;
    ```

-   设置页面扩展参数

    -   接口：

        ```
        - (void)updatePageProperties:(UIViewController *)pViewController properties:(NSDictionary *)pProperties;
        ```

    -   功能：设置页面扩展参数。
    -   是否必须调用：否。
    -   调用时机：调用pageDisAppear之前。
    代码示例：

    ```
    // 进入页面
    [[ALBBMANPageHitHelper getInstance] pageAppear:self];
    // 设置页面事件扩展参数
    NSDictionary *properties = [NSDictionary dictionaryWithObject:@"pageValue" forKey:@"pageKey"];
    [[ALBBMANPageHitHelper getInstance] updatePageProperties:self properties:properties];
    // 离开页面
    [[ALBBMANPageHitHelper getInstance] pageDisAppear:self];
    ```

-   页面事件

    ALBBMANPageHitBuilder类也是用来产生页面事件日志的，虽然可以通过pageAppear和pageDisAppear来进行页面埋点，但是它针对的只是UIViewController级别的页面，并不能满足一些场景的页面事件。例如UIView，如果用户需要将一个UIView当做一个页面，那么就可以通过ALBBMANPageHitBuilder来进行页面事件的埋点，即用户自己采集页面事件相关的信息（如页面的 refer、页面停留时间等），通过ALBBMANPageBuilder构造出一条页面事件的日志map，最后通过某个ALBBMANTracker埋点实例的send API发送上传。

    ALBBMANTracker是一个用于对埋点数据进行上报的工具，下文提到的ALBBMANPageHitBuilder等事件类都是通过ALBBMANTracker进行事件件上报的。其中ALBBMANTracker的获取如下所示：

    ```
    // 获取默认ALBBMANTracker实例ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
    ```

    埋点数据上报都是通过ALBBMANTracker进行，可以设定/删除上报数据的全局字段，全局字段设定后在所有的上报日志中都可以查看，如下所示：

    ```
    ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
    // 设定全局字段
    [tracker setGlobalProperty:@"globalKey1" value:@"globalValue1"];
    // 删除全局字段
    [tracker removeGlobalProperty:@"globalKey1"];
    ```

-   设置页面名称
    -   接口：

        ```
        - (void)setPageName:(NSString *)pPageName;
        ```

    -   功能： 设置页面名称。
    -   入参： 页面名称，pPageName不能为空。
    -   是否必须调用： 是，在已经创建ALBBMANPageHitBuilder实例后，必须调用该方法。
    -   调用时机：创建ALBBMANPageHitBuilder后，必须调用setPageName，否则调用build后返回为nil。
    -   备注：页面名称是页面事件的基础，必须设置。
-   设置页面refer
    -   接口：

        ```
        - (void)setReferPage:(NSString *)pReferPageName;
        ```

    -   功能：设置页面的refer，即当前页面的来源页面。
    -   入参：refer页面名称，refer可以为空。
    -   是否必须调用：否。
-   设置页面停留时间
    -   接口：

        ```
        - (void)setDurationOnPage:(long long)durationTimeOnPage;
        ```

    -   功能：记录页面从展现到页面离开的停留时间。
    -   入参：页面停留时间。
    -   是否必须调用：否。
    -   调用时机：需要记录当前页面停留时间。
-   设置页面事件扩展参数
    -   接口：

        ```
        - (void)setProperty:(NSString *)pKey value:(NSString *)pValue;
        ```

    -   功能：给单条日志添加一个扩展参数。
    -   入参：key和value都不能为nil，其中key不能为PAGE/EVENTID/ARG1/ARG2/ARG3/ARGS，否则build返回nil。
    -   是否必须调用：否。
    -   调用时机：需要给ALBBMANPageHitBuilder实例添加扩展参数时调用。
-   组装单条日志map

    -   接口：

        ```
        - (NSDictionary *)build;
        ```

    -   功能：将塞入的参数组成map返回。
    -   入参：无。
    -   是否必须调用：否。
    -   调用时机：创建ALBBMANPageHitBuilder或ALBBMANCustomHitBuilder实例，塞入一些业务字段后，调用build组装业务字段生成日志map，通过某个ALBBMANTracker埋点实例send接口发送。
    -   备注：build函数返回的日志map，必须通过ALBBMANTracker埋点上报工具的send接口上传数据完成打点。
    代码示例：

    ```
    ALBBMANPageHitBuilder *pageHitBuilder = [[ALBBMANPageHitBuilder alloc] init];
    // 设置页面refer
    [pageHitBuilder setReferPage:@"pageRefer"];
    // 设置页面名称
    [pageHitBuilder setPageName:@"pageName"];
    // 设置页面停留时间
    [pageHitBuilder setDurationOnPage:100];
    // 设置页面事件扩展参数
    [pageHitBuilder setProperty:@"pagePropertyKey1" value:@"pagePropertyValue1"];
    [pageHitBuilder setProperty:@"pagePropertyKey2" value:@"pagePropertyValue2"];
    ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
    // 组装日志并发送
    [tracker send:[pageHitBuilder build]];
    ```


## 性能数据统计 {#section_vgw_ljb_r2b .section}

APP性能直接决定了用户体验效果，同时也一直是业务开发人员容易忽视的问题。Mobile Analytics提供了丰富的性能监控API方便用户全方位监控自己的APP运行状态。

网络性能统计

Mobile Analytics针对传统APP的网络性能统计进行了封装，方便用户一键埋点，获取网络层面的关键技术指标。

-   请求的整体响应时间
-   网络异常统计

接口及统计

从客户端角度来看，一个正常的网络请求分为tcp建连、请求发送、开始接收响应数据、接受数据完成四个阶段。分阶段性能指标有：TCP建连时间、首字节时间、整体的响应时间。针对这几个阶段，分别提供有对应的API接口。

```
- (instancetype)initWithHost:(NSString *)host method:(NSString *)method;
/*
* 用户可以自行设置属性，但是不要包含“/Host/Method/EVENTID/PAGE/ARG1/ARG2/ARG3/ARGS/COMPRESS”
*/
- (void)setProperty:(NSString *)pKey value:(NSString *)pValue;
- (void)setproperties:(NSMutableDictionary *)properties;
/*
* 网络请求打点开始
*/
- (void)requestStart;
/*
* 打点标记建连成功
*/
- (void)connectFinished;
/*
* 打点标记首字节到达
*/
- (void)requestFirstBytes;
/*
* 打点正常结束
*/
- (void)requestEndWithBytes:(long)loadBytes;
/*
* 出错的时候上报
*/
- (void)requestEndWithError:(ALBBMANNetworkError *)error;
/*
* 整合打点日志
*/
- (NSDictionary *)build;
```

初始化ALBBMANNetworkHitBuilder时，必须使用Host/Method。若传入Nil，数据将无法上报。以NSURLConnection为例：

```
#import 
/**
* @brief 
同步请求网络性能埋点
*/
- (void)syncNetworkHit {
NSURL *URL = [NSURL URLWithString:@"http://www.taobao.com/"];
NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:URL
cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
timeoutInterval:30];
// 由于NSURLConnection同步接口，封装了TCP连接、首字节的过程，所以同步接口上面我们统计不到TCP建连时间跟首字节时间
ALBBMANNetworkHitBuilder *builder = [[ALBBMANNetworkHitBuilder alloc] initWithHost:URL.host method:[request HTTPMethod]];
// 开始请求打点
[builder requestStart];
NSHTTPURLResponse *response;
NSError *error;
NSData *data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];
if (error) {
// 自定义网络异常，见文档5.1.2
ALBBMANNetworkError *networkError = [[ALBBMANNetworkError alloc] initWithErrorCode:1001];
// 自定义网络异常附加信息
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
}
if (response.statusCode >= 400 && response.statusCode < 600) {
// 默认网络异常，见文档5.1.2
ALBBMANNetworkError *networkError;
if (response.statusCode < 500) {
networkError = [ALBBMANNetworkError ErrorWithHttpException4];
} else {
networkError = [ALBBMANNetworkError ErrorWithHttpException5];
}
// 默认网络异常附加信息
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
}
else {
//结束请求打点，bytes是下载的数据量大小
[builder requestEndWithBytes:data.length];
}
// 组装日志并发送
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[tracker send:[builder build]];
}
// 异步接口，相对同步接口的情况下,可以多统计到首字节,其他跟同步接口一样
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response {
// 首字节响应埋点
[_builder requestFirstBytes];
}
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data {
[_mutableData appendData:data];
}
- (void)connectionDidFinishLoading:(NSURLConnection *)connection {
// 结束请求打点
[_builder requestEndWithBytes:_mutableData.length];
// 组装日志并发送
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[tracker send:[_builder build]];
}
- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error {
ALBBMANNetworkError *networkError = [[ALBBMANNetworkError alloc] initWithErrorCode:[NSString stringWithFormat:@"%ld", (long)error.code]];
[_builder requestEndWithError:networkError];
// 组装日志并发送
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[tracker send:[_builder build]];
}
//由于NSURLConnection封装性,已然看不到TCP连接过程,所以统计不到建连
//时间,如果是自实现的Socket的情况下,可以额外在TCP连接成功的时刻调用如下代码,统计tcp建连时间
[builder connectFinished];
```

**说明：** 目前Method字段的value只支持POST和GET（不区分大小写）。

异常统计

网络请求无法避免异常情况，在埋点时，如果抛出异常打断了整个网络请求的流程，那么您是需要对此做额外处理的，否则将导致打点数据紊乱。处理的办法也很简便，您只需要在抓获异常以后选择网络默认异常类型和自定义网络异常类型的任意一种方式来标记这次请求发生了异常。

默认网络异常类型

|异常类型|对应接口|说明|
|:---|:---|:-|
|400<=HttpResponseCode<500|ErrorWithHttpException4|Http状态码4XX的错误|
|500<=HttpResponseCode|ErrorWithHttpException5|Http状态码5XX的错误|
|IO错误|ErrorWithIOInterrupted|请求的IO错误|
|Timeout错误|ErrorWithSocketTimeout|连接/请求超时的错误|

请参见上述示例。

使用说明：

```
ALBBMANNetworkError *networkError = [ALBBMANNetworkError ErrorWithHttpException4];
// 网络异常附加信息
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
```

自定义网络异常类型

如果默认网络异常类型无法满足您的需求，您可以自定义网络异常类型，但是网络异常类型编码要在1001 <= YouErrorCode <= 1010范围内，如果不进行前端配置将以错误编码（10xx）的形式进行展示，如果在前端配置错误编码对应的信息，就会展示您所配置的信息。

|异常类型|对应接口|说明|
|:---|:---|:-|
|自定义错误|initWithErrorCode|默认网络错误不可描述时，可自定义|

使用说明：

```
ALBBMANNetworkError *networkError = [[ALBBMANNetworkError alloc] initWithErrorCode:1001];
// 自定义网络异常附加信息
[networkError setProperty:@"IP" value:@"1.2.3.4"];
[builder requestEndWithError:networkError];
```

## 自定义性能事件埋点 {#section_spw_tkb_r2b .section}

自定义性能事件埋点可满足用户对定制化的性能事件进行实时监控的需求\(比如，Cache的读写耗时，私有网络协议的RPC耗时，客户端算法的耗时等）。

自定义性能事件可包含以下几内容：

-   事件名称（event\_label），只能由字母、数字和下划线组成（必选）。
-   事件从开始到完成消耗的时长（必选）。
-   事件所携带的属性（可选）。

**说明：** 自定义性能事件用于监控用户事件的时间开销，请参见下述代码实例正确传递参数。

```
ALBBMANCustomPerformanceHitBuilder *customPerfBuilder = [[ALBBMANCustomPerformanceHitBuilder alloc] init:@"HomeActivityInit"];
// 记录事件时间方式1，自定义性能事件开始
[customPerfBuilder hitStart];
...
// 自定义性能事件结束
[customPerfBuilder hitEnd];
// 设置定义性能事件持续时间，方式2
// [customPerfBuilder setDurationIntervalInMillis:1234];
// 设置扩展参数
[customPerfBuilder setProperty:@"Page" value:@"Home"];
// 组装日志并发送
ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
[traker send:[customPerfBuilder build]];
```

## 自定义事件 {#section_oj1_ykb_r2b .section}

自定义事件埋点可用于满足用户的定制化需求。

-   设置自定义事件的标签
    -   接口：

        ```
        - (void)setEventLabel:(NSString *)pEventId;
        ```

    -   功能：区分不同自定义事件的标签，同一种自定义事件的pEventId相同。
    -   入参：自定义事件标签，相当于自定义事件的业务ID，不能为空。
    -   是否必须调用：是。
    -   调用时机：创建ALBBMANCustomHitBuilder后，必须调用setEventLabel，否则调用build后返回为nil。
-   设置自定义事件的页面名称
    -   接口：

        ```
        - (void)setEventPage:(NSString *)pPageName;
        ```

    -   功能：设置该自定义事件发生在哪个页面。
    -   入参：自定义事件的页面名称，可以为空，这种情况日志中默认页面名称为UT。
    -   是否必须调用：否。
    -   调用时机：需要明确该自定义事件发生时的页面，不调用情况下默认为UT。
-   设置自定义事件停留时间
    -   接口：

        ```
        - (void)setDurationOnEvent:(long long)durationOnEvent;
        ```

    -   功能：设置自定义事件持续时间，和ALBBMANPageHitBuilder中的页面停留时间类似。
    -   入参：自定义事件停留时间。
    -   是否必须调用：否。
    -   调用时机：需要记录自定义事件的停留时间。
-   设置自定义事件扩展参数

    -   接口：

        ```
        - (void)setDurationOnEvent:(long long)durationOnEvent;
        ```

    -   功能：给单条日志添加一个扩展参数。
    -   入参：key和value都不能为nil，其中key不能为PAGE/EVENTID/ARG1/ARG2/ARG3/ARGS，否则build返回nil。
    -   是否必须调用：否。
    -   调用时机：需要给ALBBMANCustomHitBuilder实例添加扩展参数时调用。
    代码示例：

    ```
    ALBBMANCustomHitBuilder *customBuilder = [[ALBBMANCustomHitBuilder alloc] init];
    // 设置自定义事件标签
    [customBuilder setEventLabel:@"test_event_label"];
    // 设置自定义事件页面名称
    [customBuilder setEventPage:@"test_Page"];
    // 设置自定义事件持续时间
    [customBuilder setDurationOnEvent:12345];
    // 设置自定义事件扩展参数
    [customBuilder setProperty:@"ckey0" value:@"value0"];
    [customBuilder setProperty:@"ckey1" value:@"value1"];
    [customBuilder setProperty:@"ckey2" value:@"value2"];
    ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
    // 组装日志并发送
    NSDictionary *dic = [customBuilder build];
    [traker send:dic];
    ```


**说明：** 您可在控制台**自定义事件** \> **详细数据** \> **参数分析**中查看自定义事件扩展参数，查看前需要在**管理设置** \> **自定义事件管理**中添加要在控制台显示的事件ID。

## 如何实时验证数据是否正常上报 {#section_vrf_vlb_r2b .section}

1.  打开SDK的debug日志。

    ```
    ALBBMANAnalytics *man = [ALBBMANAnalytics getInstance];
    [man turnOnDebug];
    ```

2.  如果您在控制台输出窗口看到如下日志，即代表日志上传已经成功，稍后5分钟左右即可在您的MaxCompute表中看到相关数据。

    ```
    _uploadThroughTnetWithStream:logs:] line:502 
    上传成功: {
    allLength = 25;
    dataLen = 17;
    errorcode = 0;
    flag = 40;
    recvData = "{\"config\":{}}";
    type = 2;
    version = 2;
    }
    累计上传条数: 1
    ```


## H5页面数据的采集 {#section_kdr_nmb_r2b .section}

H5页面采集并没有单独的SDK，依赖native进行上传，通过JSBridge通知给native，然后调用MAN的相应方法，进行数据的上报。

代码示例：

H5进行自定义事件的上报，JavaScript代码如下：

```
// 通过iframe发起请求，然后在native端进行捕获。
var iframe = document.createElement("IFRAME");
// 通过自定义scheme，来判断是正常请求还是H5通信。
// 参数可以放在url中，然后在native解析。
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
NSURL *url = [request URL];
NSString *scheme = [url scheme];
// js通信
if ( [@"jsbridge" isEqualToString:scheme] ) {
if ( [@"custom" isEqualToString:url.host] ) {
ALBBMANCustomHitBuilder *customBuilder = [[ALBBMANCustomHitBuilder alloc] init];
// 设置自定义事件标签
[customBuilder setEventLabel:@"test_event_label"];
// 设置自定义事件页面名称
[customBuilder setEventPage:@"test_Page"];
// 设置自定义事件持续时间
[customBuilder setDurationOnEvent:12345];
// 设置自定义事件扩展参数
[customBuilder setProperty:@"ckey0" value:@"value0"];
[customBuilder setProperty:@"ckey1" value:@"value1"];
[customBuilder setProperty:@"ckey2" value:@"value2"];
ALBBMANTracker *traker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
// 组装日志并发送
NSDictionary *dic = [customBuilder build];
[traker send:dic];
}
return NO;
}
return YES;
}
```

