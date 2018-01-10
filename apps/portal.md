# Portal
-----------

## Table of Contents
#### 1. Overview
#### 2. Front-end architecture description
    2.1   登入頁面  
      2.1.1   Login.vue

    2.2   Device Management 頁面       
      2.2.1   ProjectList.vue
      2.2.2   ScadaList.vue
      2.2.3   DeviceList.vue
      2.2.4   TagList.vue

    2.3   Alarm Summary 頁面
      2.3.1   AlarmSummary.vue

    2.4   Account
      2.4.1   Account Setting 頁面
        2.4.1.1   AccountSetting.vue
      2.4.2   Role Setting 頁面
        2.4.2.1   RoleSetting.vue

    2.5   WISE-PaaS Dashboard 頁面
      2.5.1   MenuBar.vue

    2.6   User Guide 頁面
      2.6.1   UserGuide.vue

    2.7   API Document 頁面 
     2.7.1   MenuBar.vue

    2.8   使用者帳號管理
      2.8.1   Profile 頁面
        2.8.1.1   UserInfo.vue
      2.8.2   logout   

--

### 1. Overview
### 2. Front-end architecture description  
##### 2.1 登入頁面
- 2.1.1 Login.vue    
  - 目的:使用者輸入帳號密碼以登入WebAccess/SCADA  
  - 架構說明:  
  Vue-components  
  　　src/components/Login.vue  
  Vue-router  
  　　src/router/index.js
  - UI:
   ![](/assets/login.PNG)  
  - 邏輯說明:  
        1. Login Page  
        　1.1 進入Url: https://portal-scada-1-1-12-develop.wise-paas.com/#/。  
        　1.2 執行mounted function，判斷$cookie.get("EIName")狀態，使用者是否登錄過。  
        　　1.2.1 若為true，執行checkToken function。  
        　　　1.2.1.1 呼叫 $http.get("/Users/me") 進入 Device Management 頁面。  
        　　1.2.2 若為false，則等待使用者按下①按鈕。  
        2.將帳號密碼輸入完後按下①按鈕，執行login function，判斷Conf.ValidSSO && Conf.mode === 'development'是否為false。  
        　2.1 若為false，定義user這個objecct 並進入 Device Management 頁面。  
        　2.2 若為true，判斷帳號密碼是否已輸入，並將執行 checkToken function。  
        　　2.2.1 呼叫 $http.get("/Users/me") 進入 Device Management 頁面。  