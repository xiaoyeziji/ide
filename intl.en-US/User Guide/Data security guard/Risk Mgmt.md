# Risk Mgmt {#concept_gkk_2s3_r2b .concept}

The **Risk Mgmt** page provides the risk data rule configuration, you can identify risks in your daily visits and start AI to identify data risks automatically. The identified risk data is displayed on the [Data risk page](intl.en-US/User Guide/Data security guard/Manual ajust.md#) and audited, it also marks the data at the data access page.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17061/15475224098843_en-US.png)

The page description is as follows:

-   Risk identification management: divided into risk rule configuration and AI identification. Ai-aware pages include personal information queries, similar SQL queries, and identification descriptions of these two pieces. You only need to start it in the Status column. It can also be turned off after startup \(no previously identified data is deleted \).
-   Risk Rule Configuration \_ new rule: after you enter the rule name, owner, and rule note information in the dialog box, the rule basic information is created.
-   Risk Rule Configuration \_ actions: provides the ability to copy rules, edit risk rule entries, and delete rules.
-   Risk Rule Configuration \_ rule item configuration: provides project \(Multi-select Enabled\), type \(Multi-select supported\), rules \(Multi-selection support\), grading \(Multi-selection support\), export method \(Multi-selection Support\), tables \(supports fuzzy/exact matching\), fields \(supports fuzzy/precise matching\), accessor \(supports fuzzy/exact matching\), the amount of operation data, and the access time condition configuration.
-   Risk Rule Configuration \_ Status: After you have configured the rule, you need to take effect after the Status column starts the rule.

**Note:** Risk identification management data needs to follow the rules configured as well as AI identification, data takes effect on the page by t+1.

