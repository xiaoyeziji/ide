# Synchronous task waiting for slots {#concept_dzh_lhb_r2b .concept}

## Issue Description {#section_bhk_mhb_r2b .section}

The task is not functioning properly, and the log prompts the current instance that it has not yet generated log information, waiting for the slot.

## Root cause {#section_vkt_4hb_r2b .section}

The above prompts occur because the configuration schedule for the task uses a custom resource, however, there are currently no custom resources available.

## Solution {#section_v3x_shb_r2b .section}

1.  You can go to the **DataWorks** \> **operations center** \> **task operations** page, right-click tasks that are not scheduled as expected, select **view node properties** to view the resource groups used by the task.
2.  Go to the **Project Management** \> **scheduling Resource Management** page, locate the scheduling resource that the task uses, and click **server administration**, check to see if the status of the server is stopped or occupied by other tasks.
3.  If the above troubleshooting does not resolve the issue, you can restart the service by executing the following command.

    ```
    su - admin`
    /home/admin/alisatasknode/target/alisatasknode/bin/serverctl restart`
    ```


