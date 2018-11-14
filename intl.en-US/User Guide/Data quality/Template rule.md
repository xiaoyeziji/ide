# Template rule {#concept_w42_3r3_r2b .concept}

Currently, Data Quality Center \(DQC\) has 36 template rules every of which is described in this article.

## **Fluctuation calculation** {#section_acg_pr3_r2b .section}

`Fluctuation=(Sample - Reference value)/Reference value`

## Fluctuation variance calculation {#section_qd2_wr3_r2b .section}

\(Current sample - historical N-day average values\) / standard deviation

## Glossary {#section_yjl_xr3_r2b .section}

-   Sample: The value of the specific samples collected per day, such as the number of rows in the SQL task table, one-day fluctuation detection. Sample is the number of partitions of the table in the current day.
-   Reference value: Comparison of historical samples.
    -   For example, rule is the number of rows in the SQL task table and one-day fluctuation detection, then the reference value is the number of partitions of the table generated in the previous day.
    -   For example, rule is the number of rows in the SQL task table and seven-day fluctuation detection, then the reference value is the average data value in rows of the table for the previous seven days.

## Verification logic {#section_fmn_zr3_r2b .section}

Currently, Data Quality only supports **Fluctuation detection value** and **Comparison of fixed** value verification methods.

|Verification method|Verification logic|
|Fluctuation detection value| -   If the absolute value of the check value is less than or equal to the orange threshold, it returns normal.
-   If the absolute value of the check value does not meet the first condition and is less than or equal to the red threshold, orange alarm is triggered.
-   If the check value does not meet the second condition, red alarm is triggered.
-   If there is no orange threshold, only two cases are possible: red alarm and normal.
-   If there is no red threshold, only two cases are possible: orange alarm and normal.
-   If none of them is filled, red alert is triggered, as the front end is not allowed to leave two thresholds blank.

 |
|Comparison of fixed value| -   According to the check expression, calculate s opt expect, returns Boolean value, opt supports greater than, less than, equal to, greater than or equal to, less than or equal to, not equal to.
-   According to the preceding formula, if true, it returns normal, otherwise, red alarm is triggered.

 |

## Template rule {#section_k3x_fs3_r2b .section}

