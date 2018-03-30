# Cloud Foundry

---

* 在PCF上的測試流程
  * clone project
  * npm install
  * change package name of package.json, because this is for testing.
  * npm run generate
  * npm run build
  * scripts/gen\_man.bat
  * 把共用的route拿掉 \(manifest-develop\)
  * 看有沒有測試中的dbmanager或utility要蓋掉的，記得要蓋掉一起上傳
  * setup.bat



