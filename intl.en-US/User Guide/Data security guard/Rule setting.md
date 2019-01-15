# Rule setting {#concept_dgr_wr3_r2b .concept}

## Defining sensitive data {#section_ufp_jw3_r2b .section}

The steps for the data security administrator are as follows:

1.  Logon Data Protection Platform.
2.  Navigate to **Rules Setting** \> **identification**, and click **New**.
3.  Complete the basic information in the dialog box, and click **Next**.Configurations:
    -   Data Type: that is, the classification to which the rule belongs, which supports adding by template or custom adding.
    -   Data name: 11 Sensitive data identification definition templates are built into the system, ID card, banking card number, mailbox, mobile phone number, IP, MAC address, fixed phone, license plate number, identification of company, address and name, user-defined rules are also provided.
    -   Owner: the rule sets the person information.
    -   Note: set additional information descriptions for this rule.
4.  Complete the configuration rules in the dialog box, and click **Next**.

    Configurations:

    -   **Classification**: rank the configured data, and if the existing level does not meet the requirements, please set up in the Grading Information Management Service.
    -   **Content scan**: One of the Data Recognition Methods provided, each of the 11 Data Recognition templates in the system is content scanned.
        -   If you select a template, you cannot change the recognition rule, but you are provided with a channel to verify the accuracy of the rule, at the same time, the recognition of the situation can be manually corrected.
        -   If you select regular match, the recognition rules are customized.
    -   **Meta Scan**: Provides the exact matching of Field Names and Fuzzy Matching methods to support multiple field matches, the relationship between the fields is or.
5.  When the settings are complete, click **Next**and save.
6.  If you need to modify an existing rule, you can click the **Configuration rules** that you want to manipulate, configure and modify advanced information.
7.  When the rule configuration is complete, click **Save**.
8.  After saving the rule is invalid, the change status takes effect after the confirmation rule is correct.

**Note:** When defining sensitive data, follow these rules.

-   The rule name must be unique.
-   Content or field scans for different rules must be unique.
-   Rules identify data, T + 1 is displayed in the report.

## Sensitive data defined {#section_cfh_w1j_r2b .section}

If you have defined sensitive data, jump directly to identify data distribution, data access behavior, and data export module features.

