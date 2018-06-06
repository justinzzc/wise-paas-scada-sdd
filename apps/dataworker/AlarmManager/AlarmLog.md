### MongoDB Schema \(scada\_AlarmLog\) {#schema_AlarmLog}

| Column Name | Type | Description | Index | Notes |
| :--- | :--- | :--- | :--- | :--- |
| a | Number | alarm Id | Y ||
| s | String | scada Id | Y ||
| d | String | device Id | Y ||
| t | String | tag Name | Y ||
| v | Object | alarm value || default: null |
| ts | Date | trigger or clear time || default: null |
| ack | Date | ack time || default: null|
| clear | Date | clear time || default: null|

### AlarmStatus Object {#obj_AlarmStatus}
| Column Name | Type |
| :--- | :--- |
| alarmId | Number |
| scadaId | String |
| deviceId | String |
| tagName | Strin
| value | Object |
| ts | Date |
| ackTs | Date |
| clearTs | Date|

### Method (alarmLogHelper.js)
#### getAlarmLog
* Purpose: get alarm's status
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| params | Object | filter Object |
| params.alarmId | Number | filter alarmId (totally compare) |
| params.scadaId | String | filter scadaId(totally compare) |
| params.startTs | Date | filter alarm ts |
| params.endTs | Date | filter alarm ts. Default: new Date()|
| params.count | Number | retrieve log count|
| params.order | Boolean | data order by ts |

* Output:
  * Array of [AlarmStatus](#obj_AlarmStatus) Object
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    ts: {$gte: startTs, $lte: endTs},
    }
  2. sort = {
    ts: order
    }
  3. limit = {
    limit: count
    }
  2. Organize the output format = {
    alarmId: $a,
    scadaId: $s,
    deviceId: $d,
    tagName: $t,
    value: $v,
    ackTs: $ack,
    clearTs: $clear,
    ts: $ts
    }
    
#### insertTriggerAlarmLog
* Purpose: insert alarm trigger logs
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| records | Array | array of record |
| record.alarmId | Number | alarm Id |
| record.scadaId | String | scada Id |
| record.deviceId | String | device Id |
| record.tagName | String | tag Name |
| record.ts | Date | alarm trigger time. default: new Date() |
| record.value | Object | tag value |

* Logical description:
  1. doc = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName,
    ts: ts,
    v: value
    }
  2. insertMany(docs)

#### insertAckAlarmLog
* Purpose: record ack record time
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |
| ts | Date | default: new Date() |
* Output:
  * if document udated or not.
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName
    }
  2. update = {
    ack: ts
    }
  3. [_updateLastAlarmLog(filter, update)](#func_updateLastAlarmLog)

#### insertClearAlarmLog
* Purpose: record clear record time
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| alarmId | Number | alarm Id |
| scadaId | String | scada Id |
| deviceId | String | device Id |
| tagName | String | tag Name |
| ts | Date | default: new Date() |
* Output:
  * if document udated or not.
* Logical description:
  1. filter = {
    a: alarmId,
    s: scadaId,
    d: deviceId,
    t: tagName
    }
  2. update = {
    clear: ts
    }
  3. [_updateLastAlarmLog(filter, update)](#func_updateLastAlarmLog)

#### _updateLastAlarmLog {#func_updateLastAlarmLog}
* Purpose: update alarm log
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| filter | Object | filter |
| update | Object | update object |

* Logical description:
  1. findOneAndUpdate({rawResult: true})

#### deleteAlarmLog
* Purpose: delete alarm log
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
  2. _deleteAlarmLog(filter)

#### _deleteAlarmLog {#func_deleteAlarmLog}
* Purpose: delete alarm log with filter
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| filter | Object | filter object |

* Logical description:
  1. remove(filter)


