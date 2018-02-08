# FAQ

---

Q:  DeviceShadow透過connQueue觸發時，如果流程走到deviceShadow.turnOnline，除了會改變mongodb裡scada\_DeviceStatus的狀態外，它還會去對deviceCmdTopic: '/wisepaas/scada/{scadaId}/{deviceId}/cmd'這個Topic送資料。**我的問題是: 有那裡會訂閱上述這個Topic，以及發送這個Topic用途為何?**

* Ans: 這個topic是worker去publish 所以不用訂閱，因為規格是設備連上線時 worker必須要主動通知設備請他送資料，我們叫做dataon，所以那個deviceCmdTopic 就是設備端去subscribe，worker publish dataon message到這個topic



