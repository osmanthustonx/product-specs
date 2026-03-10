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

## Step 4: 掃描依賴圖，檢查連鎖影響

這一步非常重要。修改一個 spec 時，可能有其他 spec 依賴它。

1. 讀取 `openspec/specs/` 下**所有** spec.md 的 frontmatter
2. 找出 `depends` 欄位包含當前功能名稱的 specs（即：誰依賴了這個功能）
3. 如果找到依賴方，向 PM 報告：

```
⚠ 以下功能依賴 <功能名稱>，可能受此次修改影響：

| 功能 | 依賴原因 | 可能受影響的部分 |
|------|---------|----------------|
| todo-img | depends: [todo-crud] | data model 變更可能影響圖片附件邏輯 |
| todo-share | depends: [todo-crud] | API 變更可能影響分享功能 |
```

4. 針對每個受影響的依賴方，讀取其 spec 內容，判斷是否真的需要連帶更新
5. 問 PM：「以下依賴方的 spec 也需要一起更新嗎？」
   - 如果需要 → 在同一個 branch 和 PR 裡一起修改
   - 如果不需要 → 在 PR body 標注「已檢查，不影響」

## Step 5: 執行對應流程

### 如果是 small（澄清）

```bash
git checkout main && git pull origin main
git checkout -b feature/clarify-<描述>
```

直接修改 `openspec/specs/<功能名稱>/spec.md`。

如果有連鎖影響的依賴方也需要更新，一起修改它們的 spec.md。

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

## Cascading Updates
<列出連帶更新的依賴方 featureId，或「無」>

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
2. 寫 delta spec，使用標準化格式：
   - `## ADDED Requirements` — 新增的需求和場景
   - `## MODIFIED Requirements` — 修改的需求（標注 `Previously: <舊行為>`）
   - `## REMOVED Requirements` — 移除的需求（標注 `Reason: <原因>`）
3. 同步更新 `openspec/specs/<功能名稱>/spec.md`（living spec）
4. 如果有連鎖影響的依賴方：
   - 在同一個 change 目錄下建立依賴方的 delta spec：`specs/<依賴方featureId>/spec.md`
   - 同步更新依賴方的 living spec

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

## Cascading Updates
<列出連帶更新的依賴方 featureId，附簡要說明為什麼需要更新>

## Change Impact
medium

## OpenSpec Change
update-<描述>

## Behavior Diff
### Before
<改之前的行為>

### After
<改之後的行為>

### Cascading Changes
<每個依賴方的 Before/After，如有的話>
```

### 如果是 large（方向大改）

告訴 PM：
1. 需要先把舊 spec 標為 deprecated
2. 然後用 `/spec-new` 建立全新的 spec
3. 引導 PM 執行這兩個步驟

## Step 6: 告知後續

告訴 PM：
- PR 已建立（包含所有連鎖更新）
- Merge 後 FE/BE 的已開 issue 會自動收到通知（包括依賴方的 issue）
- Issue 會被加上 `spec-updated` 標籤
