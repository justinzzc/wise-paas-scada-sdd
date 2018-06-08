### Method (alarm-manager.js)
| Function Name | Description |
| :---: | :---: |
| [init](#func_alarm_init) | Init Alarm Manager |
| [quit](#func_alarm_quit) | Quit Alarm Manager |
| [getAlarmLog](#func_alarm_getAlarmLog) | Get alarms' log |
| [getAlarmStatus](#func_alarm_getAlarmStatus) | Get alarms' status |
| [triggerAlarm](#func_alarm_triggerAlarm) | Trigger alarm |

#### init {#func_alarm_init}
* Purpose: Init Alarm Manager
* Input:

| Name | Data Type | Description |
| :---: | :---: | :---: |
| options | Object | connection options |
| options mongoConf | Object | mongo connection config |
| options.mqttConf | Object | mqtt conneciton config |

* Logical description:
  1. if have mongoConf and mongo not connected, then connect mongo
  2. if have mqttConf and mqtt not connected, then connect mqtt

#### quit{#func_alarm_quit}
* Purpose: Quit Alarm Manager
* Logical description:
  1. if mongo is connected, then disconnect mongo
  2. if mqtt is connected, then disconnect mqtt

#### getAlarmLog{#func_alarm_getAlarmLog}
* Purpose: Get alarms' log
* Input:
| Name | Data Type | Description |
| :---: | :---: | :---: |
| params | Object | filter object |
| params.alarmId | Number | filter alarm Id |
| params.scadaId | String | filter scada Id |
| params.startTs | Date | filter alarm trigger ts |
| params.endTs | Date | filter alarm trigger ts |
| params.count | Number | retrieve count. default: 1 |
| params.order | Boolean | data order. default: true |
* output
- Array of [Alarm Log](#obj_AlarmLog) Object

* Logical description:
  1. check startTs and endTs format
  2. alarmLog.getAlarmLog({alarmId, scadaId, startTs, endTs, count, order})

#### getAlarmStatus{#func_alarm_getAlarmStatus}
* Purpose: Get alarms' status
* Input:
| Name | Data Type | Description |
| :---: | :---: | :---: |
| params | Object | filter object |
| params.alarmId | Number | filter alarm Id |
| params.scadaId | String | filter scada Id |
| params.deviceId | String | filter device Id |
| params.tagName | String | filter tagName |
| params.acked | Boolean | filter acked |
| params.status | Boolean | filter status |
* output
- Array of [Alarm Status](#obj_AlarmStatus) Object

* Logical description:
  1. check startTs and endTs format
  2. alarmLog.getAlarmLog({alarmId, scadaId, startTs, endTs, count, order})





