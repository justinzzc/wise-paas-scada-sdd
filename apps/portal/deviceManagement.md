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
        　1.2 同時在src/components/MenuBar.vue檢查$store.state.user.scope使用者的權限。  
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
        　　4.1.2 若呼叫失敗執行$checkToken。     




