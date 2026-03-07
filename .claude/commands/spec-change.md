你是 PM 的 spec 助手。PM 要修改一個已經定好的 spec。

功能名稱: $ARGUMENTS

## Step 1: 確認功能和讀取現有 spec

如果沒提供功能名稱，列出 `openspec/specs/` 下所有功能讓 PM 選。

讀取 `openspec/specs/<功能名稱>/spec.md` 的完整內容，顯示摘要給 PM 看。

## Step 2: 詢問要改什麼

問 PM：「你要改什麼？請描述要修改的內容。」

## Step 3: 判斷改動大小

根據 PM 的描述自動判斷，並跟 PM 確認：

| 大小 | 標準 | 處理方式 |
|------|------|---------|
| **small** | 補文字、澄清描述、不影響行為 | 直接改 living spec |
| **medium** | 改邏輯細節、影響行為但不改方向 | 開新 change + delta spec |
| **large** | 整個方向變了、等於新功能 | deprecate 舊的 + 開新 spec |

## Step 4: 執行對應流程

### 如果是 small（澄清）

```bash
git checkout main && git pull origin main
git checkout -b feature/clarify-<描述>
```

直接修改 `openspec/specs/<功能名稱>/spec.md`，然後：

```bash
git add -A
git commit -m "docs: <描述>"
git push -u origin feature/clarify-<描述>
```

用 `gh pr create` 開 PR：
```
標題: clarify: <描述>
Body:
## Type
clarification

## Affected Features
<功能名稱>

## Change Impact
small

## Behavior Diff
### Before
<改之前>

### After
<改之後>
```

### 如果是 medium（需求變更）

```bash
git checkout main && git pull origin main
git checkout -b feature/update-<描述>
```

1. 建立新 change：`openspec/changes/active/update-<描述>/`
2. 寫 delta spec（只寫改了什麼，用 MODIFIED 標記）
3. 寫 tasks.md（新增的 FE/BE/Integration 任務）
4. 同步更新 `openspec/specs/<功能名稱>/spec.md`（living spec）

```bash
git add -A
git commit -m "feat: update <描述>"
git push -u origin feature/update-<描述>
```

用 `gh pr create` 開 PR：
```
標題: change: <描述>
Body:
## Type
requirement-change

## Affected Features
<功能名稱>

## Change Impact
medium

## OpenSpec Change
update-<描述>

## Behavior Diff
### Before
<改之前的行為>

### After
<改之後的行為>
```

### 如果是 large（方向大改）

告訴 PM：
1. 需要先把舊 spec 標為 deprecated
2. 然後用 `/spec:new` 建立全新的 spec
3. 引導 PM 執行這兩個步驟

## Step 5: 告知後續

告訴 PM：
- PR 已建立
- Merge 後 FE/BE 的已開 issue 會自動收到通知
- Issue 會被加上 `spec-updated` 標籤
