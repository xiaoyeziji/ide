# An error occurred when using username root to add MongoDB data source {#concept_ysp_skm_hfb .concept}

## Issue description {#section_ntd_4lm_hfb .section}

An error occurred when using username root to add MongoDB data source.

## Root cause {#section_xwx_vlm_hfb .section}

When adding the MongoDB data source, you must use the username created by the database where the table you are required to synchronize resides, instead of the root.

## Solution {#section_fbq_xlm_hfb .section}

For example, to import the name table, which is in the test database, enter test as the database name.

Enter the username created in a specific database, instead of root. For example, if the test database is specified, then use the account created in the test database as the username.

