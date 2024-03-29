### MongoDB Schema \(scada\_AlarmStatus\) {#schema_AlarmStatus}

| Column Name | Type | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- |
| a | Number | alarm Id | Y ||
| s | String | scada Id | Y ||
| d | String | device Id | Y ||
| t | String | tag Name | Y ||
| status | Boolean | alarm is trigger || default is false |
| acked | Boolean | is acked || default is false |
| ts | Date | trigger or clear time || default is null |
| ackTs | Date | ack time ||  default is null |
| v | Object | alarm value || default is null |

### AlarmStatus Object {#obj_AlarmStatus}
| Column Name | Type |
| :--- | :--- |
| alarmId | Number |
| scadaId | String |
| deviceId | String |
| tagName | Strin
| status | Boolean |
| acked | Boolean |
| ts | Date |
| ackTs | Date |
| value | Object |

### Method (alarmStatusHelper.js)
#### getAlarmStatus
* Purpose: get alarm's status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| params | Object | filter Object |
| params.alarmId | Number | filter alarmId (totally compare) |
| params.scadaId | String | filter scadaId(totally compare) |
| params.deviceId | String  | filter deviceId(totally compare) |
| params.tagName | String | filter tagName|
| params.acked | Boolean | filter acked |
| params.status | Boolean | filter status |

* Output:
  * Array of [AlarmStatus](#obj_AlarmStatus) Object
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    status: status,
    acked: acked
    }
  2. Organize the output format = {
    alarmId: $a,
    scadaId: $s,
    deviceId: $d,
    tagName: $t,
    value: $v,
    acked: $acked,
    status: $status,
    ackeTs; $ackeTs,
    ts: $ts
    }
    
#### initAlarmStatus
* Purpose: init alarm's status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |

* Logical description:
  1. findOneAndUpdate({upsert: true, setDefaultsOnInsert: true)

#### triggerAlarmStatus
* Purpose: trigger alarm's status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |
| ts | Date | default: new Date() |
* Output:
  * if document updated or not.
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    status: false,
    ts: null || ts < ts
    }
  2. update = {
    status: true,
    ts: ts
    }
  3. [_updateAlarmStatus(filter, update)](#func_updateAlarmStatus)

#### ackAlarmStatus
* Purpose: ack alarm status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |
* Output:
  * if document updated or not.
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    status: true,
    acked: false
    }
  2. update = {
    acked: true,
    ackTs: new Date()
    }
  3. [_updateAlarmStatus(filter, update)](#func_updateAlarmStatus)

#### clearAlarmStatus
* Purpose: clear alarm status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |
| ts | Date | default: new Date() |
* Output:
  * if document updated or not.
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    status: true,
    ts: ts < ts
    }
  2. update = {
    acked: false,
    status: false,
    ackTs: null
    }
  3. [_updateAlarmStatus(filter, update)](#func_updateAlarmStatus)

#### deleteAlarmStatus
* Purpose: delete alarm status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |

* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName
    }
  2. _deleteAlarmStatus(filter)

#### deleteAlarmStatusByAlarmId
* Purpose: delete alarm status by alarmId
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |

* Logical description:
  1. filter = {
    a: alarmId
    }
  2. _deleteAlarmStatus(filter)

#### deleteAlarmStatusByScadaId
* Purpose: delete alarm status by scadaId
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| scadaId | String | scada Id |

* Logical description:
  1. filter = {
    s: scadaId
    }
  2. _deleteAlarmStatus(filter)

#### _deleteAlarmStatus
* Purpose: delete alarm status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| filter | Object | filter Object |

* Logical description:
  1. remove(filter)
  
#### _updateAlarmStatus {#func_updateAlarmStatus}
* Purpose: update alarm status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| filter | Object | filter |
| update | Object | update object |

* Output:
  * if document updated or not.
* Logical description:
  1. findOneAndUpdate({rawResult: true})




