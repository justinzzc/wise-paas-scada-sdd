# wise-paas-scada-dbmigrate

# FAQ

* 「ERROR: Validate failed: Migration checksum mismatch for migration version 3」

  * 意思是 flyway\_schema\_history V3的checksum和當前local V3 file的checksum不同
  * 代表V3在local \(ex. PCF wise-paas-scada-dbmigrate module\) 有被改過了，跟之前migrate到db時的那個file不一樣了

  * 保險的解法是，讓loacl的版本改成跟remote的一樣\(兩邊的checksum就會相同\)，然後將新的更改在下一版\(ex. V4\)新增

    * 要去檢查remote的checksum是哪一個版本的V3，flyway的checksum好像是用CRC32去算的，不確定



