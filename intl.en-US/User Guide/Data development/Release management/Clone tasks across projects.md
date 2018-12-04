# Clone tasks across projects {#concept_m24_w2z_gfb .concept}

After you successfully clone a task using the **cross-project cloning** feature, the system will automatically alter the output name of each task to replicate or maintain the dependencies between two nodes. This allows the system to distinguish different projects under the same Alibaba Cloud account.

## A complete business flow cloning process {#section_vtd_wb1_hfb .section}

The output name project\_1.task\_1\_out of task\_A in Project\_1 will be renamed as project\_2.task\_out after it is cloned to Project\_2.

## Single task cloning {#section_bvl_wb1_hfb .section}

After you clone task\_B in Project\_1 to Project\_2, the system will automatically configure the root node of the new project as the parent node of the cloned task. The output name of the root node in Project\_2 will be the input name of the cloned task. This will automatically establish dependencies between task\_B and the root node of Project\_2 after task\_B is cloned to Project\_2 from Project\_1.

## Cross-project dependencies cloning {#section_j41_xb1_hfb .section}

By default, task\_ B in Project\_1 is dependent on task\_A in Project\_3. After you clone task\_B in Project\_1 to Project\_2, the dependencies between task\_B in Project\_1 and task\_A in Project\_3 are also cloned, which means task\_B in Project\_2 is still dependent on task\_A in Project\_3.

