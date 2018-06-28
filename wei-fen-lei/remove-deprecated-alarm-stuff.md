移除舊的alarm清單

## dbmanager

* **Vo \(model\)**

  * models/alarmAnalogVo.js
  * models/alarmDiscreteVo.js
  * models/alarmDetailVo.js

* **Dao functions**

  * TagDao.getAlarmAnalogTag
  * TagDao.getAlarmDiscreteTag
  * TagDao.getAlarmTags
  * TagDao.insertAlarmAnalogTag
  * TagDao.insertAlarmDiscreteTag
  * TagDao.updateAlarmAnalogTag
  * TagDao.updateAlarmDiscreteTag
  * TagDao.deleteAlarmTag

* **Column**

  * tagVo - alarmStatus
  * wholeAnalogTagVo - alarmStatus
  * wholeDiscreteTagVo - alarmStatus
  * wholeTextTagVo - alarmStatus

## dbmigrate

```
DROP TABLE IF EXISTS alarm_analog;
DROP TABLE IF EXISTS alarm_discrete;
ALTER TABLE tag_list DROP COLUMN IF EXISTS alarm_status;
```

## Postgresql Shema

* 刪除兩張表
  * alarm\_analog
  * alarm\_discrete
* 刪除tag_list的alarm_status欄位
* schema
  * https://coolgo0811.gitbooks.io/wise-paas-scada-software-development-document/content/apps/database.html

## Portal API

* tag跟alarm有關的code拿掉
  * common/models/tag.js
  * commion/models/tag.json
  * 其它相關models...



