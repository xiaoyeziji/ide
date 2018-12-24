# 使用用户名root添加MongoDB数据源报错 {#concept_ysp_skm_hfb .concept}

## 问题描述 {#section_ntd_4lm_hfb .section}

使用用户名root添加MongoDB数据源时报错。

## 问题原因 {#section_xwx_vlm_hfb .section}

添加MongoDB数据源时，使用的用户名必须是用户需要同步的这张表所在的数据库创建的用户名，不能用root。

## 解决方法 {#section_fbq_xlm_hfb .section}

例如需要导入name表，name表在test库，则此处数据库名称填写为test。

用户名为指定数据库中创建的用户名，不要使用root。例如之前指定的是test库，则用户名需使用test数据库中创建的账户。

