# Stream Studio常见问题 {#concept_lml_cdg_xgb .concept}

本文为您介绍使用Stream Studio可能会遇到的常见问题。

Q：使用Stream Studio之前需要开通什么计算引擎服务？

A：Stream Studio是开发平台，基于阿里云实时计算服务构建，因此需要首先开通[实时计算](../../../../../cn.zh-CN/产品简介/什么是阿里云实时计算.md#)服务。

Q：Stream Studio支持哪些集群模式的实时计算服务？

A：共享模式和独享模式都支持。

Q：Stream Studio在共享模式和独享模式的支持上是否有功能区别？

A：有区别。出于安全考虑，对于共享模式不支持UDF。独享模式支持UDF。如果你对性能和功能要求较高，建议采用独享模式。

Q：实时计算项目在哪里创建？如何与Stream Studio关联上？

A：实时计算项目在实时计算控制台中创建，创建好之后可以到DataWorks控制台的工作空间列表中绑定到已有工作空间中，或者直接创建新的工作空间并绑定实时计算项目。绑定好项目之后，就可以在Stream Studio中开发任务了。

Q：Stream Studio中的DAG开发模式有什么优势，与SQL有什么异同？

A：Stream Studio支持可视化DAG和SQL两种开发模式。使用DAG开发模式是所见即所得的，无需编写代码，以拖拽组件的方式就可以完成任务开发，简单快捷。同时DAG工作流可以与SQL互转。你可以自由选择。

Q：Stream Studio支持哪种类型的SQL？

A：阿里云实时计算引擎是基于Flink构建的，因此Stream Studio支持Flink SQL。

