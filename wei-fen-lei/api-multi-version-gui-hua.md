# API Multi Version - 規劃

* 核心: 不同版本的api 輸入輸出可能不同，至於api實際邏輯，user並不知道
* 需要做一支TOOL
  * 執行後就能產生新板的目錄, 並將舊的檔案複製且改名一份
* const.js不用整份複製給多版本
  * 但可以在裡面定義多版本
* gen_yml要處理yml tag, ex. group\_V\_1\_1 =&gt; group v1.1_



