# 使用Shell执行DataX任务最佳实践 {#concept_r2s_j2s_xgb .concept}

本文将为您介绍如何使用Shell执行DataX任务。

## 前提条件 {#section_ekd_m2s_xgb .section}

使用Shell执行DataX任务前，您需要进行以下准备工作。

-   创建Shell节点任务，详情请参见[Shell节点](cn.zh-CN/使用指南/数据开发/节点类型/SHELL节点.md#)。
-   添加自定义资源组，详情请参见[新增任务资源](cn.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。

    **说明：** 请在**管理控制台** \> **调度资源列表**页面而不是数据集成页面添加自定义资源组。


## 操作步骤 {#section_mgn_3ks_xgb .section}

1.  双击创建的Shell节点，填写下述代码。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/132473/155123854239667_zh-CN.png)

    ```
    shell_datax_home='/home/admin/shell_datax'
    mkdir -p ${shell_datax_home}
    shell_datax_config=${shell_datax_home}/${ALISA_TASK_ID}
    echo '''
    {
        "job": {
            "setting": {
                "speed": {
                    "channel": 1
                },
                "errorLimit": {
                    "record": 0,
                    "percentage": 0.02
                }
            },
            "content": [
                {
                    "reader": {
                        "name": "streamreader",
                        "parameter": {
                            "column": [
                                {
                                    "value": "${bdp.system.bizdate}",
                                    "type": "string"
                                },
                                {
                                    "value": "${bdp.system.cyctime}",
                                    "type": "string"
                                },
                                {
                                    "value": "${params1}__${params2}",
                                    "type": "string"
                                },
                                {
                                    "value": 19890427,
                                    "type": "long"
                                },
                                {
                                    "value": "1989-06-04 00:00:00",
                                    "type": "date"
                                },
                                {
                                    "value": true,
                                    "type": "bool"
                                },
                                {
                                    "value": "test",
                                    "type": "bytes"
                                }
                            ],
                            "sliceRecordCount": 10
                        }
                    },
                    "writer": {
                        "name": "streamwriter",
                        "parameter": {
                            "print": true,
                            "encoding": "UTF-8"
                        }
                    }
                }
            ]
        }
    }
    ''' > ${shell_datax_config}
    
    params1=$1
    params2=$2
    datax_params='-p "-Dparams1=${params1} -Dparams2=${params2}"'
    echo "`date '+%Y-%m-%d %T'` shell datax config: ${shell_datax_config}"
    echo "`date '+%Y-%m-%d %T'` shell datax params: -p \"-Dparams1=${params1} -Dparams2=${params2}\""
    /home/admin/datax3/bin/datax.py ${shell_datax_config} -p "-Dparams1=${params1} -Dparams2=${params2}"
    shell_datax_run_result=$?
    
    rm ${shell_datax_config}
    
    if [${shell_datax_run_result} -ne 0]
    then
        echo "`date '+%Y-%m-%d %T'` shell datax ended failed :("
        exit -1
    fi
    echo "`date '+%Y-%m-%d %T'` shell datax ended success～"
    ```

    代码说明如下：

    -   生成临时datax配置文件（您只需修改配置文件内容即可，详情请参见[DataX文档](https://github.com/alibaba/DataX)。
    -   读取调度参数，分别为$1, $2。
        1.  不需配置$\{bdp.system.bizdate\}，$\{bdp.system.cyctime\}，参数详情请参见[参数配置](cn.zh-CN/使用指南/数据开发/调度配置/参数配置.md#)。
        2.  执行datax任务，进行数据同步。
        3.  删除临时文件。
        4.  判断任务成功或失败，进行返回，0代表成功。
2.  单击右侧的**调度配置**，进行系统参数的配置，详情请参见[调度配置](cn.zh-CN/使用指南/数据开发/调度配置/基本属性.md#)模块的文档。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/132473/155123854239669_zh-CN.png)

3.  配置完成后，提交并发布节点任务。
4.  进入**运维中心** \> **周期任务**页面，选择相应的节点修改自定义资源组。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/132473/155123854239679_zh-CN.png)

5.  单击相应节点后的**测试**，并查看执行结果。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/132473/155123854239680_zh-CN.png)


