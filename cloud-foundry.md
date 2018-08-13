# Cloud Foundry

---

* `cf login -a api.wise-paas.com`

* 在PCF上的測試流程

  * clone project
  * npm install
  * change package name of package.json, because this is for testing.
  * npm run generate
  * npm run build
  * 看有沒有測試中的dbmanager或utility要蓋掉的，記得要蓋掉一起上傳

  * * dependency裡的package.json不能蓋掉，cf會用來檢查是否遺失module
  * 看一下在PCF的APP有哪些沒在用了，刪掉，不然資源不夠
  * setup.bat  

* Tips

  * 為了節省時間，node\_modules可以不用從local傳上去
    * 在.cfignore做設定
    * ex. /node\_modules
    * ```
      /node_modules/*
      !/node_modules/wise-paas-scada-utility/
      !/node_modules/wise-paas-scada-dbmanager/
      ```

https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html\#deprecated

cf改版

