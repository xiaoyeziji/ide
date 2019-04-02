# 在AppStudio中部署二方包 {#concept_o2b_q2b_hhb .concept}

目前，WebIDE前端没有发布二方包的按钮或入口，您可通过Terminal的Shell功能进行二方包发布。

## 选择main函数 {#section_u35_t2b_hhb .section}

如果工作空间中没有main函数，您可以在工作空间中创建一个main函数，配置完成后再删除main函数即可。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/151808/155417554842198_zh-CN.png)

## 运行程序 {#section_m4c_bfb_hhb .section}

分配容器需要运行程序，只要机器启动完成，即可进入Terminal。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/151808/155417554842199_zh-CN.png)

## 进入代码目录 {#section_ur4_gfb_hhb .section}

进入source目录，通常最长的目录名便是用户的代码工程目录。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/151808/155417554942200_zh-CN.png)

## 在pom.xml中增加配置 {#section_qcz_kfb_hhb .section}

通常在本地mvn deploy只能部署SNAPSHOT包，您需要在pom中执行需要往哪个仓库去部署，WebIDE提供的仓库不允许直接deploy，需要在pom中增加依赖，如下所示，parent会指定部署的远程仓库地址。

```
<parent>
    <groupId>com.taobao</groupId>
    <artifactId>parent</artifactId>
    <version>2.0.0</version>
</parent>
```

## 执行deploy {#section_bxv_rfb_hhb .section}

由于已经在容器中设置了mvnrepo的snapshot用户名和密码，您可以直接mvn deploy，当出现build success 字样时即标志着部署成功。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/151808/155417554942202_zh-CN.png)

