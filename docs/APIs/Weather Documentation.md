# Dialog Weather API

## Content
* [Overview](#overview)
* [Method](#method)
* [Authorization](#authorization-api-calls)
* [Charging a subscriber](#charging-a-subscriber)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

 

## Overview

The Dialog Weather API allows you to get current status of the weather station by providing device ID and get the weather station status, sunrise details, weather forecast data and lightening alerts for given location. Location parameters given as latitude and longitude. This API also delivers weather forecast data for given District.


## Requirements

**Authorization API calls** 
All API call request to ideabiz.lk required Authorization headers. Please refer the *Token Management* (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) document for Authorization.

**Request Header**
```
Content-Type: application/json 
Authorization: Bearer [access token]
Accept: application/json 
```
### Method
```
POST
```

[https://ideabiz.lk/apicall/weather/v1/callActiondev](https://ideabiz.lk/apicall/weather/v1/callActiondev)


### Body
```
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getStatus",
     "actionParameters" : {
                                "lon":-116,
                                "lat":43,
                                "version":"v3"
     },
     "userId":57569
}
```

|Parameter Name|Description|Type|Mandatory/Optional|
|---|---|---|---|
|deviceId|This uniquely identifies the Earth                     networks Device|Integer|Mandatory|
|operation|This parameter is used to identify the device operation. User should send pre-defined phrase to do the operation within the device. Eg: deviceControl.|String|Mandatory|
|actionName|This parameter states the actions that can happen in a particular device. One device can has several actions and user can give the action that needed to be executed from API. Eg: getUVIndex, getSunInfo.|String|Mandatory|
|actionParameters|These action parameters are used to send required parameters to complete the action. Eg: lon, lat, version, period|String|Mandatory|
|userId|User Id uses to identify the user's time zone|String|Mandatory


### Response
```
{
    "deviceResponse": {
        "observation": {
            "key": "3_PLWTT",
            "stationId": "PLWTT",
            "providerId": 3,
            "observationTimeLocalStr": "2014-10-16T15:44:44",
            "observationTimeUtcStr": "2014-10-16T10:14:44",
            "iconCode": 202,
            "altimeter": null,
            "altimeterRate": null,
            "dewPoint": -1.4,
            "dewPointQc": "Q",
            "dewPointRate": null,
            "heatIndex": 73.7,
            "heatIndexQc": "Q",
            "humidity": 5,
            "humidityQc": "C",
            "humidityRate": -20,
            "humidityRateQc": "C",
            "pressureSeaLevel": 29.82,
            "pressureSeaLevelQc": "C",
            "pressureSeaLevelRate": 0.08,
            "pressureSeaLevelRateQc": "C",
            "rainDaily": 0.06,
            "rainDailyQc": "C",
            "rainRate": 0,
            "rainRateQc": "C",
            "rainMonthly": 0.19,
            "rainMonthlyQc": "C",
            "rainYearly": 0.19,
            "rainYearlyQc": "C",
            "snowDaily": null,
            "snowRate": null,
            "snowMonthly": null,
            "snowYearly": null,
            "temperature": 73.7,
            "temperatureQc": "C",
            "temperatureRate": -12.4,
            "temperatureRateQc": "C",
            "visibility": null,
            "visibilityRate": null,
            "windChill": 73.7,
            "windChillQc": "S",
            "windSpeed": 7.5,
            "windSpeedQc": "C",
            "windDirection": 47,
            "windDirectionQc": "C",
            "windSpeedAvg": 11,
            "windSpeedAvgQc": "C",
            "windDirectionAvg": 82,
            "windDirectionAvgQc": "C",
            "windGustHourly": 25,
            "windGustHourlyQc": "C",
            "windGustTimeLocalHourlyStr": "2014-10-16T15:41:00",
            "windGustTimeUtcHourlyStr": "2014-10-16T10:11:00",
            "windGustDirectionHourly": 54,
            "windGustDirectionHourlyQc": "C",
            "windGustDaily": 25,
            "windGustDailyQc": "C",
            "windGustTimeLocalDailyStr": "2014-10-16T15:42:00",
            "windGustTimeUtcDailyStr": "2014-10-16T10:12:00",
            "windGustDirectionDaily": 54,
            "windGustDirectionDailyQc": "C",
            "observationTimeAdjustedLocalStr": "2018-07-19T15:55:38",
            "wetBulbTemperature": 47.5,
            "wetBulbTemperatureQc": "S",
            "RuleDetails": null,
            "FinalDistance": 0,
            "FromHourlyForecastLocationId": null,
            "IconDescription": "80% Chance of Storms",
            "feelsLike": 73.7
        },
        "highlow": {
            "humidityHigh": 5,
            "humidityHighQc": "C",
            "humidityHighLocalStr": "2014-10-16T13:30:00",
            "humidityHighUtcStr": "2014-10-16T08:00:00",
            "humidityLow": 5,
            "humidityLowQc": "C",
            "humidityLowLocalStr": "2014-10-16T15:44:00",
            "humidityLowUtcStr": "2014-10-16T10:14:00",
            "pressureSeaLevelHigh": 29.9,
            "pressureSeaLevelHighQc": "C",
            "pressureSeaLevelHighLocalStr": "2014-10-16T08:23:00",
            "pressureSeaLevelHighUtcStr": "2014-10-16T02:53:00",
            "pressureSeaLevelLow": 29.73,
            "pressureSeaLevelLowQc": "C",
            "pressureSeaLevelLowLocalStr": "2014-10-16T14:22:00",
            "pressureSeaLevelLowUtcStr": "2014-10-16T08:52:00",
            "rainRateMax": 0.04,
            "rainRateMaxQc": "C",
            "rainRateMaxLocalStr": "2014-10-16T13:10:00",
            "rainRateMaxUtcStr": "2014-10-16T07:40:00",
            "temperatureHigh": 88.3,
            "temperatureHighQc": "C",
            "temperatureHighLocalStr": "2014-10-16T13:15:00",
            "temperatureHighUtcStr": "2014-10-16T07:45:00",
            "temperatureLow": 73.6,
            "temperatureLowQc": "C",
            "temperatureLowLocalStr": "2014-10-16T03:54:00",
            "temperatureLowUtcStr": "2014-10-15T22:24:00"
        },
        "station": {
            "StationId": "PLWTT",
            "ProviderId": 3,
            "ProviderName": "Earth Networks Inc",
            "StationName": "The Overseas School of Colombo",
            "Latitude": 6.8925,
            "Longitude": 79.9294444444444,
            "ElevationAboveSeaLevel": 72
        }
    }
}
```
### Response Codes

```
200 – Success
417 – Bad request; check the error message for details
500 – Internal server error

```

## Get Current status 


#### Description

```
Get Current status API will provide you with a set of current condition data based on the location or device id requested.
```
### By Device ID
```
Request
```
```
{
     "deviceId" : 8802,
     "operation" : "deviceControl",
     "actionName" : "getStatus",
     "actionParameters" : {
             "version":"v4"
     },
     "userId":57569
}
```
|Parameter Name|Description|Type|
|---|---|---|
|deviceId|This uniquely identifies the Earth networks Weather Station.|Integer|
|version|Version of Earth networks Weather APIs (V4 or V3).|String|

### By Location (Latitude,Longitude)
````
Request
````
```
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getStatus",
     "actionParameters" : {
                                "lon":-116,
                                "lat":43,
                                "version":"v3"
     },
     "userId":57569
}
```
|Parameter Name|Description|Type
|---|---|---|
lon|Longitude of the required location |Double|
|lat|Latitude of the required location.|Double|
|version|Version of Earth networks Weather APIs (V4 or V3).|String|
```
Response
````
```
{
    "deviceResponse": {
        "observation": {
            "key": "3_PLWTT",
            "stationId": "PLWTT",
            "providerId": 3,
            "observationTimeLocalStr": "2014-10-16T15:44:44",
            "observationTimeUtcStr": "2014-10-16T10:14:44",
            "iconCode": 202,
            "altimeter": null,
            "altimeterRate": null,
            "dewPoint": -18.5,
            "dewPointQc": "Q",
            "dewPointRate": null,
            "heatIndex": 23.2,
            "heatIndexQc": "Q",
            "humidity": 5,
            "humidityQc": "C",
            "humidityRate": -20,
            "humidityRateQc": "C",
            "pressureSeaLevel": 1010,
            "pressureSeaLevelQc": "C",
            "pressureSeaLevelRate": 3,
            "pressureSeaLevelRateQc": "C",
            "rainDaily": 1.5,
            "rainDailyQc": "C",
            "rainRate": 0,
            "rainRateQc": "C",
            "rainMonthly": 4.8,
            "rainMonthlyQc": "C",
            "rainYearly": 4.8,
            "rainYearlyQc": "C",
            "snowDaily": null,
            "snowRate": null,
            "snowMonthly": null,
            "snowYearly": null,
            "temperature": 23.2,
            "temperatureQc": "C",
            "temperatureRate": -6.9,
            "temperatureRateQc": "C",
            "visibility": null,
            "visibilityRate": null,
            "windChill": 23.2,
            "windChillQc": "S",
            "windSpeed": 12,
            "windSpeedQc": "C",
            "windDirection": 47,
            "windDirectionQc": "C",
            "windSpeedAvg": 17.6,
            "windSpeedAvgQc": "C",
            "windDirectionAvg": 82,
            "windDirectionAvgQc": "C",
            "windGustHourly": 40.2,
            "windGustHourlyQc": "C",
            "windGustTimeLocalHourlyStr": "2014-10-16T15:41:00",
            "windGustTimeUtcHourlyStr": "2014-10-16T10:11:00",
            "windGustDirectionHourly": 54,
            "windGustDirectionHourlyQc": "C",
            "windGustDaily": 40.2,
            "windGustDailyQc": "C",
            "windGustTimeLocalDailyStr": "2014-10-16T15:42:00",
            "windGustTimeUtcDailyStr": "2014-10-16T10:12:00",
            "windGustDirectionDaily": 54,
            "windGustDirectionDailyQc": "C",
            "observationTimeAdjustedLocalStr": "2018-07-24T16:22:38",
            "wetBulbTemperature": 8.6,
            "wetBulbTemperatureQc": "S",
            "RuleDetails": null,
            "FinalDistance": 0,
            "FromHourlyForecastLocationId": null,
            "IconDescription": "80% Chance of Storms",
            "feelsLike": 23.2
        },
        "highlow": {
            "humidityHigh": 5,
            "humidityHighQc": "C",
            "humidityHighLocalStr": "2014-10-16T13:30:00",
            "humidityHighUtcStr": "2014-10-16T08:00:00",
            "humidityLow": 5,
            "humidityLowQc": "C",
            "humidityLowLocalStr": "2014-10-16T15:44:00",
            "humidityLowUtcStr": "2014-10-16T10:14:00",
            "pressureSeaLevelHigh": 1013,
            "pressureSeaLevelHighQc": "C",
            "pressureSeaLevelHighLocalStr": "2014-10-16T08:23:00",
            "pressureSeaLevelHighUtcStr": "2014-10-16T02:53:00",
            "pressureSeaLevelLow": 1007,
            "pressureSeaLevelLowQc": "C",
            "pressureSeaLevelLowLocalStr": "2014-10-16T14:22:00",
            "pressureSeaLevelLowUtcStr": "2014-10-16T08:52:00",
            "rainRateMax": 1,
            "rainRateMaxQc": "C",
            "rainRateMaxLocalStr": "2014-10-16T13:10:00",
            "rainRateMaxUtcStr": "2014-10-16T07:40:00",
            "temperatureHigh": 31.3,
            "temperatureHighQc": "C",
            "temperatureHighLocalStr": "2014-10-16T13:15:00",
            "temperatureHighUtcStr": "2014-10-16T07:45:00",
            "temperatureLow": 23.1,
            "temperatureLowQc": "C",
            "temperatureLowLocalStr": "2014-10-16T03:54:00",
            "temperatureLowUtcStr": "2014-10-15T22:24:00"
        },
        "station": {
            "StationId": "PLWTT",
            "ProviderId": 3,
            "ProviderName": "Earth Networks Inc",
            "StationName": "The Overseas School of Colombo",
            "Latitude": 6.8925,
            "Longitude": 79.9294444444444,
            "ElevationAboveSeaLevel": 21.9456
        }
    }
}
```

## Get Almanac  

#### Description

``````
Dialog Weather Get Almanac API will provide you with sun rise and sun set data based on the location requested.
``````
``````
Request
``````

``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getSunInfo",
     "actionParameters" : {
                                "lon":-116,
                                "lat":43
     },
     "userId":57569
}
``````

|Parameter Name|Description|Type
|---|---|---|
|lon|Longitude of the required location. |Double|
|lat|Latitude of the required location.|Double|

``````
Response
``````
``````
{
    "deviceResponse": {
        "solar": [
            {
                "sunriseDateTimeLocalS": "2018-07-24T06:26:31",
                "sunsetDateTimeLocalStr": "2018-07-24T21:13:53"
            }
        ],
        "lunar": [
            {
                "phaseIdentifier": -82,
                "phaseName": "Waxing Gibbous",
                "phaseIconCode": 12
            }
        ]
    }
}
``````

## Get Daily Forecast 

#### Description

```
Dialog Weather Get Daily Forecast API will provide you with daily weather forecast data based on the location or District ID requested.
```

### By Location (Latitude, Longitude)
``````
Request
``````
``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getForecastDataByLoc",
     "actionParameters" : {
                                "period":"daily",
                                "lon":78.9,
                                "lat":6.92
     },
     "userId":57569
}
``````

|Parameter Name|Description|Type
|---|---|---|
|period|Period the required forecast (Daily or Hourly).|String
|lon|Longitude of the required location.| Double|
|lat|Latitude of the required location.|Double|

### By District ID 
``````
Request
``````

``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getForecastDataByCity",
     "actionParameters" : {
                                "cityId" : "CEXXK0003",
                                "period":"daily"
     },
     "userId":57569
}
``````
|Parameter Name|Description|Type
|---|---|---|
|cityId|District ID (9 characters).|String|
|period|Period the required forecast (Daily or Hourly).|String|

``````
Response
``````
``````
{
    "deviceResponse": {
        "dailyForecastPeriods": [
            {
                "cloudCoverPercent": 48,
                "dewPoint": 21,
                "iconCode": 87,
                "precipCode": 1,
                "precipProbability": 25,
                "relativeHumidity": 83,
                "summaryDescription": "30% Chance of Rain",
                "temperature": 25,
                "thunderstormProbability": 20,
                "windDirectionDegrees": 244,
                "windSpeed": 14,
                "snowAmountMm": 0,
                "detailedDescription": "Partly cloudy with a slight chance of rain. Chance of precipitation 25%. High temperature around 25C. Dew point will be around 21C with an average humidity of 83%. Winds will be 14 kph from the WSW.",
                "forecastDateLocalStr": "2018-07-24T06:00:00",
                "forecastDateUtcStr": "2018-07-24T00:30:00Z",
                "isNightTimePeriod": false
            }, ...................................
        ],
        "forecastCreatedUtcStr": "2018-07-24T11:01:00.0000000Z",
        "location": "CEXXK0003",
        "locationType": "city"
    }
}
``````

## Get Hourly Forecast 

#### Description

```
Dialog Weather Get Hourly Forecast API will provide you with hourly weather forecast data based on the location or District ID requested.
```

### By Location (Latitude, Longitude)
``````
Request
``````

``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getForecastDataByLoc",
     "actionParameters" : {
                                "period":"hourly",
                                "lon":78.9,
                                "lat":6.92
     },
     "userId":57569
}
``````
|Parameter Name|Description|Type
|---|---|---|
|period|Period the required forecast (Daily or Hourly).|String|
|lon|Longitude of the required location.|Double|
|lat|Latitude of the required location.|Double|

### By District ID 
``````
Request
``````

``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getForecastDataByCity",
     "actionParameters" : {
                                "cityId" : "CEXXK0003",
                                "period":"hourly"
     },
     "userId":57569
}
``````

|Parameter Name|Description|Type
|---|---|---|
|cityId|District ID (9 characters).|String|
|period|Period the required forecast (Daily or Hourly).|String|

``````
Response
``````

``````
{ 
  "deviceResponse": { 
     "hourlyForecastPeriod": [ 
	       {
         "adjustedPrecipProbability": 20,
         "cloudCoverPercent": 64, 
         "description": "Partly Cloudy", 
         "dewPoint": 21, 
         "feelsLike": 26, 
         "forecastDateLocalStr": "2018-08-23T14:30:00", 
         "forecastDateUtcStr": "2018-08-23T09:00:00Z", 
         "iconCode": 3, 
         "precipCode": 1, 
         "precipProbability": 18, 
         "precipRate": 0.2, 
         "relativeHumidity": 72, 
         "temperature": 26, 
         "thunderstormProbability": 50, 
         "windDirectionDegrees": 248, 
         "windSpeed": 11, 
         "surfacePressure": 942, 
         "snowRate": 0, 
         "globeTemperature": 28, 
         "wetBulbTemperature": 23 
       }, 
       ………………….………………….………………….…………………………………. 
    ], 
     "forecastCreatedUtcStr": "2018-08-23T08:35:00.0000000Z", 
     "location": "CEXXK0003", 
     "locationType": "city" 
  } 
}
``````

## 5. Get Lightning Alerts

### Description

``````
Dialog Weather Get Lightning Alerts API will provide you with Lightning alerts based on the location or district ID requested.
``````
``````
Request
``````
``````
{
     "deviceId" : 8801,
     "operation" : "deviceControl",
     "actionName" : "getLightningAlerts",
     "actionParameters" : {
                                "nwlon" : -78.0000,
                                "nwlat": 100.3000,
                                "selon": -59.6000,
                                "selat": -33.2000
     },
     "userId":57569
}

``````

``````
Response (When Lightning alerts are available)
``````

````
{  
   "deviceResponse":{  
      "response":"<?xml version=\"1.0\" encoding=\"utf-8\"?>
     <ArrayOfAlert xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\"                        xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\">
        <alert xmlns=\"urn:oasis:names:tc:emergency:cap:1.2\">
        <identifier  >SAS201808280908001</identifier>
        <sender  >cap-alert-feed@earthnetworks.com</sender>
        <sent  >2018-0828T09:09:00+00:00</sent>
        <status  >Actual</status>
        <msgType  >Alert</msgType>
        <source  >Earth Networks</source>
        <scope  >Public</scope>
        <info  >
            <category>Met</category>
            <event>Earth Networks Level 1 Lightning Polygon</event>
            <responseType>Monitor</responseType>
            <urgency>Expected</urgency>
            <severity>Unknown</severity>
            <certainty>Possible</certainty>
            <effective>2018-08-28T09:09:00+00:00</effective>
            <expires>2018-08-28T09:54:00+00:00</expires>
            <senderName>Earth Networks Headquarters, Germantown, MD</senderName>
            <headline>Earth Networks Level 1 Lightning Polygon until 2018-08-     28T09:54:00.0000000+00:00</headline>
            <description>* At 2:39 PM IST...Earth Networks Headquarters in Germantown, MD has issued a Level 1 Lightning Polygon until 3:24 PMIST. The Earth Networks Total Lightning Network is indicating that a storm containing lightning could affect your location in the next 45 minutes.</description>
            <instruction>A storm containing lightning could affect your location in 	the next 45 minutes.</instruction>
            <contact>https://support.earthnetworks.com/ContactSupport </contact>
            <parameter>
                <valueName>LIGHTNING_SEVERITY</valueName>
                <value>Low</value>
            </parameter>
            <parameter>
                <valueName>CELL_POLYGON</valueName>
                <value>POLYGON ((9.19489357055497 80.5079302973404, 9.20397622642414 80.5194755464661, 9.21392638229349 80.5279302973404, 9.22397622642414 80.5356733456127, 9.23361905260362 80.5279302973404, 9.22995967288422 80.5079302973404, 9.22397622642414 80.5023272375843, 9.20397622642414 80.4945996830519, 9.19489357055497 80.5079302973404))</value>
            </parameter>
            <parameter>
                <valueName>DIRECTION</valueName>
                <value>109</value></parameter>
            <parameter>
                <valueName>SPEED</valueName>
                <value>11 mph</value>
            </parameter>
            <area>
                <areaDesc>Latitude: 9.217, Longitude: 80.515</areaDesc>
<polygon>9.17823932047394 80.5384895110403, 9.3000736828604 80.7758334694344, 9.41379336490289 80.6937476746741, 9.22688922764336 80.5033727553188, 9.17823932047394 80.5384895110403</polygon>
            </area>
    </info>
    </alert>
    <alert xmlns=\"urn:oasis:names:tc:emergency:cap:1.2\">
    <identifier  >SAS201808280918001</identifier>
    <sender  >cap-alert-feed@earthnetworks.com</sender>
    <sent  >2018-08-28T09:19:00+00:00</sent>
    <status  >Actual</status>
    <msgType  >Alert</msgType>
    <source  >Earth Networks</source>
    <scope  >Public</scope><info  >
    <info  >
        <category>Met</category>
        <event>Earth Networks Level 1 Lightning Polygon</event>
        <responseType>Monitor</responseType>
        <urgency>Expected</urgency>
        <severity>Unknown</severity>
        <certainty>Possible</certainty>
        <effective>2018-08-28T09:19:00+00:00</effective>
        <expires>2018-08-28T10:04:00+00:00</expires>
        <senderName>Earth Networks Headquarters, Germantown, MD</senderName>
        <headline>Earth Networks Level 1 Lightning Polygon until 2018-08-28T10:04:00.0000000+00:00</headline>
        <description>* At 2:49 PM IST...Earth Networks Headquarters in Germantown, MD has issued a Level 1 Lightning Polygon until 3:34 PM IST. The Earth Networks Total Lightning Network is indicating that a storm containing lightning could affect your location in the next 45 minutes.</description>
        <instruction>A storm containing lightning could affect your location in the next 45 minutes.</instruction>
        <contact>https://support.earthnetworks.com/ContactSupport</contact>
        <parameter>
            <valueName>LIGHTNING_SEVERITY</valueName>
            <value>Low</value>
        </parameter>
        <parameter>
            <valueName>CELL_POLYGON</valueName>
            <value>POLYGON ((9.16556885590816 80.5343980177861, 9.16397622642414 80.534797513955, 9.15494153779705 80.5543980177861, 9.14705363312508 80.5743980177861, 9.1565053685631 80.5943980177861, 9.16397622642414 80.5991087427692, 9.18281185529689 80.5943980177861, 9.18397622642414 80.5938689517114, 9.20397622642414 80.5834651545276, 9.20959581018457 80.5743980177861, 9.20397622642414 80.5596210887017, 9.1872271419239 80.5343980177861, 9.18397622642414 80.5311471022864, 9.16556885590816 80.5343980177861))</value>
        </parameter>
        <parameter>
            <valueName>DIRECTION</valueName>
            <value>43</value>
        </parameter>
        <parameter>
            <valueName>SPEED</valueName>
            <value>8 mph</value>
        </parameter>
        <area>
            <areaDesc>Latitude: 9.179, Longitude: 80.568</areaDesc>
<polygon>9.1846116630124 80.5850100483297, 9.41440488785169 80.5057207467929, 9.36376066810835 80.4111862277737, 9.17044486253167 80.5585657339286, 9.1846116630124 80.5850100483297</polygon>
        </area>
    </info>
</alert>
</ArrayOfAlert>"
}
}
````

````
Response (When lightning alerts are not Available)
````
````
{
     "deviceResponse":{
        "response":"﻿<?xml version=\"1.0\" encoding=\"utf-8\"?>
<ArrayOfAlert xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" />"
}
}
````


## Lookup Tables

#### District IDs

|Distict|District ID|
|---|---|
|Jaffna|CEXXJ0002|
|Kilinochchi|81787584|
|Mannar|CEXXM0001|
|Mullaitivu|81785076|
|Vavuniya|CEXXV0001|
|Puttalam|CEXXP0004|
|Kurunegala|CEXXK0008|
|Gampaha|CEXXG0002|
|Colombo|CEXXC0002|
|Kalutara|CEXXK0002|
|Anuradhapura|CEXXA0002|
|Polonnaruwa|CEXXP0003|
|Matale|CEXXM0002|
|Kandy|CEXXK0003|
|Nuwara Eliya|CEXXN0003|
|Kegalle|CEXXK0006|
|Ratnapura|CEXXR0002|
|Trincomalee|CEXXT0001|
|Batticaloa|CEXXB0004|
|Ampara|81792347|
|Badulla|CEXXB0001|
|Monaragala|81785231|
|Hambantota|81789586|
|Matara|CEXXM0003|
|Galle|CEXXG0001|


#### Unit of response parameters

|Variable Field (short/long) |Variable Name|Units (Metric/English)|Format|
|---|---|---|---|
|k/key|Key|-|(always null)|
|si/stationId|Station ID|Text|4 or 5 Characters|
|pi/providerId|Provider ID|Whole Number|1or 2 Digits|
|otls/observationTimeLocalStr|Observation Time, Local|Text/Local Time Zone of Station|yyyy-MM-ddThh:mm:ss|
|otus/observationTimeUtcStr|Observation Time, UTC|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|Ic/iconCode|Icon Code|Whole Number|###
|a/altimeter|Altimeter|(Millibars/inches of Mercury)|####
|ar/altimeterRate|Altimeter Rate|(Millibars/inches of Mercury) per hour|##/-##
|dp/dewPoint|Dew Point|Degrees (Celsius/Fahrenheit)|##.#/-##.#
|dpr/dewPointRate|Dew Point Rate|Degrees (Celsius/Fahrenheit) per hour|##.#/-##.#
|hi/heatIndex|Heat Index|Degrees (Celsius/Fahrenheit)|##.#/-##.#
|h/humidity|Humidity|Percent relative humidity|###.#
|hr/humidityRate|Humidity Rate|Change per hour of percent relative humidity|##.#/-##.#
|psl/pressureSeaLevel|Sea Level Pressure|(Millibars/inches of Mercury)|##.## or ####.#|
|pslr/pressureSeaLevelRate|Sea Level Pressure Rate|(Millibars/inches of Mercury) per hour|##.##/-##.##|
|rd/rainDaily|Daily Rain|(Millimeters/Inches)|###.#|
|rr/rainRate|Rain Rate|(Millimeters/Inches) per hour|###.#|
|rm/rainMonthly|Monthly Rain|(Millimeters/Inches)|###.#|
|ry/rainYearly|Yearly Rain|(Millimeters/Inches)|###.#|
|sd/snowDaily|Daily Snow|(Millimeters/Inches)|###.#|
|sr/snowRate|Snow Rate|(Millimeters/Inches) per hour|###.#|
|sm/snowMonthly|Monthly Snow|(Millimeters/Inches)|###.#|
|sy/snowYearly|Yearly Snow|(Millimeters/Inches)|###.#|
|t/temperature|Temperature|Degrees (Celsius/Fahrenheit)|##.#/-##.#|
|tr/temperatureRate|Temperature Rate|Degrees (Celsius/ Fahrenheit) per hour|##.#/-##.#|
|v/visibility|Visibility|Kilometers/Miles|##.#|
|vr/visibilityRate|Visibility Rate|Kilometers/Miles per hour|##.#|
|wc/windChill|Wind Chill|Degrees (Celsius/Fahrenheit)|##.#/-##.#|
|ws/windSpeed|Wind Speed|Kilometers/Miles per hour|##.#
|wd/windDirection|Wind Direction|Degrees from north (clockwise|###|
|wsa/windSpeedAverage|Average Wind Speed|Kilometers/Miles per hour|##.#|
|wda/windDirectionAverage|Average Wind Direction|Degrees from north (clockwise)|###|
|wgh/windGustHourly|Hourly Wind Gust|Kilometers/Miles per hour|##.#|
|wgtlhs/ windGustTimeLocalHourlyStr|Hourly Wind Gust Time, Local|Text/Local Time Zone of Station|yyyy-MM-ddThh:mm:ss|
|wgtuhs/ windGustTimeUtcHourlyStr|Hourly Wind Gust Time, UTC|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|wgdh/ windGustDirectionHourly|Hourly Wind Gust Direction|Degrees from north (clockwise)|###|
|wgd/windGustDaily|Daily Wind Gust|Kilometers/Miles per hour|##.#|
|wgtlds/ windGustTimeLocalDailyStr|Daily Wind Gust Time, Local|Text/Local Time Zone of Station|yyyy-MM-ddThh:mm:ss|
|wgtuds/ windGustTimeUtcDailyStr|Daily Wind Gust Time, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|wgdd/ windGustDirectionDaily|Daily Wind Gust Direction|Degrees from north (clockwise)|###|
|wbt/wetBulbTemperature|Wet Bulb Temperature|Degrees (Celsius/Fahrenheit)|##.#/-##.#|
|fl/feelsLike|Feels Like (Heat Index/Wind Chill)|Degrees (Celsius/Fahrenheit)|##.#/-##.#|
|humidityHigh|Humidity Daily High|Percent %|##.#|
|humidityHighLocalStr|Humidity Daily High, Local|Text/Local Time Zone of Station|yyyy-MM-ddThh:mm:ss|
|humidityHighUtcStr|Humidity Daily High, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|humidityLow|Humidity Daily Low|Percent %|##.#|
|humidityLowLocalStr|Humidity Daily Low, Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|humidityLowUtcStr|Humidity Daily Low, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|pressureSeaLevelHigh|Mean Sea Level Pressure (MSLP)|Millibars/Inches of Mercury|##.## or ####.#|
|pressureSeaLevelHighLocalStr|Daily High MSLP, Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss
|pressureSeaLevelHighUtcStr|Daily High MSLP, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss
|pressureSeaLevelLow|Mean Sea Level Pressure (MSLP)|Millibars/Inches of Mercury|##.## or ####.#|
|pressureSeaLevelLowLocalStr|Daily Low MSLP,Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|pressureSeaLevelLowUtcStr|Daily Low MSLP, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|rainRateMax|Daily Max Rain Rate|milimeters or inches per hour|##.##|
|rainRateMaxLocalStr|Daily Max Rain Rate, Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|rainRateMaxUtcStr|Daily Max Rain Rate, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|temperatureHigh|Daily High Temperature|Degrees F or C|###.#|
|temperatureHighLocalStr|Daily High Temperature, Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|temperatureHighUtcStr|Daily High Temperature, UTC String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|temperatureLow|Daily Low Temperature|Degrees F or C|###.#|
|temperatureLowLocalStr|Daily Low Temperature, Local Time String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|temperatureLowUtcStr|Humidity Low, UTC|String|Text/Universal Time Zone|yyyy-MM-ddThh:mm:ss|
|si/stationId|Station ID|Text|4 or 5 Characters|
|pi/providerId|Provider ID|Whole Number|1 or 2 Digits|
|ProviderName|Provider Name|Text|N/A|
|StationName|Station Name|Text|N/A|
|Latitude|Latitude|Decimal|##.########|
|Longitude|Longitude|Decimal|##.######|
|ElevationAboveSeaLevel|Elevation Above Sea Level|Decimal|##.####|

#### Lightning Alerts XML elements

|Elements| Description|
|---|---|
|Identifier| A number or string uniquely identifying this message, assigned by the sender.
|Sender |Identifies the originator of this alert. Guaranteed by assigner to be unique globally; e.g., may be based on an Internet domain name.
|sent| The date and time the alert originator sent the alert. The date and time represented in the DateTime Data Type format (e.g., "2002‐05‐24T16:49:00‐07:00" for 24 May 2002 at 16:49 PDT).|
|status| The code denoting the appropriate handling of the alert message.[“Actual” ‐ Actionable by all targeted recipients],[“Exercise” ‐ Actionable only by designated exercise participants],[“System” ‐ For messages that support alert network internal functions],[“Test” ‐ Technical testing only, all recipients disregard], [“Draft” ‐ A preliminary template or draft, not actionable in its current form]|
|msgType| The code denoting the nature of the alert message;[“Alert” ‐ Initial information requiring attention by targeted recipients]
|source| The text identifying the source of the alert message: Earth Networks|
|scope| The code denoting the intended distribution of the alert message:[“Public” ‐ For general dissemination to unrestricted audiences],[“Restricted” ‐ For dissemination only to users with a known operational requirement],[“Private” ‐ For dissemination only to specified addresses]|
|category| The code denoting the category of the subject event of the alert message. Met is the code for weather.|
|event| The text denoting the type of the subject event of the alert message.|
|urgency| The code denoting the urgency of the subject event of the alert message. The code will always be “expected”: Responsive action SHOULD be taken soon (within next hour)|
|severity| The code denoting the severity of the subject event of the alert message. [“Severe” – DTA], [“Moderate” – Level 2 threshold], [“Low” – Level 1 threshold]|
|Info| The container for all component parts of the info sub‐element of the alert message|
|Certainty| The code denoting the certainty of the subject event of the alert message. The code will always be: [“Likely” ‐ meaning that the possibility of occurrence is greater than 50%]|
|effective| The alert event start time. The date and time will be represented in the DateTime Data Type format (e.g., “2002‐05‐24T16:49:00‐07:00” for 24 May 2002 at 16: 49 PDT).|
|expires| The alert event expected end time. The date and time will be represented in the DateTime Data Type format (e.g., “2002‐05‐24T16:49:00‐07:00” for 24 May 2002 at 16: 49 PDT).|
|senderName| The human‐readable name of the agency or authority issuing this alert.|
|Description| A complete bulletin that includes all the DTA information in a readable that mimics the National Weather Service warnings bulletin. You can use the bulletin as is or add to it other elements from the feed or any other custom content. The time is this element is the location local time in the following format: HH:MM AM/PM CST (3 letter time zone abbreviation.|
|Instruction| The text describing the recommended action to be taken by recipients of the alert message. The feed contains a generic instruction that can be customized before being delivered to the end user.|