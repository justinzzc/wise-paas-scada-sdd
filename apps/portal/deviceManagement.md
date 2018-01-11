

- 1.Login.vue    
  - 目的:使用者輸入帳號密碼以登入WebAccess/SCADA  
  - 架構說明:  
  Vue-components  
  　　src/components/Login.vue  
  Vue-router  
  　　src/router/index.js
  - UI:
   ![](/assets/login.PNG)  
  - 邏輯說明:  
        1. Login Page (Url: /Login)。  
        　1.1 透過src/router/index.js呼叫src/components/Login.vue，進入Login的頁面。  
        　1.2 執行mounted function，檢查$cookie.get("EIName")狀態，使用者是否登錄過。  
        　　1.2.1 若為true，執行checkToken function。  
        　　　1.2.1.1 呼叫$http.get("/Users/me") 進入Device Management頁面。  
        　　1.2.2 若為false，則等待使用者按下1號按鈕。  
        2.將帳號密碼輸入完後按下①按鈕，執行login function，檢查Conf.ValidSSO && Conf.mode === 'development'狀態。  
        　2.1 若為false，定義user這個objecct 並進入Device Management頁面。  
        　2.2 若為true，檢查帳號密碼是否已輸入。  
        　　2.2.1 呼叫axios.post(Conf.SSOUrl + "/auth", obj)檢查response.data.status === 'locked'狀態，使用者是否第一次註冊。  
        　　　2.2.1.1 若為true，addVisible=true   
        　　　2.2.1.2 若為false，檢查rememberMe狀態，2號按鈕是否被打勾，並且執行checkToken function。  
        　　　　2.2.1.2.1 若為true，記下帳號密碼，呼叫$http.get("/Users/me") 進入 Device Management 頁面。  
        　　　　2.2.1.2.2 若為false，直接呼叫$http.get("/Users/me") 進入 Device Management 頁面。    
        　　　　  


# Device Management頁面

---

- 1.ProjectList.vue  
  - 目的:使用者在Device Management 頁面可以編輯project的Id及敘述，並可新增新的project項目，另外也可以執行刪除動作。 
  - 架構說明:  
  Vue-components  
  　　src/components/ProjectManger/ProjectList.vue  
  　　src/components/MenuBar.vue  
  　　src/components/Common/Modal.vue  
  　　src/components/Common/ConfigModal.vue   
  Vue-router  
  　　src/router/index.js   
  - UI:  
  ![](/assets/projectlist.PNG)  
  - 邏輯說明:  
        1. 在Device Management中編輯Project的頁面(Url: /Login/CloudManager/DeviceManagement)  
        　1.1 透過src/components/MenuBar.vue按下1號按鈕執行select function，透過src/router/index.js呼叫src/components/ProjectList.vue進入可編輯Project的頁面。  
        　1.2 同時在src/components/MenuBar.vue檢查$store.state.user.scope使用者的權限將變數存入scopeEdit。  
        2. 一進入Project的頁面即執行$checkLogin和getProjectList function。  
        　2.1 執行$checkLogin判斷scopeEdit狀態。  
          　2.1.1 若為true即可顯示4、5號按鈕。  
          　2.1.2 若為false即不顯示4、5號按鈕。  
          　2.2 執行getProjectList列出所有project相關資訊。  
        　　2.2.1 呼叫API: GET /Projects。  
        　　　2.2.1.1 若呼叫成功列出列出project資料，並將要顯示資訊存到projects陣列中。  
        　　　2.2.1.2 若呼叫失敗則判斷err.response.status === 404狀態。  
        　　　　2.2.1.2.1 若為true，令projects = []。  
        　　　　2.2.1.2.2 若為false，執行$checkToken。   
        3. 按下2號按鈕執行getProjectList並依使用者選擇或輸入做排序。  
        4. 按下3號按鈕執行handleDetailClick。  
        　4.1 呼叫API: /Projects/:projectId。  
        　　4.1.1 若呼叫成功，令detailVisible = true，並將要顯示的資訊存到tempProject物件中。  
        　　　4.1.1.1 呼叫src/components/Common/ConfigModal.vue，並傳入Config、typeEdit、obj、type等參數，分別為:Config="true" :typeEdit="scopeEdit" :obj="tempProject" :type="'project'"。  
        　　　　4.1.1.1.1 若呼叫成功則跳出detail視窗  
        　　　　　4.1.1.1.1.1 若typeEdit=true即顯示7號按鈕，按下7號按鈕執行handleEditClick。  
        　　　　　　4.1.1.1.1.1.1 分別將顯示的參數存入showObj物件，編輯參數存入editObj物件，並判斷obj.alarm!==undefined。  
        　　　　　　　4.1.1.1.1.1.1.1 若為true將obj.alarm參數存入showAlarm和editAlarm。  
        　　　　　4.1.1.1.1.2 若有按下7號按鈕則轉成8號按鈕，編輯完後按下8號按鈕執行handleSaveClick。  
        　　　　　　4.1.1.1.1.2.1 呼叫API: PUT /Projects/:projectId。  
        　　　　　　　4.1.1.1.1.2.1.1 若呼叫成功即完成編輯。  
        　　　　　　　4.1.1.1.1.2.1.2 若呼叫失敗回傳errMsg。  
        　　　　　4.1.1.1.1.3 按下9號按鈕，令editObj = {}並關閉視窗。  
        　　　　　4.1.1.1.1.4 按下10號按鈕，令tempProject = {}並關閉視窗。  
        　　　4.1.2 若呼叫失敗執行$checkToken。  
        5. 按下4號按鈕執行handleDeleteClick。  
        　5.1 將選取的project資訊存入tempProject物件中，並令deleteVisiable = true。  
        　　5.1.1 呼叫src/components/Common/Modal.vue，顯示delete視窗。  
        　　　5.1.1.1 按下11號按鈕執行handleDeleteSubmit。  
        　　　　5.1.1.1.1 執行deleteProject並呼叫API: DELETE /Projects/:projectId，  
        　　　5.1.1.2 按下12號按鈕執行handleCancelClick。     
          
        



