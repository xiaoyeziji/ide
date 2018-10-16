# RDS同步失败转换成JDBC格式 {#concept_zgb_33b_r2b .concept}

## 问题描述 {#section_bhk_mhb_r2b .section}

从RDS（MySQL/SQL Server/PostgreSQL）同步到自建MySQL/SQL Server/PostgreSQL时，报错为：DataX无法连接对应的数据库。

## 解决方法 {#section_v3x_shb_r2b .section}

以 RDS（MySQL）的数据同步到自建SQL Server为例，操作如下：

1.  新建一个数据源，将数据源配置为MySQL\>JDBC 格式。
2.  使用新数据源配置同步任务，重新执行即可。

    **说明：** 如果是在RDS（MySQL）\>RDS（SQL Server）等云产品之间同步时，建议选择RDS（MySQL）\>RDS（SQL Server）数据源来配置同步任务。


