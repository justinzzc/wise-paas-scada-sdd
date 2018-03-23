# Front-End Release SOP

---

1. 整理CHANGELOG
2. 整理package.json
   1. 要確認dbmanager和wise-paas-scada版號正確
3. npm run generate \(gen API DOC\)
   1. 在會更新Front-End的index.yml
4. 用develop branch的code在PCF做測試
   1. 重新npm install，因為會連node\_module一起上PCF，或學Stacy再複製一份
   2. npm build \(for Vue\)
   3. gen\_mm.bat
   4. setup.bat
   5. push to PCF
   6. test on PCF
5. 把新產生的index.yml複製到Document，蓋過去，並push上去
   1. [http://advgitlab.eastasia.cloudapp.azure.com/WISE-PaaS-Documentation](http://advgitlab.eastasia.cloudapp.azure.com/WISE-PaaS-Documentation)
6. commit to develop branch，要確保以下4個動作在同一commit
   1. CHANGELOG
   2. package.json
   3. swagger doc \(index.yml\)
   4. 加tag
7. push to develop branch
8. 在master branch把develop branch merge回來
9. 發信給team members, 格式參考stacy，不要直接複製vs code。
10. 和益連同dataworker準備好給QA後，再去更新redmine相關issue
11. release完後記得把其他unrelease的branch做rebase

### 建議

再做上述動作前，可以clone一個新的project，避免非release的程式包含在裡面

