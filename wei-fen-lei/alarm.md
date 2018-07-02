# Alarm 開發文件

### DB Schema \(config\) {#db-schema}

* alarm\_list

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y | Y | Y |  |
| scada\_id | varchar\(36\) | Y |  |  |  |  |
| code | varchar\(16\) | Y |  |  |  | code在同一scada下不能重覆，在程式裡檔 |
| message | varchar\(256\) | Y |  |  |  |  |
| condition\_type | integer | Y |  |  |  | {1: above, 2: below, 3: equal, 4: in range, 5: out range} |
| lower\_limit | double |  |  |  |  |  |
| upper\_limit | double |  |  |  |  |  |
| instance\_launched | boolean | Y |  |  |  | default: false |

* alarm\_tag

| Column Name | Type | Not Null | PK | auto increment | Index | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| alarm\_id | integer | Y | Y |  | Y |  |
| device\_id | varchar\(256\) | Y | Y |  | Y |  |
| tag\_name | varchar\(128\) | Y | Y |  | Y |  |

### Requirement

* **code規則                                                  **

  * 同一個scada下的code不能重覆，不同scada下可以定義相同的code

* **condition**

  * above \(大於等於某個值，就觸發警報\)
    * if \(x &gt;= upperLimit\) { return true }
  * below \(小於等於某個值，就觸發警報\)
    * if \(x &lt;= lowerLimit\) { return true }
  * equal
    * if \(x === lowerLimit && x === upperLimit\)
  * in range

    * if \(x &gt; lowerLimit && x &lt; upperLimit\) { return true }

  * out range

    * if \(x &gt; upperLimit \|\| x &lt; lowerLimit\) { return true}

* **Permission**

  * check right到device

* **API**

  * POST /Alarms
  * GET /Alarms
  * PUT /Alarms/{alarm\_id}

    * 如果alarm的instanceLaunched是true才call utility function

  * DELETE /Alarms/{alarm\_id}

  * POST /Alarms/data

  * POST /Alarms/status

  * POST /Alarms/syncInstance

  * POST /Alarms/ack

* **utility alarm delete**

  * \[{scadaId: 'xxxx'}\]
    * 刪除該scada下的所有alarm
  * \[{scadaId:'xxxx', alarmId: 'xxxx'}\]
    * 刪除整個instance
  * \[{scadaId: 'xxx', alarmId: 'xxx', tags: \[{deviceId:'xxx', tagName: 'xxx'}\]}\]
    * update用途，把tag移除該alarm的監控
    * 只改tags,沒改config
    * 如果是config/tags都有改, 就直接call update
      * config/tags一起帶

* **filter**

  * alarm log
    * 選tag就要給device
    * 至少要給scadaId
  * alarm status
    * 選tag就要給devcie
    * 可以都不給

* **連動刪除**

  * project/scada/device/tag

### FAQ

* 文字點是否要做?

  * 不用

* 刪除config要刪除log嗎?

  * log一律都不要刪，但為了讓record有意義，所以在保留code+message

* 相同alarmId/scadaId的config\(但可能lowerLimit/upperLimit不一樣\)重複insert，utility那邊會怎處理?

  * 不處理



