# Permission list {#concept_i53_vfm_hfb .concept}

DataWorks provides seven roles for project owners \(non-authorizable\), project administrators, development, operations, deployment, guest, and Security Administrators. This article will introduce you to the permissions descriptions for specific roles.

## Data Management {#section_ssf_nhh_kfb .section}

|Permission Point|Owner|Administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:------------|:----------|:--|:-----|:------|:---------------------|
|Delete tables created by one-self|√|√|√|N/A|N/A|N/A|N/A|
|Settings of table categories by one-self|√|√|√|N/A|N/A|N/A|N/A|
|View your own collection of tables|√|√|√|N/A|N/A|N/A|N/A|
|New table|√|√|√|N/A|N/A|N/A|N/A|
|Unhide the table you created|√|√|√|N/A|N/A|N/A|N/A|
|Self-created table structure changes|√|√|√|N/A|N/A|N/A|N/A|
|Self-created table view|√|√|√|N/A|N/A|N/A|N/A|
|Viewing the content of the Right applied by one-self|√|√|√|N/A|N/A|N/A|N/A|
|Self-created table Hiding|√|√|√|N/A|N/A|N/A|N/A|
|Self-created table lifecycle settings|√|√|√|N/A|N/A|N/A|N/A|
|Non-self-created table data permission application|√|√|√|N/A|N/A|N/A|N/A|
|Update table|√|√|√|√|√|N/A|N/A|
|Delete a table|√|√|√|N/A|N/A|N/A|N/A|

## **release management** {#section_e5c_phh_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|Create a publishing package|√|√|√|√|N/A|N/A|N/A|
|View the Publishing Package List|√|√|√|√|√|√|N/A|
|Delete package|√|√|√|√|N/A|N/A|N/A|
|Perform publish|√|√|N/A|√|√|N/A|N/A|
|see release package content|√|√|√|√|√|√|N/A|

## button control {#section_qjj_g3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|button-stop|√|√|√|N/A|N/A|N/A|N/A|
|button-format|√|√|√|N/A|N/A|N/A|N/A|
|button-Edit|√|√|√|N/A|N/A|N/A|N/A|
|button-run|√|√|√|N/A|N/A|N/A|N/A|
|button-Amplification|√|√|√|N/A|N/A|N/A|N/A|
|button-save|√|√|√|N/A|N/A|N/A|N/A|
|button-expand/collapse|√|√|√|N/A|N/A|N/A|N/A|
|button-delete|√|√|√|N/A|N/A|N/A|N/A|

## Code development {#section_vrz_h3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|Save submitted code|√|√|√|N/A|N/A|N/A|N/A|
|view code content|√|√|√|√|√|√|N/A|
|Create Code|√|√|√|N/A|N/A|N/A|N/A|
|Delete Code|√|√|√|N/A|N/A|N/A|N/A|
|view code list|√|√|√|√|√|√|N/A|
|run code|√|√|√|N/A|N/A|N/A|N/A|
|modify code|√|√|√|N/A|N/A|N/A|N/A|
|Your files download|√|√|√|N/A|N/A|N/A|N/A|
|Your files upload|√|√|√|N/A|N/A|N/A|N/A|

## Function development {#section_lkw_33h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|View function details|√|√|√|√|√|√|N/A|
|Create Function|√|√|√|N/A|N/A|N/A|N/A|
|query function|√|√|√|√|√|√|N/A|
|delete function|√|√|√|N/A|N/A|N/A|N/A|

## Node Type Control {#section_gpt_k3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|node-PAI|√|√|√|N/A|N/A|N/A|N/A|
|Node -- MR|√|√|√|N/A|N/A|N/A|N/A|
|node-CDP|√|√|√|N/A|N/A|N/A|N/A|
|Node -- sql|√|√|√|N/A|N/A|N/A|N/A|
|Node -- xlib|√|√|√|√|√|√|N/A|
|node-Shell|√|√|√|N/A|N/A|N/A|N/A|
|node-virtual node|√|√|√|√|√|√|N/A|
|node-script\_seahawks|√|√|√|N/A|N/A|N/A|N/A|
|node-dtboost\_analytic|√|√|√|N/A|N/A|N/A|N/A|
|Node -- dtboost\_recommand|√|√|√|N/A|N/A|N/A|N/A|
|Node -- pyodps|√|√|√|N/A|N/A|N/A|N/A|

