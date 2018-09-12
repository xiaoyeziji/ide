# Baseline is broken. Why not call the alarm? {#concept_dtj_1xs_42b .concept}

The monitoring of a baseline with the baseline function enabled is targeted for tasks. If all tasks of the baseline are normal, no alert is reported even if the baseline is broken because Intelligent Monitor cannot judge which task has an error.

The possible causes for baseline breakage while tasks are normal are as follows.

-   The baseline time is set improperly.
-   The task dependency is incorrect, and no alert is reported even if the baseline is broken.

