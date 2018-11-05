# Configure Stream Writer {#concept_okj_c24_q2b .concept}

In this article we will show you the data types and parameters supported by Stream Writer and how to configure Writer in script mode.

The Stream Writer plug-in provides the ability to read data from Reader and print data on the screen or directly discard data. It is mainly applicable to performance testing for data synchronization and basic functional testing.

## Parameter description {#section_a3m_424_q2b .section}

-   Print
    -   Description: Whether to print the outputted data on the screen.
    -   Required: No
    -   Default value: true.

## Development in wizard mode {#section_bhw_rj5_q2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_pcz_fh4_q2b .section}

Configure a job to read data from the Reader and print the data on the screen:

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

