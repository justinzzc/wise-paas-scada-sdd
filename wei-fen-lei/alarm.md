# Alarm

### SPEC

* condition
  * table固定有兩個欄位, condition_lower/condition\_upper_
  * above
    * 只存upper
  * below
    *  只存lower
  * in range
    * 兩個都要存
  * equal
    * upper/lower都存一樣的
  * out range
    * 兩個都要存
* alarm log那一頁
  * 那些欄位可以直接從stacy那邊給我，那些是要我這裡用dao join出來
  * time跟ack time一定是stacy那邊給我
* current alarm list跟alarm log幾乎依樣
  * 多了value
  * 多了發ack的按鈕跟icon

### FAQ

* 什麼是alarm code?
* alarm code有什麼規則嗎?
* alarm message的長度限制?
* new alarm的tags, 是指要套用這個condition rule的tag有哪些嗎?
* notification是我們定義好嗎?user不可以從portl自己設定對嗎
* nitifucation要從哪裡處trigger, worker/portal?
* 要做update嗎?如果要 能改跟不能改的有那些
* current alarm list裡，只秀tag就好了嗎?這樣會不會分不清是哪個scada/device?
* 舊的那一套alarm 機制是不是就棄用了, 像是離散有8個priority....類比有hh/ll等等的
  * 改成很單純的六種condition配合user自己設定的upper and lower limit
* 文字點是否要做?開會時是講不用 可是eventlog後來變成有