|Template level|Template name|Description|
|1|The average value of the field, fluctuation compared to the one day, one week, one month before.|Take the average value of this field, compare with the one-day, seven-day, one-month period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|2|The summary value of the field, fluctuation compared to the one day, one week, one month before.|Take the sum value of this field, compare with the one-day, seven-day, one-month period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|3|The minimum value of the field, fluctuation compared to the one day, one week, one month before.|Take the minimum value of this field, compare with the one-day, seven-day, one-month period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|4|The maximum value of the field, fluctuation compared to the one day, one week, one month before.|Take the maximum value of this field, compare with the one-day, seven-day, one-month period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|5|The number of unique values in the field.|Count the number after removing duplicates, then compare with an expected number, that is, fixed value verification.|
|6|The number of unique values in the field, volatility compared to the one day, one week, one month before.|Count the number after removing duplicates, compare with one day, one week, one month, that is, fixed value verification.|
|7|The number of rows in the table, fluctuation compared to the one day, one week, one month before.|Compare the number of rows in the table collected one day, one week, and one month before, and compare the fluctuation.|
|8|The number of null values in the field.|The number of null values in this field compare to the fixed value.|
|9|The number of null values in the field / Total number of rows.|Calculate the number of null values and the total number of rows to get a rate, then compare with a fixed value. Note: The fixed value is a decimal.|
|10|The number of duplications in the field / Total number of rows.|The rate of the number of repeated values to the total number of rows, then compare with a fixed value.|
|11|The number of duplicated values in the field.|The total number of rows minus the number after removing duplicates \(that is the number of duplicated values in the field\), and the number of duplicated values compared to the fixed value.|
|12|The number of unique values in the field / Total number of rows.|The rate of the number of unique values to the total number of rows, then compare with a fixed value.|
|13|The average value of the field, fluctuation compared to the one day before.|Take the average value of the field, compare with the previous period. Calculate the fluctuation, then compare with a threshold value.|
|14|The summary value of the field, fluctuation compared to the one day before.|Take the sum value of this field, compare with the previous period. Calculate the fluctuation, then compare with a threshold value.|
|15|The minimum value of the field, fluctuation compared to the one day before.|Take the maximum value of this field, compare it to the one day before. Calculate the volatility, then compare with a threshold value.|
|16|The maximum value of the field, fluctuation compared to the one day before.|Take the maximum value of this field, compare it to the one day before, calculate the fluctuation, then compare with a threshold value.|
|17|The summary value of the field, fluctuation compared to the previous period.|Take the sum value of this field, compare it with the previous period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|18|The minimum value of the field, fluctuation compared to the previous period.|Take the minimum value of this field, compare it with the previous period, calculate the volatility. Then compare it with the threshold, if there is an alarm, it is triggered.|
|19|The maximum value of the field, fluctuation compared to the previous period.|Take the maximum value of this field, compare it with the previous period, calculate the fluctuation. Then compare it with the threshold, if there is an alarm, it is triggered.|
|20|Table size \(bytes\) is unchanged, compared to the previous period.|Table size \(bytes\) is unchanged, compared to the previous period.|
|21|Table size \(bytes\) has changed, compared to the previous period.|Table size \(bytes\) has changed, compared to the previous period.|
|22|The number of rows in the table has changed, compared to the previous period.|The number of rows in the table has changed, compared to the previous period.|
|23|The number of rows in the table is unchanged, compared to the previous period.|The number of rows in the table is unchanged, compared to the previous period.|
|24|Table size, difference value compared to the previous period \(bytes\).|Table size, difference value compared to the previous period \(bytes\).|
|25|The number of rows in the table, difference value compared to the previous period.|The reference value is the number of partitions of the table generated in the previous period. Compare to the number of table rows collected on the current day, then compare the difference value.|
|26|The number of rows in the table.|The number of rows in the table.|
|27|Table space size \(bytes\).|Table space size \(bytes\).|
|28|The number of rows in the table, difference value compared to one day before.|The reference value is the number of partitions of the table generated one day before. Compare to the number of table rows collected on the current day, then compare the difference value.|
|29|Table space size, difference value compared to one day before \(bytes\).|Table space size, difference value compared to one day before \(bytes\).|
|30|Table space size, fluctuation compared to the one day before.|The template is the fluctuation of the table size monitoring. The sample is compared with the quota sample of the previous day. If the orange threshold is 5% and the red threshold is 10%, the orange alarm is triggered when the fluctuation is greater than 5% and less than or equal to 10%. The red alarm is triggered when the orange threshold is greater than 10%.|
|31|Table space size, fluctuation compared to the one week before.|The template is the fluctuation of the table size monitoring. The sample is compared with the quota sample of the previous week. If the orange threshold is 5% and the red threshold is 10%, the orange alarm is triggered when the fluctuation is greater than 5% and less than or equal to 10%. The red alarm is triggered when the orange threshold is greater than 10%.|
|32|Table space size, fluctuation compared to the one month before.|The template is the fluctuation of the table size monitoring. The sample is compared with the quota sample of the previous month. If the orange threshold is 5% and the red threshold is 10%, the orange alarm is triggered when the fluctuation is greater than 5% and less than or equal to 10%. The red alarm is triggered when the orange threshold is greater than 10%.|
|33|The number of rows in the table, average fluctuation value compared to the last seven days.|The reference value is the average value of the number of table rows in the last seven days.|
|34|The number of rows in the table, average fluctuation value compared to the last thirty days.|The reference value is the average value of the number of table rows in the last thirty days.|
|35|The number of rows in the table, fluctuation compared to the one day before.|The reference value is the number of partitions of the table generated one day before. Compare to the number of table rows collected on the day, then compare the fluctuation.|
|36|The number of rows in the table, fluctuation compared to the one week before.|The reference value is the number of partitions of the table generated one week before. Compare to the number of table rows collected on the current day, then compare the fluctuation.|
|37|The number of rows in the table, fluctuation compared to the one month before.|The reference value is the number of partitions of the table generated one month before. Compare to the number of table rows collected on the current day, then compare the fluctuation.|
|38|The number of rows in the table, the first day of the current month fluctuation compared to the one day, one week, one month before.|Compare the number of table rows collected on the first day of the current month to one day, one week, one month before, and compare the fluctuation.|
|39|The number of rows in the table, fluctuation compared to the previous period.|The reference value is the number of partitions of the table generated in the previous period. Compare to the number of table rows collected on the current day, and compare the fluctuation.|
|40|Discrete value monitoring \(number of packets\)|The number of packets is compared with a fixed value.|
|41|Discrete value monitoring \(group number fluctuation\)|The number of divisions for fluctuation detection, one day, seven days, one month ago that day the number of groups is the benchmark.|
|42|Discrete value monitoring \(state value\)|As in select count \(\*\) from table group by table.id, the value of each group after grouping is compared to a certain number.|
|43|Discrete value monitoring \(state value and fluctuation of state value\)|Like select count \(\*\) from table group by table. id, it compares the value of each group after grouping with a certain number; and if the number of groupings increases, it will alarm, without alarming.|

