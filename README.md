# Introduction
This document is taken from my article on http://www.martynsimpson.com/2019/04/reverse-engineering-hozelock-cloud-controller/.

I am in no way affiliated with Hozelock.

# Overview
I recently purchased the Hozelock Cloud Controller to automatically water my garden. While trying to integrate it with OpenHab I discovered various things about the REST API Service that are concerning and useful. – https://www.hozelock.com/product/cloud-controller/

# Basics
The Cloud Controller is made up of two parts the “Hub” which connects to your Router/ LAN, and the Cloud Controller which connects to the Hub (and is the bit that is connected to your tap.

## Hub
Usually it is just one Hub per "account" although I don't believe this is fact.

## Controllers
One or more controllers can be associated with a single hub (perhaps one for the front garden and one for the rear).

# Endpoints

## /{hubId}
Supports "GET"

Return the full payload for your hub.
```JSON
GET http://hoz3.com/restful/support/hubs/{hubId}
{
    "errorCode": 0,
    "hub": {
        "hubID": "{hubId}",
        "name": "{hubName}",
        "mode": "normal",
        "location": {
            "city": "{city}",
            "country": "{country}",
            "localTime": 1556221904283,
            "timezone": "Europe/London"
        },
        "schedules": [
            {
                "scheduleID": "{scheduleId}",
                "name": "{scheduleName}",
                "description": null,
                "scheduleDays": {
                    "Monday": {
                        "dayOfWeek": "Monday",
                        "wateringEvents": [
                            {
                                "startTime": -2000,
                                "endTime": 898000,
                                "duration": 900000,
                                "enabled": true
                            },
                            {
                                "startTime": -1000,
                                "endTime": 899000,
                                "duration": 900000,
                                "enabled": true
                            }
                        ]
                    },
                    .... removed
                }
            }
        ],
        "controllers": [
            {
                "name": "{controllerName}",
                "image": null,
                "controllerID": "0",
                "scheduleID": "{scheduleId}",
                "schedule": {
                    "scheduleID": "{scheduleId",
                    "name": "{scheduleName}",
                    "description": null,
                    "scheduleDays": {
                        "Monday": {
                            "dayOfWeek": "Monday",
                            "wateringEvents": [
                                {
                                    "startTime": -2000,
                                    "endTime": 898000,
                                    "duration": 900000,
                                    "enabled": true
                                },
                                {
                                    "startTime": -1000,
                                    "endTime": 899000,
                                    "duration": 900000,
                                    "enabled": true
                                }
                            ]
                        },
                        "Thursday": {
                            "dayOfWeek": "Thursday",
                            "wateringEvents": [
                                {
                                    "startTime": -2000,
                                    "endTime": 898000,
                                    "duration": 900000,
                                    "enabled": true
                                },
                                {
                                    "startTime": -1000,
                                    "endTime": 899000,
                                    "duration": 900000,
                                    "enabled": true
                                }
                            ]
                        }
                        .... removed
                    }
                },
                "hasWaterNowEvent": false,
                "pause": null,
                "adjustment": null,
                "waterNowEvent": null,
                "currentWateringEvent": null,
                "nextWateringEvent": {
                    "startTime": 1556219700000,
                    "endTime": 1556220600000,
                    "duration": 900000,
                    "enabled": true
                },
                "lastCommunicationWithServer": 1556217783000,
                "nextCommunicationWithServer": 1556219160000,
                "batteryStatus": "OK",
                "signalStrength": "GOOD",
                "overrideScheduleDuration": null,
                "isChildlockEnabled": false,
                "isWatering": false,
                "isPanelRemoved": false,
                "isTested": true,
                "isAdjusted": false,
                "isScheduleUpToDate": true,
                "isPaused": false
            }
        ],
        "inPairingMode": false,
        "lastServerContactDate": 1556217783000,
        "hubResetRequired": false,
        "controllerResetRequired": false,
        "isUresponsive": false
    }
}
```

## /{hubId}/schedules

Supports "GET"

Returns an array of all Schedules for the Hub. Note: This does NOT mean the schedule is applied to the controller. (Sample below with some days removed). This example is set to water at Sunrise and Sunset hence two entries per day.

```JSON
GET https://hoz3.com/restful/support/hubs/{hubId}/schedules
[
     {
         "scheduleID": "{scheduleId}",
         "name": "{scheduleName}",
         "description": null,
         "scheduleDays": {
             "Monday": {
                 "dayOfWeek": "Monday",
                 "wateringEvents": [
                     {
                         "startTime": -2000,
                         "endTime": 898000,
                         "duration": 900000,
                         "enabled": true
                     },
                     {
                         "startTime": -1000,
                         "endTime": 899000,
                         "duration": 900000,
                         "enabled": true
                     }
                 ]
             },
             "Thursday": {
                 "dayOfWeek": "Thursday",
                 "wateringEvents": [
                     {
                         "startTime": -2000,
                         "endTime": 898000,
                         "duration": 900000,
                         "enabled": true
                     },
                     {
                         "startTime": -1000,
                         "endTime": 899000,
                         "duration": 900000,
                         "enabled": true
                     },
                    ....... removed
                 ]
             }
         }
     }
 ]
```

## /{hubId}/schedules/{scheduleId}

Supports "DELETE", "GET", "HEAD", "PATCH", "POST", "PUT", "OPTIONS"

Used to manage a specific schedule from the schedule array by appending the scheduleID to the URL.

```JSON
GET https://hoz3.com/restful/support/hubs/{hubId}/schedules/{scheduleId}
{
    "schedule": {
        "scheduleID": "{scheduleId}",
        "name": "{scheduleName}",
        "description": null,
        "scheduleDays": {
            "Monday": {
                "dayOfWeek": "Monday",
                "wateringEvents": [
                    {
                        "startTime": -2000,
                        "endTime": 898000,
                        "duration": 900000,
                        "enabled": true
                    },
                    {
                        "startTime": -1000,
                        "endTime": 899000,
                        "duration": 900000,
                        "enabled": true
                    }
                ]
            },
            "Thursday": {
                "dayOfWeek": "Thursday",
                "wateringEvents": [
                    {
                        "startTime": -2000,
                        "endTime": 898000,
                        "duration": 900000,
                        "enabled": true
                    },
                    {
                        "startTime": -1000,
                        "endTime": 899000,
                        "duration": 900000,
                        "enabled": true
                    }
                ]
            },
            ... removed
        }
    },
    "errorCode": 0
}
 ```


## /{hubId}/controllers
Supports 

Retrieve all controllers

```JSON
GET https://hoz3.com/restful/support/hubs/{hubId}/controllers
{
         "controllers": [
             {
                 "name": "{controllerName}",
                 "image": null,
                 "controllerID": "0",
                 "scheduleID": "{scheduleId}",
                 "schedule": {
                     "scheduleID": "{scheduleId}",
                     "name": "{scheduleName}",
                     "description": null,
                     "scheduleDays": {
                         "Monday": {
                             "dayOfWeek": "Monday",
                             "wateringEvents": [
                                 {
                                     "startTime": -2000,
                                     "endTime": 898000,
                                     "duration": 900000,
                                     "enabled": true
                                 },
                                 {
                                     "startTime": -1000,
                                     "endTime": 899000,
                                     "duration": 900000,
                                     "enabled": true
                                 }
                             ]
                         },
                         ....... removed
                         }
                     }
                 },
                 "hasWaterNowEvent": false,
                 "pause": null,
                 "adjustment": null,
                 "waterNowEvent": null,
                 "currentWateringEvent": null,
                 "nextWateringEvent": {
                     "startTime": 1556219700000,
                     "endTime": 1556220600000,
                     "duration": 900000,
                     "enabled": true
                 },
                 "lastCommunicationWithServer": 1556211067000,
                 "nextCommunicationWithServer": 1556212140000,
                 "batteryStatus": "OK",
                 "signalStrength": "GOOD",
                 "overrideScheduleDuration": null,
                 "isChildlockEnabled": false,
                 "isWatering": false,
                 "isPanelRemoved": false,
                 "isTested": true,
                 "isAdjusted": false,
                 "isScheduleUpToDate": true,
                 "isPaused": false
             }
         ],
         "inPairingMode": false,
         "lastServerContactDate": 1556211067000,
         "hubResetRequired": false,
         "controllerResetRequired": false,
         "isUresponsive": false
     }
 }
```

## /{hubId}/controllers/{controllerId}

Supports "GET", "HEAD" and "OPTIONS".

```JSON
GET https://hoz3.com/restful/support/hubs/{hubId}/controllers/{controllerId}/

{
    "errorCode": 0,
    "controller": {
        "name": "{controllerName}",
        "image": null,
        "controllerID": "0",
        "scheduleID": "{scheduleId}",
        "schedule": {
            "scheduleID": "{scheduleId}",
            "name": "{scheduleName}",
            "description": null,
            "scheduleDays": {
                "Monday": {
                    "dayOfWeek": "Monday",
                    "wateringEvents": [
                        {
                            "startTime": -2000,
                            "endTime": 898000,
                            "duration": 900000,
                            "enabled": true
                        },
                        {
                            "startTime": -1000,
                            "endTime": 899000,
                            "duration": 900000,
                            "enabled": true
                        }
                    ]
                }
                ... removed
            }
        },
        "hasWaterNowEvent": false,
        "pause": null,
        "adjustment": null,
        "waterNowEvent": null,
        "currentWateringEvent": null,
        "nextWateringEvent": {
            "startTime": 1556219700000,
            "endTime": 1556220600000,
            "duration": 900000,
            "enabled": true
        },
        "lastCommunicationWithServer": 1556218742000,
        "nextCommunicationWithServer": 1556220120000,
        "batteryStatus": "OK",
        "signalStrength": "GOOD",
        "overrideScheduleDuration": null,
        "isChildlockEnabled": false,
        "isWatering": false,
        "isPanelRemoved": false,
        "isTested": true,
        "isAdjusted": false,
        "isScheduleUpToDate": true,
        "isPaused": false
    },
    "currentTime": 1556219227627
}
```

## /{hubId}/controllers/actions/
Supports "GET"

This section is pretty much entirely credited to anthonyangel (see credits)

An undocumented part of the API’s called Actions exists which allow you to submit tasks for the Hub (and thus controller) to pick up.

```JSON
GET http://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/

{
     "errorCode": 0,
     "actions": [
         "pause",
         "unpause",
         "adjust",
         "unadjust",
         "waterNow",
         "stopWatering",
         "setMode",
         "ping"
     ]
 }
 ```

## /{hubId}/controllers/actions/waterNow

Will issue a waterNow command for {duration} in milliseconds

```JSON
POST http://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/waterNow

Request Body
{
     "controllerIDs":[{controllerId}],
     "duration":{duration}
 }

Response
{
     "errorCode": 0
 }
 ```


## /{hubId}/controllers/actions/Pause
Will pause watering schedule for {pauseDuration} number of milliseconds

```JSON
POST http://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/pause

Request Body
{
     "controllerIDs":[{controllerId}],
     "duration":{pauseDuration}
 }

Response
{
    "errorCode": 0
}
```

## /{hubId}/controllers/actions/Unpause 
Will remove a Pause for a specified controller

```JSON
POST http://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/unpause

Request Body
{
     "controllerIDs":[0]
 }

Response
{
    "errorCode": 0
 }

```

## /{hubId}/controllers/actions/Adjust
Make a temporary percentage adjustment {percentage} to the controller/s {controlerId} for {duration} in milliseconds

Note: Percentage can be signed (positive or negative) but should NOT include the % symbol.

```JSON
POST http://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/adjust

Request Body
{
     "controllerIDs":[{controlerId}],
     "duration":{duration},
     "wateringAdjustment":{percentage}
 }

Response
{
    "errorCode": 0
}
```

## /{hubId}/controllers/actions/UnAdjust
Used to remove the adjustment.

```JSON
POST https://hoz3.com/restful/support/hubs/{hubId}/controllers/actions/unadjust

Request Body
{
     "controllerIDs":[{controllerId}]
 }

Response
{
    "errorCode": 0
}
```

## /{hubId}/controllers/actions/stopWatering
Stops the watering

TODO - Document
## /{hubId}/controllers/actions/setMode
Not sure but is expecting “mode” parameter

TODO - Document
## /{hubId}/controllers/actions/ping
Despite ping being listed as available if you issue a POST or a GET the response says that ping is not implemented.

TODO - Investigte more.

# Credits
Original inspiration to look into this comes from https://community.home-assistant.io/t/having-hozelock-cloud-controller-kit-intergration/55694/3
anthonyangel.