## Resources Management {#section_yl5_l3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|view resources list|√|√|√|√|√|√|N/A|
|delete Resources|√|√|√|N/A|N/A|N/A|N/A|
|create resources|√|√|√|N/A|N/A|N/A|N/A|
|Upload jar your files|√|√|√|N/A|N/A|N/A|N/A|
|Upload taxt your files|√|√|√|N/A|N/A|N/A|N/A|
|Upload archive your files|√|√|√|N/A|N/A|N/A|N/A|

## workflow development {#section_ip3_m3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|Run/Stop Workflow|√|√|√|N/A|N/A|N/A|N/A|
|save workflow|√|√|√|N/A|N/A|N/A|N/A|
|View workflow content|√|√|√|√|√|√|N/A|
|Submitted Node Code|√|√|√|N/A|N/A|N/A|N/A|
|Modify Workflow|√|√|√|N/A|N/A|N/A|N/A|
|View workflow list|√|√|√|√|√|√|N/A|
|Modify the Owner property|√|√|N/A|N/A|N/A|N/A|N/A|
|Open Node Code|√|√|√|N/A|N/A|N/A|N/A|
|delete Workflow|√|√|√|N/A|N/A|N/A|N/A|
|create workflow|√|√|√|N/A|N/A|N/A|N/A|
|Create folder|√|√|√|N/A|N/A|N/A|N/A|
|delete folder|√|√|√|N/A|N/A|N/A|N/A|
|Modify folder|√|√|√|N/A|N/A|N/A|N/A|

## Data Integration {#section_kpl_n3h_kfb .section}

|Permission Point|Owner|Project administrator|Development|O&M|Deploy|Visitor|Security Administrator|
|:---------------|:----|:--------------------|:----------|:--|:-----|:------|:---------------------|
|Data Integration-node Edit|√|√|√|N/A|N/A|N/A|N/A|
|Data Integration-node View|√|√|√|N/A|N/A|N/A|N/A|
|Data Integration-node Delete|√|√|√|N/A|N/A|N/A|N/A|
|project resources consumption monitoring menu|√|√|N/A|N/A|N/A|N/A|N/A|
|Project synchronous Resources Management menu|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group list|√|√|√|√|√|√|N/A|
|Project synchronous Resources Group create|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group Management machine list|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group Add Machine|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group Delete Machine|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group modify Machine|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group get Resources Group AK|√|√|√|√|√|N/A|N/A|
|Project synchronous Resources Group Delete|√|√|√|√|√|N/A|N/A|
|project resources consumption monitoring|√|√|N/A|N/A|N/A|N/A|N/A|
|Operation and Maintenance Center task modify Resources Group|√|√|√|√|√|N/A|N/A|
|Synchronous task list menu|√|√|√|√|√|N/A|N/A|
|The task is moved script|√|√|√|√|√|N/A|N/A|
|Get project members list|√|√|√|√|√|N/A|N/A|
|New Code Interface|√|√|√|√|√|N/A|N/A|
|Save/update code Interface|√|√|√|√|√|N/A|N/A|
|According to fileId get code Interface|√|√|√|√|√|√|N/A|
|Get Data Integrated node list|√|√|√|√|√|N/A|N/A|
|Search table Interfaces|√|√|√|√|√|N/A|N/A|
|search field interface|√|√|√|√|√|N/A|N/A|
|query data source list Interface|√|√|√|√|√|√|N/A|
|new data source interface|√|√|N/A|N/A|N/A|N/A|N/A|
|query data source details Interface|√|√|√|√|√|N/A|N/A|
|update data source interface|√|√|N/A|N/A|N/A|N/A|N/A|
|delete data source interface|√|√|N/A|N/A|N/A|N/A|N/A|
|Test connectivity|√|√|√|√|√|N/A|N/A|
|Data Preview|√|√|√|√|√|N/A|N/A|
|Check whether open OTSStream|√|√|√|√|√|N/A|N/A|
|Open Table Store|√|√|√|√|√|N/A|N/A|
|Query ODPS table building statement|√|√|√|√|√|N/A|N/A|
|New ODPS table|√|√|√|√|√|N/A|N/A|
|Query ODPS table status|√|√|√|√|√|N/A|N/A|
|Migration Database Table|√|√|N/A|N/A|N/A|N/A|N/A|

