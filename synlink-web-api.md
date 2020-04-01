# SynLink Web HTTP(S) API
###### tags: `documentation` `http`

---
- [**Overview**](#Overview)
- [**Authentication**](#Authentication)
  + [Session Authentication](#Session-Based-Authentication)
  + [Personal Access Token Authentication](#Personal-Access-Token-Authentication)
  + [Login](#Login)
- [**Endpoints**](#Endpoints)
  + [Device](#Device)
    + [Device Object](#Device-Object)
    + [Get Device Object](#Get-Device-Info)
    + [Device Events](#Device-Events)
      + [3 Phase Imbalance](#3-Phase-Imbalance)
      + [Breaker Trip](#Breaker-Trip)
  + [Banks](#Banks)
    + [Bank Object](#Bank-Object)
    + [List All Banks](#List-All-Banks)
    + [Get Individual Bank](#Get-Individual-Bank)
    + [Modify Bank Info](#Modify-Bank-Info)
    + [Bank Events](#Bank-Events)
      + [Bank Current Max Threshold](#Bank-Current-Max-Threshold)
      + [Bank Current Min Threshold](#Bank-Current-Min-Threshold)
      + [Bank Voltage Max Threshold](#Bank-Voltage-Max-Threshold)
      + [Bank Voltage Min Threshold](#Bank-Voltage-Min-Threshold)
      + [Bank Power Factor Min Threshold](#Bank-Power-Factor-Min-Threshold)
      + [Bank Breaker Trip](#Bank-Breaker-Trip)
  + [Outlets](#Outlets)
    + [Outlet Object](#Outlet-Object)
    + [List All Outlets](#List-All-Outlets)
    + [Get Individual Outlet Info](#Get-Individual-Outlet-Information)
    + [Modify Outlet Info](#Modify-Bank-Info)
    + [Outlet Events](#Outlet-Events)
  + [Groups](#Groups)
    + [Group Object](#Group-Object)
    + [List All Groups](#List-All-Groups)
    + [Create a Group](#Create-A-Group)
    + [Modify Group Info](#Modify-Group-Info)
    + [Remove a Group](#Remove-A-Group)
    + [Group Events](#Group-Events)
  + [Network](#Network)
    + [Network Object](#Network-Object)
    + [Get Network Information](#Get-Network-Information)
    + [Network Events](#Network-Events)
      + [AutoPing Reboot](#AutoPing-Reboot)
  + [System](#System)
    + [System Object](#System-Object)
    + [Get System Information](#Get-System-Information)
    + [System Events](#System-Events)
      + [Scheduled Events](#Scheduled-Events)
  + [Configure](#Configure)
    + [Modify Network Settings](#Modify-Network-Settings)
    + [Modify System Settings](#Modify-System-Settings)
    + [Modify Logging Settings](#Modify-Logging-Settings)
    + [Download Configuration Backup File](#Download-Configuration-Backup-File)
    + [Restore Saved Configuration from File](#Restore-Saved-Configuration-From-File)
  + [Users](#Users)
    + [User Object](#User-Object)
    + [List All Users](#List-All-Users)
    + [List A User's SSH Keys](#List-A-User's-SSH-Keys)
    + [Add User SSH Key](#Add-User-SSH-Key)
    + [Remove User SSH Key](#Remove-User-SSH-Key)
    + [Generate User PAT](#Generate-User-PAT)
    + [Revoke User PAT](#Revoke-User-PAT)
    + [Remove User](#Remove-User)
  + [Sensors](#Sensors)
    + [Sensor Object](#Sensor-Object)
    + [List Connected Sensors](#List-Connected-Sensors)
    + [Get Individual Sensor Details](#Get-Individual-Sensor-Details)
    + [Modify Individual Sensor Details](#Modify-Individual-Sensor-Details)
    + [Sensor Events](#Sensor-Events)
      + [Temperature Thresholds](#Temperature-Max-Threshold)
      + [Humidity Thresholds](#Humidity-Max-Threshold)
  + [Events](#Events)
    + [Event Object](#Event-Object)
    + [Listing Events](#Listing-Events)
    + [Create an Event](#Create-An-Event)
    + [Modify an Event](#Modify-An-Event)
    + [Remove an Event](#Remove-An-Event)
  + [Logs](#Logs)
    + [List Syslog Logs](#List-Syslog-Logs)
    + [Clear Syslog Logs](#Clear-Syslog-Logs)
    + [List Power Logs](#List-Power-Logs)
    + [Clear Power Logs](#Clear-Power-Logs)
  + [Other Endpoints](#Other-Endpoints)
    + [Restart Network](#Restart-Network)
    + [Reboot Controller](#Reboot-Controller)
    + [Factory Reset Controller](#Factory-Reset-Controller)
+ [All Endpoints](#All-Endpoints)

<!-- [TOC] -->

# Overview
SynLink PDUs functionality can be accessed via a Web/HTTP(S) Application Programming Interface (API).
Examples within this guide use the `curl` command on Linux OS Ubuntu 18.04. `curl` is available for download via `sudo apt install curl` on Linux OS's that support Apt Package Manager. Testing the API is possible with [Postman](https://www.postman.com/product/api-client) on Mac, Windows, or Linux Operating Systems.
Endpoints Post bodys use JSON (Javascript Object Notation). Output Responses are mainly also in JSON.

# Authentication
SynLink API has various authentication methods.

## Session Based Authentication
Session Based Authentication involves using the `/login` route to generate a session token which is 

Using `POST /login` route with proper credentials will return User Object with **`sessionToken`** Field. 
To use any endpoint requiring a token add **`sessionToken`** to HTTP request inside header.
`-h "Cookie: SPID=[sessionToken]"`

**Example Request for `/api/banks`**
```bash
curl 'http://192.168.1.100/system/banks' -H 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

## Personal Access Token Authentication
Can be found in web interface or ssh session.
Must be logged in as Admin user or have proper permissions.
Generate Personal Access Token **(PAT)** and keep it secure.
To use any endpoint requiring a token add **`accessToken`** 

![generate-access-token](https://i.imgur.com/Dfcn99J.png)

**Figure: Web Interface Personal Access Token**

## Login
```
POST /login     Returns session token with correct credentials
```
### Request
```javascript
{
  "username": "admin",
  "password": "admin"
}
```
**username** *required*
Username string. Max 100 characters. Default factory username is "admin"

**password** *required*
Password string. Default factory password is "admin"

#### Example Request
```bash
curl 'http://192.168.1.100/login' \
  --data '{"username":"admin","password":"admin"}' 
```
### Response 
Expected response is a User Object (todo show link)
```javascript
{
  "username":"admin",
  "accessToken":"09hj",
  "sessionToken": "j2d36cG2ciHKDDqFc3k",
  "id":"1"
}

```


----
# Endpoints
## Device

Device Object shows top level information about PDU power, energy, sensors, and etc.
Attributes will be different depending on different SynLink models.

**Endpoints**
```
GET    /api/device                               Returns an overview of all banks, groups, outlets, and networking configurations
GET    /api/device/events                        Returns device event related information
POST   /api/device/events                        Create event triggered by device specific activity.
GET    /api/device/events/:event_id              Modifies device related event or action
DELETE /api/device/events/:event_id              Permanently removes device related event/action.
```
### Device Object
```javascript
{
  "numBanks": 3,
  "numOutlets": 36,
  "inletPlug": "L21-30P",
  "outletPwrMeasurementsSupported": true,
  "outletSwitchingSupported": true,
  "lineFrequency": 60.1879997253418,
  "powerCapacity": 10807,
  "controllerSerialNumber": 11748295,
  "enclosureSerialNumber": 84759076,
  "totalCurrentRms": 5.129478162
}
```
<!--   "activePower": 5.199999809265137,
  "apparentPower": 14.069999694824219,
  "powerFactor": 0.36956787109375,
 -->


| Key | Type | Description |
|:-----|:----|:------------|
|**numBanks**|*number*|Number of banks|
|**numOutlets**|*number*|Number of outlets total|
|**inletPlug**|*string*|Inlet Plug Type. If multiple inlets, both are same plug type|
|**outletPwrMeasurementsSupported**|*boolean*|When true, each outlet can measure current draw in amps|
|**outletSwitchingSupported**|*boolean*|When true, each outlet can switch on or off with a relay|
|**totalCurrentRms**|*number*|Current draw of all devices. TODO necessary?|
|**lineFrequency**|*number*|Line Frequency in Hz of on input cord|
|**powerCapacity**|*number*|Max Power Capacity value in VA|
|**controllerSerialNumber**|*number*|Serial Number of SynLink Controller Module|
|**enclosureSerialNumber**|*number*|Serial number of SynLink Enclosure|

<!-- |**powerFactor**|*number*|Ratio of actual electrical power dissapated by AC Circuit to product of RMS values of current and voltage. Number between 1-100
|**activePower**|*number*|Power consumed by electrical resistance in watts|
|**apparentPower**|*number*|It is the total power which is delivered to the load in VA|
 -->
 
#### Additional Device Attributes - 3 phase
These additional attributes are available to the device object if the PDU enclosure is a 3 phase PDU. Wye or Delta.
| Key | Type | Description |
|:-----|:----|:------------|
|**currentRmsA**|*number*|Current Draw in amps on Line A|
|**currentRmsB**|*number*|Current Draw in amps on Line B|
|**currentRmsC**|*number*|Current Draw in amps on Line C|
|**3PhaseOutOfBalance**|*number*|3 Phase out of balance in percentage|
```javascript
{
  "currentRmsA": 0.17135562800934104,
  "currentRmsB": 0.10625681451860468,
  "currentRmsC": 0.13708140007839667,
  "3PhaseOutOfBalance": 12
}
```

#### Additional Device Attributes - 3 phase with Breakers
Following attribute are included with 3 phase PDU's which have inlet plugs > 20Amp capacity.
|Key | Type | Description |
|:-----|:----|:------------|
|**breakerAB**|*boolean*|Health of breaker, true is breaker is not tripped healthy|
|**breakerBC**|*boolean*|Health of breaker, true is breaker is not tripped healthy|
|**breakerAC**|*boolean*|Health of breaker, true is breaker is not tripped healthy|
```javascript
{
  "breakerAB": true,
  "breakerBC": true,
  "breakerAC": true
}
```
#### Additional Device Attributes - 1 phase
Following attributes apply to single phase PDUs with one set of outlets.
| Key | Type | Description |
|:-----|:----|:------------|
|**currentRms**|*number*|Current RMS for cord inlet(s)|
```javascript
{
  "currentRms": 0.13708140007839667
}
```
#### Additional Device Attributes - 1 phase with Breakers
These additional attributes apply to single phase dual bank PDUs with breakers.
| Key | Type | Description |
|:-----|:----|:------------|
|**breakerA**|*boolean*|health of breaker, true is breaker is not tripped and healthy|
|**breakerB**|*boolean*|health of breaker, true is breaker is not tripped and healthy|
```javascript
{
  "breakerA": true,
  "breakerB": true
}
```


#### Additional Attributes - ATS
Automatic Transfer Switch (ATS) Compatible PDU
```javascript
{
  "atsIncluded": true,
  "inletActive": "a",
  "inletAReady": true,
  "inletBReady": true
}
```

#### Additional Attributes - Dual Inlet/Circuit
Dual Inlet/Dual Circuit Compatible PDU
```javascript
{
  "currentRmsA": 0.13708140007839667,
  "currentRmsB": 0.23481276538896477
}
```


### Get Device Info
Retrieve PDU device information.
```
GET    /api/device                               Returns an overview of all banks, groups, outlets, and networking configurations
```
#### Request
Follows [Authentication Scheme](##Authentication)

```javascript
{} // no request parameters
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/device' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
Expected Response is a [Device Object](#Device-Object)
```javascript
{
  "numBanks": 3,
  "numOutlets": 36,
  "inletPlug": "L21-30P",
  "outletPwrMeasurementsSupported": true,
  "outletSwitchingSupported": true,
  "lineFrequency": 60.1879997253418,
  "activePower": 5.199999809265137,
  "apparentPower": 14.069999694824219,
  "powerCapacity": 10807,
  "powerFactor": 0.36956787109375,
  "controllerSerialNumber": 11748295,
  "enclosureSerialNumber": 84759076,
  "totalCurrentRms": 5.129478162
}

```

### Device Events

#### Manage Events
See [Events Section](#Events) for instructions to list, modify, and remove events.
| Event                 | Event Code | Parameter 1          | P1 Type  |
|:--------------------- |:---------- |:-------------------- |:-------- |
| **3 Phase Imbalance** | **13**     | Imbalance Percentage | *number* |
| **Breaker Trip**      | **14**     | NA                   |          |

#### 3 Phase Imbalance
Trigger a response whenever there is a sustained 3 Phase Imbalance. Only available on 3 phase PDUs.

##### Request
```javascript
{
  "event": {
    "eventCode": 13,
    "imbalanceThreshold": 20,
  },
  "action": ActionObject
  
}
```

**event.eventCode** *required*
Event Code must be: **`13`**

**event.imbalanceThreshold** *required*
Imbalance Threshold parameter must be a number between 1-100. Represents a percentage of Phase Imbalance. 
Phase imbalance is calculated by the largest deviation from any 2 lines current rms values divided by average current rms, times 100.

**action** *required*
See [Events Section](#Events) for valid action objects.

#### Breaker Trip
Trigger a response whenever there is a breaker trip event

##### Request
```javascript
{
  "event": {
    "eventCode": 14,
  },
  "action": ActionObject
}
```
**event.eventCode** *required*
Event Code must be **`14`**

**action** *required*
See [Events Section](#Events) for valid action objects.


---

## Banks
Banks are representation of groups of outlets. Each bank has it's own energy measurement capabilities. Individual Outlet Current Measurements or/and Inidividual Outlet Switching are optional additions.

**Endpoints**
```
GET    /api/banks                                Return list of all banks and their outlets
GET    /api/banks/:bank_id                       Returns individual bank information, including related events and actions
PUT    /api/banks/:bank_id                       Modifies individual bank
GET    /api/banks/:bank_id/events                Returns individual bank event related information
POST   /api/banks/:bank_id/events                Create event triggered by bank activity.
GET    /api/banks/:bank_id/events/:event_id      Modifies bank related event or action
DELETE /api/banks/:bank_id/events/:event_id      Permanently removes bank related event/action.
```

### Bank Object

```javascript
{
  "serialNumber":286331153,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.082999996840953827,
  "voltageRms":122.69999694824219,
  "lineFrequency":60.212001800537109,
  "powerFactor":0.424346923828125,
  "activePower":4.320000171661377,
  "reactivePower":1.7699999809265137,
  "apparentPower":10.180000305175781,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Bank 1"
}
```

| Key | Type | Description |
|:-----|:----|:------------|
|**serialNumber**| *number* | Unique identifier for the bank |
|**connected**| *boolean* | Whether bank is currently connected to a SynLink Controller|
|**outletMeteringSupported**| *boolean* | Whether the Bank supports individual outlet current metering |
|**outletSwitchingSupported**| *boolean* | Whether the Bank supports individual outlet switching |
|**version**| *string* | Current version of Bank Firmware|
|**currentRms**| *number* | The instantaneous current measured at the inlet in Amps|
|**lineFrequency**| *number* | Line Frequency measurement valid from 45-65 GHz|
|**voltageRms**| *number* | Instantaneous voltage reading at inlet and all outlets in volts|
|**powerFactor**| *number* | Power Factor reading. Value is signed value representing polarity of power factor |
|**activePower**| *number* | Active Power measured in KW (kiloWatts). Power consumed by electrical resistance |
|**reactivePower**| *number* | Reactive Power measured in KW (kiloWatts). Inductive and Capacitive power consumption |
|**apparentPower**| *number* | Power which is actually consumed or utilized |
|**importActiveEnergy**| *number* | kWH |
|**exportActiveEnergy**| *number* | kWH |
|**importReactiveEnergy**| *number* | kWH |
|**exportReactiveEnergy**| *number* | kWH |
|**bankName**| *string* | User defined string for bank name |
|**outlets**|*array*|Array of [Outlet Objects](#Outlet-Object)|


### List All Banks
Returns a list of all banks that the SynLink Controller has connected to. If **`outletSwitchingSupported`** or **`outletMeteringSupported`** are true, outlets field containing an array of [Outlet Objects](#Outlet-Object)
```
GET /api/banks    Returns all banks
```
#### Request
Follows [Authentication Scheme](##Authentication)

```javascript
{} // no request parameters
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/banks' -H 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
Expected response is an array of [Bank Objects](#Bank-Object)
```javascript
[{
  "serialNumber":286331153,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.082999996840953827,
  "voltageRms":122.69999694824219,
  "lineFrequency":60.212001800537109,
  "powerFactor":0.424346923828125,
  "activePower":4.320000171661377,
  "reactivePower":1.7699999809265137,
  "apparentPower":10.180000305175781,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Bank 1"
},{
  "serialNumber":572662306,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.037399999797344208,
  "voltageRms":122.90000152587891,
  "lineFrequency":60.212001800537109,
  "powerFactor":-0.0849609375,
  "activePower":-0.38999998569488525,
  "reactivePower":2.5299999713897705,
  "apparentPower":4.5900001525878906,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Bank 1"
},{
  "serialNumber":858993459,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.10949999839067459,
  "voltageRms":122.80000305175781,
  "lineFrequency":60.212001800537109,
  "powerFactor":0.34149169921875,
  "activePower":4.5900001525878906,
  "reactivePower":6.0799999237060547,
  "apparentPower":13.439999580383301,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Bank 3"
}]
```

### Get Individual Bank Information
Returns a bank object that the SynLink Controller has connected to from a bank_id. If **`outletSwitchingSupported`** or **`outletMeteringSupported`** are true, outlets field containing an array of [Outlet Objects](#Outlet-Object)

`:bank_id` should be replaced with a bank serial number.
```
GET /api/banks/:bank_id    Returns individual bank information, including related events and actions
```
#### Request
Follows [Authentication Scheme](##Authentication).

```javascript
{} // no request parameters
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/banks/286331153' -H 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
Expected response is a [Bank Object](#Bank-Object)
```javascript
{
  "serialNumber":286331153,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.082999996840953827,
  "voltageRms":122.69999694824219,
  "lineFrequency":60.212001800537109,
  "powerFactor":0.424346923828125,
  "activePower":4.320000171661377,
  "reactivePower":1.7699999809265137,
  "apparentPower":10.180000305175781,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Bank 1",
  "events": [{
    TODO PUT EVENTS HERE
  }]
}
```

### Modify Bank Info
The only value changeable for banks is a custom name field for banks.
```
PUT    /api/banks/:bank_id                       Modifies individual bank
```
#### Request
Follows [Authentication Scheme](##Authentication).
```javascript
{
  "bankName": "Critical Equipment Bank"
}
```
**bankName** *required*
Name string up to 100 characters.

##### Example Request
```bash
curl 'http://192.168.1.100/api/banks/286331153' \
  -H 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
  --data '{"bankName":"Critical Equipment Bank"}'
```
#### Response
Expected response is newly modified [Bank Object](#Bank-Object)
```javascript
{
  "serialNumber":286331153,
  "connected":true,
  "version":"0.0.0.0",
  "outletSwitchingSupported":false,
  "outletMeteringSupported":false,
  "currentRms":0.082999996840953827,
  "voltageRms":122.69999694824219,
  "lineFrequency":60.212001800537109,
  "powerFactor":0.424346923828125,
  "activePower":4.320000171661377,
  "reactivePower":1.7699999809265137,
  "apparentPower":10.180000305175781,
  "importActiveEnergy":0.0,
  "exportActiveEnergy":0.0,
  "importReactiveEnergy":0.0,
  "exportReactiveEnergy":0.0,
  "bankName": "Critical Equipment Bank"
}
```
### Bank Events
#### Manage Events
See [Events Section](#Events-Section) for instructions to list, modify, and remove events.
| Event                               | Event Code | Parameter 1                         | P1 Type  | Parameter 2 | P2 Type  | Parameter 3                | P3 Type  |
|:----------------------------------- |:---------- |:----------------------------------- |:-------- |:----------- |:-------- |:-------------------------- |:-------- |
| **Bank Current Max Threshold**      | **15**     | Current Threshold in milliAmperes   | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* |
| **Bank Current Min Threshold**      | **16**     | Current Threshold in milliAmperes   | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* |
| **Bank Voltage Max Threshold**      | **17**     | Voltage Threshold in mmilliVolts    | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* |
| **Bank Voltage Min Threshold**      | **18**     | Voltage Threshold in milliVolts     | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* |
| **Bank Power Factor Min Threshold** | **20**     | Power factor in ratio between 1-100 | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* |
| **Bank Breaker Trip**               | **21**     | Bank UUID                           | *string* | NA          |          | NA                         |          |

<!-- | **Bank Power Factor Max Threshold** | **19**     | Power factor in ratio between 1-100 | *number* | Bank UUID   | *string* | Num seconds past threshold | *number* | -->

#### Bank Current Max Threshold
Trigger a response whenever current RMS for a given bank exceeds a given current value for x number of seconds.

##### Request
```javascript
{
  "event": {
    "eventCode": 15,
    "currentMaxThreshold": 16000, // 16 amps, 16000 milliamps
    "bankId": 286331153,
    "numSecondsOver": 5
  },
  "action": ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`15`**

**event.currentMaxThreshold** *required*
Current Max Threshold in milliAmperes.

**event.bankId** *required*
Serial Number of Bank.

**event.numSecondsOver** *optional*
Default value is 5 seconds

**action** *required*
See [Events Section](#Events) for valid action objects.

#### Bank Current Min Threshold
Trigger a response whenever current RMS for a given bank is under a given current value for x number of seconds.

##### Request
```javascript
{
  "event": {
    "eventCode": 16,
    "currentMinThreshold": 500, // 0.5 amp minimum
    "bankId": 286331153,
    "numSecondsOver": 10
  },
  "action": ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`16`**

**event.currentMinThreshold** *required*
Current Min Threshold in milliAmperes.

**event.bankId** *required*
Serial Number of Bank.

**event.numSecondsOver** *optional*
Default value is 5 seconds

**action** *required*
See [Events Section](#Events) for valid action objects.

#### Bank Voltage Max Threshold
Trigger a response whenever Voltage measurement for a given bank is over a given current value for x number of seconds.

##### Request
```javascript
{
  "event": {
    "eventCode": 17,
    "voltageMaxThreshold": 218400, // 218.4 volts
    "bankId": 286331153,
    "numSecondsOver": 10
  },
  "action" ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`17`**

**event.voltageMaxThreshold** *required*
Current Max Threshold in milliVolts.

**event.bankId** *required*
Serial Number of Bank.

**event.numSecondsOver** *optional*
Default value is 5 seconds

**action** *required*
See [Events Section](#Events) for valid action objects.

#### Bank Voltage Min Threshold
Trigger a response whenever Voltage measurement for a given bank is under a given current value for x number of seconds.

##### Request
```javascript
{
  "event": {
    "eventCode": 18,
    "voltageMinThreshold": 197600, // 197.6 volts
    "bankId": 286331153,
    "numSecondsOver": 10
  },
  "action" ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`18`**

**event.voltageMinThreshold** *required*
Current Min Threshold in milliVolts.

**event.bankId** *required*
Serial Number of Bank.

**event.numSecondsOver** *optional*
Default value is 5 seconds

**action** *required*
See [Events Section](#Events) for valid action objects.

<!-- #### Bank Power Factor Max Threshold
Trigger a response whenever Power Factor  is  -->

#### Bank Power Factor Min Threshold
Trigger a response whenever power factor for a given bank is under a given power factor value for x number of seconds.

##### Request
```javascript
{
  "event": {
    "eventCode": 19,
    "powerFactorMinThreshold": 80,
    "bankId": 286331153,
    "numSecondsOver": 5
  },
  "action": ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`19`**

**event.powerFactorMinThreshold** *required*
Minimum Power Factor value before triggering response. Acceptable values are numbers between 1 and 100 which represent a ratio between active power and apparent power. 

**event.bankId** *required*
Serial Number of Bank.

**event.numSecondsOver** *optional*
Default value is 5 seconds

**action** *required*
See [Events Section](#Events) for valid action objects.


#### Bank Breaker Trip
Trigger a response whenever any breaker is tripped from over current detection. 

##### Request
```javascript
{
  "event": {
    "eventCode": 20,
  },
  "action": ActionObject
}
```
**event.eventCode** *required*
Event code must be: **`20`**

**action** *required*
See [Events Section](#Events) for valid action objects.


---


## Outlets
Outlets are representations of each individual outlet on the PDU. They show information related to outlets. They are only available of PDU has banks which support either Outlet Switching and/or Outlet Current Metering

**Endpoints**
```
GET    /api/outlets                              Returns list of all outlets
GET    /api/outlets/:outlet_id                   Returns individual outlet information, including related events & actions
PUT    /api/outlets/:outlet_id                   Modifies individual outlet
GET    /api/outlets/:outlet_id/events            Return individual outlet event related information
POST   /api/outlets/:outlet_id/events            Create event triggered by outlet activity
PUT    /api/outlets/:outlet_id/events/:event_id  Modifies outlet related event or action
DELETE /api/outlets/:outlet_id/events/:event_id  Permanently removes outlet related event/action.
```

### Outlet Object
```json
{
  "outletName": "Outlet 1",
  "outletUuid": "1-858993459",
  "pwrOnState": "PREV",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "Off",
  "maxCurrent": 20,
  "connector": "IEC 320 C14"
}
```
| Key | Type | Description |
|:-----|:----|:------------|
|**outletUuid**| *string* | Unique identifier for the Outlet.  |
|**outletName**| *string* | User defined outlet name |
|**pwrOnState**| *string* | Initial State of outlet on boot. Value is either `"on"`, `"off"`, or `"prev"` |
|**outletIndex**| *number* | Which number outlet amongst total outlets |
|**maxCurrent**| *number* |Max current in amps for outlet.|
|**connector**| *string* | Connector type for outlet receptacle. Value is either `"nema 5-15"`, `"nema 5-20"`, `"IEC 320 C14"`, or `"IEC 320 C19"` |
|**currentRms**| *number* | Key will exist if current_monitoring_supported value is `true`. return number value in amps for current load for outlet.|
|**state**| *boolean* | **`true`** if outlet is ON. Only exists of outlet current metering supported |

### List All Outlets
Returns a list of all outlets that within any connected banks. Only available if outletSwitchingSupported or outletMeteringSupported are true.
```
GET    /api/outlets                              Returns list of all outlets
```
#### Request
Follows [Authentication Scheme](##Authentication)

```javascript
{} // no request parameters
```

##### Example Request
```bash
curl 'http://192.168.1.100/api/outlets' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
```javascript
[{
  "outletName": "Outlet 1",
  "outletUuid": "1-858993459",
  "pwrOnState": "PREV",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "On",
  "connector": "IEC 320C14",
  "maxCurrent": 20
},{
  "outletName": "Outlet 2",
  "outletUuid": "2-858993459",
  "pwrOnState": "PREV",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "On",
  "connector": "IEC 320C14",
  "maxCurrent": 20
},{
  "outletName": "Outlet 3",
  "outletUuid": "3-858993459",
  "pwrOnState": "PREV",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "On",
  "connector": "IEC 320C14",
  "maxCurrent": 20
}...]
```

### Get Individual Outlet
Return one specific outlet with outlet id passed as url parameter.
`:outlet_id` should be replaced with outlet uuid. (universally unique identifier)
```
GET    /api/outlets/:outlet_id                   Returns individual outlet information, including related events & actions
```

#### Request Body
Follows [Authentication Scheme](##Authentication)

```javascript
{} // no request parameters
```

##### Example Request
```bash
curl 'http://192.168.1.100/api/outlets/1-858993459' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
Expected response is an [Outlet Object](#Outlet-Object)
```javascript
{
  "outletName": "Outlet 1",
  "outletUuid": "1-858993459",
  "pwrOnState": "PREV",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "On"
}
```

### Modify Outlet
Endpoint to modify Outlet information, including the relay state to turn outlet on and off.
```
PUT    /api/outlets/:outlet_id                   Modifies individual outlet
```

#### Request
Follows [Authentication Scheme](##Authentication).
All request parameters are optional, but at least one is required.
```javascript
{
  "outletName": "NAS Server 1",
  "pwrOnState": "off",
  "state": "on"
}
```
**outletName** *optional*
Name string up to 100 characters.

**pwrOnState** *optional*
Power on State is the state of the relay for the specific outlet when the PDU powers on from an off/powerless state.
Acceptable strings are `"on"`, `"off"`, `"prev"`. 
`"prev"` will set the relay to the state of the PDU when last lost power.

**state** *optional*
Setting state to `true` will set outlet relay to an **on** state.

##### Example Request
```bash
curl 'http://192.168.1.100/api/outlets/1-858993459' \
  --header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
  --data '{ "outletName":"NAS Server 1", "pwrOnState":"off", "state":"on" }'
```

#### Response
Expected response is an updated [Outlet Object](#Outlet-Object)
```javascript
{
  "outletName": "NAS Server 1",
  "outletUuid": "1-858993459",
  "pwrOnState": "off",
  "outletIndex": 1,
  "currentRms": 0,
  "state": "On"
}
```
---

## Groups
Groups are user created list of outlets. Outlets are designated by user. Groups can be used to manage sets of equipment together.
**Endpoints**
```
GET    /api/groups                               Returns list of all outlet groups
POST   /api/groups                               Creates a group of outlets.            
GET    /api/groups/:group_id                     Returns individual group information, including all related events/actions
PUT    /api/groups/:group_id                     Modifies individual group
DELETE /api/groups/:group_id                     Permanently remove group.
GET    /api/groups/:group_id/events              Returns group related events/actions
POST   /api/groups/:group_id/events              Create event triggered by group activity
PUT    /api/groups/:group_id/events/:event_id    Modifies a group related event/action
DELETE /api/groups/:group_id/events/:event_id    Permanently remove group related event/action
```

### Group Object
```javascript
{
  "groupName": "Networking Equipment",
  "groupUuid": "s7dGa0pS79dGA0S7d9g"
  "outlets": [{
    "outletName": "Edge Router 1",
    "outletUuid": "1-858993459",
    "pwrOnState": "on",
    "outletIndex": 1,
    "currentRms": 1.7,
    "state": "On"
  }, {
    "outletName": "Switch 2,
    "outletUuid": "4-858993459",
    "pwrOnState": "on",
    "outletIndex": 4,
    "currentRms": 1.3,
    "state": "On"
  }...]
}
```
|Key|Type|Description|
|:-|:-|:-|
|**groupName**|*string*|User defined name for group|
|**groupUuid**|*string*|Universal Unique Identifier for Group|
|**outlets**|*array*|Array of [Outlet Objects](#Outlet-Object)|


### Get List of Groups
Returns a list of all groups that have been created.
```
GET    /api/groups                               Returns list of all outlet groups
```

#### Request
Follows [Authentication Scheme](#Authentication-Scheme).
```javascript
{} // no request parameters
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/groups' --header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```
#### Response
Expected response is an array of [Group Objects](#Group-Object)
```javascript
[{
  "groupName": "Networking Equipment",
  "groupUuid": "s7dGa0pS79dGA0S7d9g"
  "outlets": [{
    "outletName": "Edge Router 1",
    "outletUuid": "1-858993459",
    "pwrOnState": "on",
    "outletIndex": 1,
    "currentRms": 1.7,
    "state": "On"
  }, {
    "outletName": "Switch 2,
    "outletUuid": "4-858993459",
    "pwrOnState": "on",
    "outletIndex": 4,
    "currentRms": 1.3,
    "state": "On"
  }...]
}, {
  "groupName": "Storage Equipment",
  "groupUuid": "0u912084N12dM5b05GU"
  "outlets": [{
    "outletName": "Nas Server 2",
    "outletUuid": "7-858993459",
    "pwrOnState": "on",
    "outletIndex": 7,
    "currentRms": 5.1,
    "state": "On"
  }, {
    "outletName": "Nas Server 1,
    "outletUuid": "8-858993459",
    "pwrOnState": "on",
    "outletIndex": 8,
    "currentRms": 7.2,
    "state": "On"
  }...]
}]
```

### Create Group
```
POST   /api/groups                               Creates a group of outlets.            
```
#### Request
Follows [Authentication Scheme](#Authentication-Scheme).
```javascript
{
  "groupName": "Example Group 1", // required
  "outlets": ["2-858993459", "7-858993459"] // required
}
```
**groupName** *required*
Name string up to 100 characters

**outlets** *required*
Outlets array of outlet UUIDs. Required at least 1.

#### Example Request
```bash
curl 'http://192.168.1.100/api/groups' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
--data '{"groupName":"Example Group 1","outlets":["2-858993459","7-858993459"]}'
```

#### Response
Expected response is the newly created [Group Object](#Group-Object) with new group uuid.
```javascript
{
  "groupName": "Example Group 1",
  "groupUuid": "0u912084N12dM5b05GU"
  "outlets": [{
    "outletName": "Nas Server 2",
    "outletUuid": "2-858993459",
    "pwrOnState": "on",
    "outletIndex": 7,
    "currentRms": 5.1,
    "state": "On"
  }, {
    "outletName": "Nas Server 1,
    "outletUuid": "7-858993459",
    "pwrOnState": "on",
    "outletIndex": 8,
    "currentRms": 7.2,
    "state": "On"
  }...]
}
```

### Modify Group
```
PUT    /api/groups/:group_id                     Modifies individual group
```

#### Request
Follows [Authentication Scheme](#Authentication-Scheme).
To modify the outlets inside of group, update the outlet array of outlet UUIDs.
`:group_id` is to be replaced by `Group UUID`
```javascript
{
  "groupName": "Storage Equipment 2",
  "outlets": ["7-858993459", "8-858993459", "3-858993459"]
}
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/groups/0u912084N12dM5b05GU' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
--data '{"groupName":"Storage Equipment 2","outlets":["7-858993459","8-858993459","2-858993459"]}'
```
#### Response
Expected response is a [Group Object](#Group-Object)
```javascript
{
  "groupName": "Storage Equipment 2",
  "groupUuid": "0u912084N12dM5b05GU"
  "outlets": [{
    "outletName": "Nas Server 2",
    "outletUuid": "7-858993459",
    "pwrOnState": "on",
    "outletIndex": 7,
    "currentRms": 5.1,
    "state": "On"
  }, {
    "outletName": "Nas Server 1,
    "outletUuid": "8-858993459",
    "pwrOnState": "on",
    "outletIndex": 8,
    "currentRms": 7.2,
    "state": "On"
  }, {
    "outletName": "Outlet 2,
    "outletUuid": "8-858993459",
    "pwrOnState": "on",
    "outletIndex": 2,
    "currentRms": 1.2,
    "state": "On"
  }...]
}
```

### Remove a Group
```
DELETE /api/groups/:group_id                     Permanently remove group.
```

#### Request
Follows [Authentication Scheme](#Authentication-Scheme)
`:group_id` should be replaced with `Group UUID`
```javascript
{} // no request parameters
```
##### Example Request
```bash
curl -X DELETE 'http://192.168.1.100/api/groups/0u912084N12dM5b05GU' \
--header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k'
```

#### Response
Expected Response is a **200 HTTP Response code** for successfull permanent deletion of group.

---

## Network
The network endpoint represents all network related configurations/settings. `/api/network` returns a json object with all settings.
`/api/network/sshKeys` endpoint represents ssh keys for passwordless ssh authentication.
```
GET    /api/network                              Returns all network related configuration values
GET    /api/network/sshKeys                      Returns list of ssh keys
POST   /api/network/sshKeys                      Create an SSH Key for passwordless login
DELETE /api/network/sshKeys/:key_id              Permanently remove an SSH Key for passwordless login
GET    /api/network/autoping/events              Returns all autoping related events/actions
POST   /api/network/autoping/events/:event_id    Create event triggered by autoping timeout
PUT    /api/network/autoping/events/:event_id    Modifies an autoping related event/action
DELETE /api/network/autoping/events/:event_id    Permanently remove autoping related event/action
```
### Network Object
`dhcpAssignedIp` only shows up when successfully assigned IP address from DHCP server at `gatewayIp` with `ipAssign` is `"dhcp"`
If `ipAssign` is `dhcp`. Static IP does not apply.
```javascript
{
  "autopingEnabled": true,
  "autopingTimeout": 5,
  "autopingNetworkActiveEnabled": true,
  "dhcpAssignedIp": "192.168.1.126",
  "disableSshKeyLogin": false,
  "disableSshPassLogin": false,
  "gatewayIp": "192.168.1.1",
  "ipAssign": "dhcp",
  "macAddr": "80:1F:12:41:ED:28",
  "ntpEnabled": true,
  "ntpHost": "pool.ntp.org",
  "primaryDns": "8.8.8.8",
  "secondaryDns": "8.8.4.4",
  "remoteSyslogEnabled": false,
  "sourceIp":"192.168.1.0",
  "sourceSubnet":"255.255.255.0",
  "sshEnabled": true,
  "sshIdleTimeout": 0,
  "sshPort": 22,
  "sshDisableKeys": false,
  "sshDisablePass": false,
  "staticIp": "192.168.1.100",
  "subnetMask": "255.255.255.0",
  "syslogEnabled": true,
  "syslogRemoteLogEnabled": true,
  "syslogRemoteLogIp": "192.168.1.181",
  "telnetEnabled": true,
  "telnetPort": 23,
  "webEnabled": true,
  "webTimeout": 0,
  "webMaxUsers": 0,
  "webPort": 80,
  "webSslEnabled": false
}
```
| Key | Type | Description |
|:-----|:----|:------------|
|**autopingEnabled**|*boolean*|Turns on or off AutoPing Reboot feature|
|**autopingTimeout**|*number*|Number of seconds without ping response before a ping is considered unsuccessfully timed out|
|**autopingNetworkActiveEnabled**|*boolean*|Enable or disables whether autoping will automatically be disabled without a network connection|
|**ipAssign**|*string*|Either "dhcp" or "static" are valid strings. DHCP assignment will show IP through dhcpAssignedIp. Static IP is set with staticIp|
|**dhcpAssignedIp**|*number*|IP Address assigned by DHCP server. Only available if ip assignment is successful and ipAssign is set to dhcp|
|**staticIp**|*string*|IP Address that the PDU will respond to if ipAssign is set to "static"|
|**subnetMask**|*string*|Subnet Mask that divides IP address into network address and host address|
|**gatewayIp**|*string*|IP Address of device on network which sends local network traffic to other networks|
|**macAddr**|*string*|Mac address string for network interface card|
|**primaryDns**|*string*|IP Address of Primary DNS Server|
|**secondaryDns**|*string*|IP Address of Secondary DNS Server|
|**sourceIp**|*string*|IP Address used in conjunction with sourceSubnet to restrict access to PDU. With a subnet of 255.255.255.255, only allowable IP is sourceIp exactly|
|**sourceSubnet**|*string*|Subnet Mask used in conjunction with IP Address to restrict access to PDU. With subnet mask of 255.255.255.0 and sourceIp of 192.168.1.0 will allow all hosts inside 192.168.1.XX network|
|**ntpHost**|*string*|NTP Host URL/IP to reference for time synchronization|
|**ntpEnabled**|*boolean*|Turns on or off NTP time synchronization. Requires network connection|
|**sshEnabled**|*boolean*|Turns on or off SSH Server|
|**sshIdleTimeout**|*number*|Number of seconds before SSH server auto disconnects from idle connection|
|**sshPort**|*number*|Port to reach SSH Server|
|**sshDisableKeys**|*boolean*|Disable or enable key based logins|
|**sshDisablePass**|*boolean*|Disable or enable password based logins|
|**syslogEnabled**|*boolean*|Turn on or off Syslog logging|
|**syslogRemoteLogEnabled**|*boolean*|Turn on or off whether remote syslog logging|
|**syslogRemoteLogIp**|*string*|Host IP for Remote Syslog Server|
|**telnetEnabled**|*boolean*|Turns on or off Telnet Server|
|**telnetPort**|*number*|Port to reach Telnet Server|
|**webEnabled**|*boolean*|Turns on or off Webserver|
|**webTimeout**|*number*|Number of seconds before a user if logged out of web browser interface during idle|
|**webMaxUsers**|*number*|Max number of users logged in to web browser|
|**webPort**|*number*|Port to reach Webserver|
|**webSslEnabled**|*number*|Turns on or off SSL security for webserver. If enabled, webserver is reachable via HTTPS instead of HTTP|

### Get Network Information
Returns a network object
```
GET    /api/network                              Returns all network related configuration values
```
#### Request
Follows [Authentication Scheme](##Authentication).
```javascript
{} // no request parameters
```

##### Example Request
```bash
curl 'http://192.168.1.100/api/network' \
  --header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
```

#### Response
Expected response is a [Network Object](#Network-Object)
```javascript
{
  "autopingEnabled": true,
  "autopingTimeout": 5,
  "autopingNetworkActiveEnabled": true,
  "dhcpAssignedIp": "192.168.1.126",
  "disableSshKeyLogin": false,
  "disableSshPassLogin": false,
  "gatewayIp": "192.168.1.1",
  "ipAssign": "dhcp",
  "macAddr": "80:1F:12:41:ED:28",
  "ntpEnabled": true,
  "ntpHost": "pool.ntp.org",
  "primaryDns": "8.8.8.8",
  "secondaryDns": "8.8.4.4",
  "remoteSyslogEnabled": false,
  "sourceIp":"192.168.1.0",
  "sourceSubnet":"255.255.255.0",
  "sshEnabled": true,
  "sshIdleTimeout": 0,
  "sshPort": 22,
  "sshDisableKeys": false,
  "sshDisablePass": false,
  "staticIp": "192.168.1.100",
  "subnetMask": "255.255.255.0",
  "syslogEnabled": true,
  "syslogRemoteLogEnabled": true,
  "syslogRemoteLogIp": "192.168.1.181",
  "syslog_port":514,
  "syslog_protocol":"RFC5424",
  "telnetEnabled": true,
  "telnetPort": 23,
  "webEnabled": true,
  "webTimeout": 0,
  "webMaxUsers": 0,
  "webPort": 80,
  "webSslEnabled": false
}
```



## System
### System Object
```javascript
{
  "tempSensorOffset": 0,
  "tempHumidityOffset": 0,
  "lcdEnabled": true,
  "lcdOrientation": 0,      
  "lcdTimeout": 20,  // screensaver ?
  "lcdBacklightTimeout": 0,
  "lcdBrightness": 6,  
  "scheduledEventsEnabled": true,
  "deviceRebootSequentialDelay": 3,
  "deviceHostName": "SynLink PDU",
  "firmwareVersion": "1.3.1 Build 20897082 rel.123(4212)",
  "hardwareVersion": "SynLink v1.0"
}
```
### Get System Info
#### Request
##### Example Request
#### Response


## Configure
### Modify Network Settings
#### Request
```javascript
{
  "autopingEnabled": true,
  "autopingTimeout": 5,
  "autopingNetworkActiveEnabled": true,
  "disableSshKeyLogin": false,
  "disableSshPassLogin": false,
  "gatewayIp": "192.168.1.1",
  "ipAssign": "dhcp",
  "ntpEnabled": true,
  "ntpHost": "pool.ntp.org",
  "primaryDns": "8.8.8.8",
  "secondaryDns": "8.8.4.4",
  "remoteSyslogEnabled": false,
  "sourceIp":"192.168.1.0",
  "sourceSubnet":"255.255.255.0",
  "sshEnabled": true,
  "sshIdleTimeout": 0,
  "sshPort": 22,
  "sshDisableKeys": false,
  "sshDisablePass": false,
  "staticIp": "192.168.1.100",
  "subnetMask": "255.255.255.0",
  "syslogEnabled": true,
  "syslogRemoteLogEnabled": true,
  "syslogRemoteLogIp": "192.168.1.181",
  "syslog_port":514,
  "syslog_protocol":"RFC5424",
  "telnetEnabled": true,
  "telnetPort": 23,
  "webEnabled": true,
  "webTimeout": 0,
  "webMaxUsers": 0,
  "webPort": 80,
  "webSslEnabled": false
}
```
##### Example Request
```
```
#### Response
### Modify System Settings
#### Request
```javascript
{
  "tempSensorOffset": 0,
  "tempHumidityOffset": 0,
  "lcdEnabled": true,
  "lcdOrientation": 0,      
  "lcdTimeout": 20, 
  "lcdBacklightTimeout": 0,
  "lcdBrightness": 6,  
  "scheduledEventsEnabled": true,
  "deviceRebootSequentialDelay": 3
}
```
##### Example Request
```bash
curl 'http://192.168.1.100/api/configure/system' \
  --header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
  --data '{ "lcdOrientation":90, "lcdBrightness":2, "scheduledEventsEnabled":false }'
```
#### Response
```javascript
{
  "tempSensorOffset": 0,
  "tempHumidityOffset": 0,
  "lcdEnabled": true,
  "lcdOrientation": 90,      
  "lcdTimeout": 20,
  "lcdBacklightTimeout": 0,
  "lcdBrightness": 2,  
  "scheduledEventsEnabled": false,
  "deviceRebootSequentialDelay": 3
}
```
---
### Modify Log Settings
#### Request
##### Example Request
#### Response

## Users
### User Object
### Get List of Users
### Get User's SSH Keys
Returns a list of SSH Key objects
```
GET    /api/network/sshKeys                              Returns a list of SSH Keys
```
#### Request
Follows [Authentication Scheme](##Authentication).
```javascript
{} // no request parameters
```

##### Example Request
```bash
curl 'http://192.168.1.100/api/network/sshKeys' \
  --header 'Cookie: SPID=j2d36cG2ciHKDDqFc3k' \
```

#### Response
Expected response is a list of SSH Keys. Key attribute is the SSH Key fingerprint.
```json
[{
  "key": "97:f0:86:00:02:96:8f:28:f6:df:38:f2:c8:de:4b:69",
  "name": "Lab PC 1"
}, {
  "key": "4a:dc:9a:66:c3:14:30:c3:88:9a:9e:a6:ed:36:ca:b6",
  "name": "Lab PC 2"
}]
```

### Create User SSH Key
Create a network SSH Key for login. 

#### Request

##### Example Request

#### Response

### Remove Network SSH Key 

#### Request

##### Example Request

#### Response

---


## Sensors

### Sensor Objects
#### Temperature & Humidity Sensor

### Get List Connected Sensors
#### Request
##### Example Request
#### Response

### Get Individual Sensor Details
#### Request
##### Example Request
#### Response

### Modify Individual Sensor Info
#### Request
##### Example Request
#### Response

---


## Events
### Event Object
| Event                           | Event Code | Parameter 1                            | P1 Type  | Parameter 2                | P2 Type  | Parameter 3                | P3 Type  |
|:------------------------------- |:---------- |:-------------------------------------- |:-------- |:-------------------------- |:-------- |:-------------------------- |:-------- |
| **autopingTimeout**             | **12**     | Target IP Address                      | *string* | Number of Attempts         | *number* | NA                         |          |
| **device3phaseImbalance**       | **13**     | Imbalance Percentage                   | *number* | NA                         |          | NA                         |          |
| **deviceAnyBreakerTrip**        | **14**     | NA                                     |          | NA                         |          | NA                         |          |
| **bankCurrentMaxThreshold**     | **15**     | Current Threshold in milliAmperes      | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* |
| **bankCurrentMinThreshold**     | **16**     | Current Threshold in milliAmperes      | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* |
| **bankVoltageMaxThreshold**     | **17**     | Voltage Threshold in Volts             | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* |
| **bankVoltageMinThreshold**     | **18**     | Voltage Threshold in Volts             | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* |
<!-- | **bankPowerfactorMaxThreshold** | **19**     | Power factor in ratio between 1-100    | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* | -->
| **bankPowerfactorMinThreshold** | **20**     | Power factor in ratio between 1-100    | *number* | Bank UUID                  | *string* | Num seconds past threshold | *number* |
| **bankBreakerTrip**             | **21**     | Bank UUID                              | *string* | NA                         |          | NA                         |          |
| **outletCurrentMaxThreshold**   | **22**     | Current Threshold in milliAmperes      | *number* | Outlet UUID                | *string* | Num seconds past threshold | *number* |
| **outletCurrentMinThreshold**   | **23**     | Current Threshold in milliAmperes      | *number* | Outlet UUID                | *string* | Num seconds past threshold | *number* |
| **groupCurrentMaxThreshold**    | **24**     | Current Threshold in milliAmperes      | *number* | Group UUID                 | *string* | Num seconds past threshold | *number* |
| **groupCurrentMinThreshold**    | **25**     | Current Threshold in milliAmperes      | *number* | Group UUID                 | *string* | Num seconds past threshold | *number* |
| **temperature1MaxThreshold**    | **26**     | Temperature threshold in celsius       | *number* | Num seconds past threshold | *number* | NA                         |          |
| **temperature1MinThreshold**    | **27**     | Temperature threshold in celsius       | *number* | Num seconds past threshold | *number* | NA                         |          |
| **temperature2MaxThreshold**    | **28**     | Temperature threshold in celsius       | *number* | Num seconds past threshold | *number* | NA                         |          |
| **temperature2MinThreshold**    | **29**     | Temperature threshold in celsius       | *number* | Num seconds past threshold | *number* | NA                         |          |
| **humidity1MaxThreshold**       | **30**     | Humidity threshold in percentage 1-100 | *number* | Num seconds past threshold | *number* | NA                         |          |
| **humidity1MinThreshold**       | **31**     | Humidity threshold in percentage 1-100 | *number* | Num seconds past threshold | *number* | NA                         |          |
| **humidity2MaxThreshold**       | **32**     | Humidity threshold in percentage 1-100 | *number* | Num seconds past threshold | *number* | NA                         |          |
| **humidity2MinThreshold**       | **33**     | Humidity threshold in percentage 1-100 | *number* | Num seconds past threshold | *number* | NA                         |          |
| **scheduledTime**               | **34**     | Time String in (HH:MM) Military time   | *string* | NA                         |          | NA                         |          |
| **scheduledInterval**           | **35**     | Number minutes for interval execution  | *number* | NA                         |          | NA                         |          |
| **userWebLogin**                | **36**     | User UUID                              | *string* | NA                         |          | NA                         |          |
| **userWebLogout**               | **37**     | User UUID                              | *string* | NA                         |          | NA                         |          |
| **userAdded**                   | **38**     | User UUID                              | *string* | NA                         |          | NA                         |          |
| **userRemoved**                 | **39**     | User UUID                              | *string* | NA                         |          | NA                         |          |

### Action Object
| Action           | Action Code | Parameter 1    | P1 Type  | Parameter 2                                                  | P2 Type   |
|:---------------- |:----------- |:-------------- |:-------- |:------------------------------------------------------------ |:--------- |
| **switchOutlet** | 11          | Outlet UUID    | *string* | Outlet State. true for on                                    | *boolean* |
| **rebootOutlet** | 12          | Outlet UUID    | *string* | Reboot Cycle Time in seconds. Num seconds off before back on | *number*  |
| **switchGroup**  | 13          | Group UUID     | *string* | Outlet States. true for on                                   | *boolean* |
| **rebootGroup**  | 14          | Group UUID     | *string* | Reboot cycle time. Num seconds in off state.                 | *number*  |
| **email**        | 15          | Email Address  | *string* | NA                                                           |           |
| **webHook**      | 16          | IP Address/URL | *string* | NA                                                           |           |
| **syslog**       | 17          | NA             |          | NA                                                           |           |
| **snmpTrap**     | 18          | NA             |          | NA                                                           |           |

### Get List of Events
#### Request
##### Example Request
#### Response

### Create an Event
#### Request
##### Example Request
#### Response

### Modify an Event
#### Request
##### Example Request
#### Response

### Remove an Event
#### Request
##### Example Request
#### Response


## Other Endpoints
```
POST   /api/network/restart                      Restart network interface card, and all network related processes with most recent configurations
POST   /api/system/factory-reset                 Factory resets entire PDU, then reboots.
POST   /api/system/reboot    
```

### Restart Network
#### Request
##### Example Request
#### Response

### Reboot PDU
#### Request
##### Example Request
#### Response

### Factory Reset PDU
#### Request
##### Example Request
#### Response


# All Endpoints
Outlet and Group related routes are not available if PDU does not support outlet switching or outlet current metering.

```bash
GET    /api/device                               Returns an overview of all banks, groups, outlets, and networking configurations
GET    /api/device/events                        Returns device event related information
POST   /api/device/events                        Create event triggered by device specific activity.
GET    /api/device/events/:event_id              Modifies device related event or action
DELETE /api/device/events/:event_id              Permanently removes device related event/action.

GET    /api/banks                                Return list of all banks and their outlets
GET    /api/banks/:bank_id                       Returns individual bank information, including related events and actions
PUT    /api/banks/:bank_id                       Modifies individual bank
GET    /api/banks/:bank_id/events                Returns individual bank event related information
POST   /api/banks/:bank_id/events                Create event triggered by bank activity.
GET    /api/banks/:bank_id/events/:event_id      Modifies bank related event or action
DELETE /api/banks/:bank_id/events/:event_id      Permanently removes bank related event/action.

GET    /api/outlets                              Returns list of all outlets
GET    /api/outlets/:outlet_id                   Returns individual outlet information, including related events & actions
PUT    /api/outlets/:outlet_id                   Modifies individual outlet
GET    /api/outlets/:outlet_id/events            Return individual outlet event related information
POST   /api/outlets/:outlet_id/events            Create event triggered by outlet activity
PUT    /api/outlets/:outlet_id/events/:event_id  Modifies outlet related event or action
DELETE /api/outlets/:outlet_id/events/:event_id  Permanently removes outlet related event/action.

GET    /api/groups                               Returns list of all outlet groups
POST   /api/groups                               Creates a group of outlets.            
GET    /api/groups/:group_id                     Returns individual group information, including all related events/actions
PUT    /api/groups/:group_id                     Modifies individual group
DELETE /api/groups/:group_id                     Permanently remove group.
GET    /api/groups/:group_id/events              Returns group related events/actions
POST   /api/groups/:group_id/events              Create event triggered by group activity
PUT    /api/groups/:group_id/events/:event_id    Modifies a group related event/action
DELETE /api/groups/:group_id/events/:event_id    Permanently remove group related event/action

GET    /api/network                              Returns all network related configuration values
GET    /api/network/autoping/events              Returns all autoping related events/actions
POST   /api/network/autoping/events/:event_id    Create event triggered by autoping timeout
PUT    /api/network/autoping/events/:event_id    Modifies an autoping related event/action
DELETE /api/network/autoping/events/:event_id    Permanently remove autoping related event/action

GET    /api/system                               Returns system related configurations and settings. Return # connected users.
GET    /api/system/scheduling/events             Returns all scheduled events/actions
POST   /api/system/scheduling/events             Returns all scheduled events/actions
PUT    /api/system/scheduling/events/:event_id   Modifies a scheduled event/action
DELETE /api/system/scheduling/events/:event_id   Permanently removes a schedule event/action

PUT    /api/configure/network                    Modifies network configurations
PUT    /api/configure/system                     Modifies system level configurations. Including sensor configurations, user configurations.
PUT    /api/configure/logs                       Modifies logging configuration.
GET    /api/configure/backup                     Downloads current configurations as .bin file
POST   /api/configure/backup                     Upload backup .bin file for changing configuration.

GET    /api/sensors                              Returns a list of any connected sensors
GET    /api/sensors/:sensor_id                   Returns details information about one particular sensor. Including events
PUT    /api/sensors/:sensor_id                   Modifies a particular sensor. (name)
GET    /api/sensors/:sensor_id/events            Returns all sensor related events
PUT    /api/sensors/:sensor_id/events/:event_id  Modifies a sensor related event
DELETE /api/sensors/:sensor_id/events/:event_id  Permanently remove a sensor related event

GET    /api/syslog                               Returns log of all syslog messages
DELETE /api/syslog                               Deletes logs of all syslog messages
GET    /api/power_logs                           Returns log of all bank, outlet, and group level power/energy measurements
DELETE /api/power_logs                           Deletes logs of all bank, outlet, and group level power/energy measurements

GET    /api/users                                Returns a list of all users
POST   /api/users                                Creates a new user
PUT    /api/users/:user_id                       Modify a particular user
GET    /api/users/:user_id/sshKeys               Returns list of ssh keys
POST   /api/users/:user_id/sshKeys               Create an SSH Key for passwordless login
DELETE /api/users/:user_id/sshKeys/:key_id       Permanently remove an SSH Key for passwordless login
POST   /api/users/:user_id/PAT                   Create a PAT for a particular user
DELETE /api/users/:user_id/PAT                   Permanently remove a PAT for a particular user         
DELETE /api/users/:user_id                       Permanently remove a user

POST   /api/network/restart                      Restart network interface card, and all network related processes with most recent configurations
POST   /api/system/restart                       Restart SynLink PDU. 
POST   /api/system/factoryReset                  Factory resets entire PDU, then reboots.
POST   /api/system/firmwareUpdate                Upload firmware .bin files for firmware update. Requires system reboot.

```


<!-- 

```
GET    /api/network/ip                           Returns all network IP related configuration
GET    /api/network/web                          Returns all network IP related configuration
GET    /api/network/ssh                          Returns all network SSH related configuration
GET    /api/network/telnet                       Returns all network Telnet related configuration
GET    /api/network/snmp                         Returns all network SNMP related configuration
GET    /api/network/syslog                       Returns all network Syslog related configuration
GET    /api/network/smtp                         Returns all network SMTP related configuration
GET    /api/network/ntp                          Returns all network NTP related configuration
GET    /api/network/autoping                     Returns all network AutoPing Reboot related configuration

GET    /api/system/lcd                           Returns lcd related configurations
GET    /api/system/scheduling                    Return scheduling related configurations

GET    /api/bank_logs                            Returns log of all bank level power/energy measurements
DELETE /api/bank_logs                            Deletes logs of all bank level power/energy measurements
GET    /api/outlet_logs                          Returns log of all outlet level power measurements
DELETE /api/outlet_logs                          Deletes logs of all outlet level power measurements
GET    /api/group_logs                           Return log of all group related power measurements
DELETE /api/group_logs                           Deletes logs of all group related power measurements
``` -->

#### Prototyping
**GET**    /api/device
  numBanks
  numOutlets
  numInlets
  inletPlug
  outletPwrMeasurementsSupported
  outletSwitchingSupported
  controllerSerialNumber
  enclosureSerialNumber
  atsSupported
  
  
**GET**    /api/inlet
  powerCapacity
  
  currentRmsA         current draw line A (3 Phase)
  currentRmsB         current draw line B (3 Phase)
  currentRmsC         current draw line C (3 Phase)
  3PhaseOutOfBalance  (3 Phase)
  
  breakerAB           breaker bool (3 Phase + breaker)
  breakerBC           breaker bool (3 Phase + breaker)
  breakerAC           breaker bool (3 Phase + breaker)
  
  breakerA            breaker bool (1 phase + breaker)
  breakerB            breaker bool (1 phase + breaker)
  
  atsInletActive      "a" or "b"
  atInletAReady       bool
  atsInletBReady      bool
  
  inletACurrentRms
  inletBCurrentRms
  
```
synaccess
  synlink
    device
      numBanks
      numOutlets
      numInlets
      numLines
      inletPlug
      outletPwrMeasurementsSupported
      outletSwitchingSupported
      controllerSerialNumber
      enclosureSerialNumber

    inlets
      inletStatusTable
        powerCapacity
        inletPlug
        numPoles
        3PhaseBalancePercentage

    lines
      linesStatusTable
        lineCurrentRms
        lineCurrentCapacity

    banks
      bankStatusTable
        bankSerialNumber
        bankVersion
        outletSwitchingSupported
        outletMeteringSupported
        currentRms
        voltageRms
        lineFrequency
        powerFactor
        reactance
        activePower
        reactivePower
        apparentPower
        bankName
        hasBreaker
        breakerStatus
      bankConfigTable

    outlets
      outletStatusTable
        outletName(rw)
        outletUuid(r)
        outletPwrOnState(rw)
        outletIndex(r)
        outletCurrentRms(r)
        outletState(rw)
        outletConnector
        outletMaxCurrent
      outletControlTable


    groups
      groupStatusTable
        groupName
        groupUuid
        
    autoTransferSwitch
      atsSupported
      atsActiveInput
      inputAStatus
      inputBStatus
    
    sensors[]

    logs[]

  lcd

  events
    autopingTimeout
    device3phaseImbalance
    deviceAnyBreakerTrip
    bankCurrentMaxThreshold
    bankCurrentMinThreshold
    bankVoltageMaxThreshold
    bankVoltageMinThreshold
    bankPowerfactorMaxThreshold
    bankPowerfactorMinThreshold
    bankLineFrequencyMaxThreshold
    bankLineFrequencyMinThreshold
    lineCurrentMaxThreshold
    lineCurrentMinThreshold
    atsInletLostPower
    bankBreakerTrip
    outletCurrentMaxThreshold
    outletCurrentMinThreshold
    groupCurrentMaxThreshold
    groupCurrentMinThreshold
    temperature1MaxThreshold
    temperature1MinThreshold
    humidity1MaxThreshold
    humidity1MinThreshold
    temperature2MaxThreshold
    temperature2MinThreshold
    humidity2MaxThreshold
    humidity2MinThreshold
    scheduledTime
    scheduledInterval
    energyMaxthreshold




  
**GET**    /api/banks
**PUT**    /api/banks/:bank_id
**GET**    /api/outlets
**PUT**    /api/outlets/:outlet_id
**GET**    /api/groups
**POST**   /api/groups
**PUT**    /api/groups/:group_id
**GET**    /api/network
**GET**    /api/system
**PUT**    /api/configure/network
**PUT**    /api/configure/system
**PUT**    /api/configure/logs
**GET**    /api/sensors
**PUT**    /api/sensors/:sensor_id
**GET**    /api/syslog
**GET**    /api/power_logs