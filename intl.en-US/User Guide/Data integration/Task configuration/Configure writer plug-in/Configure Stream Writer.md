# Configure Stream Writer {#concept_okj_c24_q2b .concept}

This topic describes the data types and parameters supported by Stream Writer and how to configure Writer in Script Mode.

The Stream Writer plug-in allows you to read data from the Reader and print data on the screen or discard data. It is primarily applied to testing, such as for data synchronization performance and basic functions.

## Parameter description {#section_a3m_424_q2b .section}

-   Print
    -   Description: Whether to print the output data on the screen.
    -   Required: No
    -   Default value: True

## Development in Wizard Mode {#section_bhw_rj5_q2b .section}

Currently, development in Wizard Mode is not supported.

## Development in Script Mode {#section_pcz_fh4_q2b .section}

Configure a job to read data from the Reader and print data on the screen as follows:

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "stream", //plug-in name
            "parameter":{
                "print": false, // do you want to print output to the screen?
                "fieldDelimiter": "," //Delimiter of each column
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record":"0"//Number of error records
        },
        "speed":{
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent":"1",//Number of concurrent tasks
            "dmu":1//DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

