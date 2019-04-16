# UT测试 {#concept_ebn_zf1_dhb .concept}

AppStudio目前支持UnitTest的运行，包括自动生成UT代码、UT测试的入口检测、UT代码的运行和运行结果的展示四大功能。

## 自动生成UT代码 {#section_xd2_hv4_fhb .section}

右键单击**方法**，选择**Generate** \> **Create Test**，即可在Test目录下自动生成测试类和测试代码。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521241735_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521241736_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521241744_zh-CN.png)

## UT测试入口检测 {#section_ojt_hg1_dhb .section}

**说明：** UT类需要写在src/test/java目录下，如果不在此目录下的Java UT类文件，将无法被正常识别成Java UT类。

完成Java类的创建后，在对应的测试用例方法上添加org.junit.Test的@Test注解即可。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521240893_zh-CN.png)

## UT代码运行 {#section_ycf_wg1_dhb .section}

单击页面上方的Run Test，UT的测试用例便可运行。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521240894_zh-CN.png)

## UT运行结果展示 { .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/139469/155417521340895_zh-CN.png)

