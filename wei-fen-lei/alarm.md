# Alarm

### SPEC

* condition
  * table固定有兩個欄位, condition_lower/condition\_upper_
  * above
    * 只存upper
  * below
    * 只存lower
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
* 首頁的alarm數量 也要做一個api讓前端抓數量
* check right到device
* 在UI和API去擋code+message不可重複
* ### FAQ
* 什麼是alarm code?

  * 因為有些設備是有自訂的alarm code 想說讓她有個相互對應

* alarm code有什麼規則嗎?

  * string/not null/length:16

* alarm code跟alarm message有關聯嗎

  * 一個code對應一個messag

* alarm message的長度限制?
  * string/256
* notification是我們定義好嗎?
  * 先不做notification

* 要做update嗎?
  * 如果要做，需要討論下那些欄位能改或不能改
  * ex. 
* current alarm list裡，只秀tag就好了嗎?這樣會不會分不清是哪個scada/device?
* 舊的那一套alarm 機制是不是就棄用了, 像是離散有8個priority....類比有hh/ll等等的
  * 改成很單純的六種condition配合user自己設定的upper and lower limit
* 文字點是否要做?
  * 不用

* id code message 三個欄位設為pk的話
  * id會變成可以重複
  * 還是我schema的pk只設id
  * code/message不能重複我從API檔?
* 預留TEXT
* alarm ack的那個頁面 有要做filter嗎?



