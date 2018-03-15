# wise-paas-scada-dbmigrate 

## FAQ

* 「Error: ERROR: Validate failed: Migration checksum mismatch for migration version 3」
  * 意思是 flyway\_schema\_history V3的checksum和當前local V3 file的checksum不同
  * 可能的原因是migrate V3成功後，有修改local的V3，當再次做migrate就會回報這錯誤
  * 最正確的解法應該是，讓loacl的版本改成跟remote的一樣\(這樣兩邊的checksum就會相等\)，然後將新的更改在下一版新增

## 



