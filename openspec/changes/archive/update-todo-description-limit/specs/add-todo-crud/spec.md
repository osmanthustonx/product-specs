# Delta for add-todo-crud

## MODIFIED Requirements

### Requirement: 建立 Todo
description 欄位新增 500 字元上限。
(Previously: 無長度限制)

#### Scenario: description 超過 500 字元
  Given 使用者在建立表單
  When 輸入超過 500 字元的 description 並提交
  Then 系統回傳 400，錯誤訊息 "Description must be 500 characters or less"
