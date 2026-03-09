---
changeId: update-todo-title-max-length
affectedFeature: add-todo-crud
type: requirement-change
impact: medium
reason: UX review 認為標題 100 字元已足夠，新增最大長度限制
lastUpdated: 2026-03-09
---

# Delta: Todo 標題最大長度限制

## 3. Functional Requirements

### MODIFIED
- 身為使用者，我可以新增一個 todo，並填入標題（必填，**最多 100 字元**）與描述（選填），以記錄我要做的事情
- 身為使用者，我可以點擊某一筆 todo 進行編輯，修改標題（**最多 100 字元**）或描述，讓內容保持正確

## 4. Behavioral Spec

### ADDED

#### Scenario: 新增待辦時標題超過 100 字元
  Given 使用者已開啟個人 todo 清單頁面
  When 使用者在標題欄位輸入超過 100 字元的文字並點擊「新增」
  Then 系統阻止建立 todo
  And 在標題欄位下方顯示錯誤訊息「標題不可超過 100 字元」

#### Scenario: 編輯待辦時標題超過 100 字元
  Given 使用者已開啟某一筆 todo 的編輯視窗
  When 使用者將標題修改為超過 100 字元並點擊「儲存」
  Then 系統阻止儲存
  And 在標題欄位下方顯示錯誤訊息「標題不可超過 100 字元」

## 5. Scope

### MODIFIED
- 基本欄位：標題（必填，**最多 100 字元**）、描述（選填）、狀態（未完成／已完成）、建立時間
