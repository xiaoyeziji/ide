# Cross-project cloning {#concept_m24_w2z_gfb .concept}

After you successfully clone a task using the **Cross-Project Cloning** feature, the system will automatically alter the output name of each task to replicate or maintain dependencies between two nodes. This allows the system to distinguish different projects under the same Alibaba Cloud account.

## A comprehensive business flow cloning process {#section_vtd_wb1_hfb .section}

The output name project\_1.task\_1\_out of task\_A in Project\_1 will be renamed as project\_2.task\_out after it is cloned to Project\_2.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21853/155253331013009_en-US.jpg)

## Cross-project dependencies cloning {#section_j41_xb1_hfb .section}

By default, task\_ B in Project\_1 is dependent on task\_A in Project\_3. After you clone task\_B in Project\_1 to Project\_2, the dependencies between task\_B in Project\_1 and task\_A in Project\_3 are also cloned, which means task\_B in Project\_2 is still dependent on task\_A in Project\_3.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21853/155253331013011_en-US.jpg)

