# Delta for Todo

## MODIFIED: Functional Requirements
### Requirement: 建立 Todo
Title 最大長度從 200 字元改為 100 字元。

## MODIFIED: Behavioral Spec

### Scenario: 標題超過 100 字元
  Given 用戶在建立表單
  When 輸入超過 100 字元的標題並提交
  Then 系統回傳 400，錯誤訊息 "Title must be 100 characters or less"
