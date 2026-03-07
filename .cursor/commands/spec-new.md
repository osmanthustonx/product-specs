你是 PM 的 spec 助手。PM 要建立一個全新功能的 spec。

功能名稱: $ARGUMENTS

請嚴格按照以下步驟執行：

## Step 1: 確認功能名稱

如果沒有提供功能名稱，請詢問 PM。功能名稱用 kebab-case（如 `user-login`、`trade-modal`）。

## Step 2: 開 branch

```bash
git checkout main && git pull origin main
git checkout -b feature/<功能名稱>
```

## Step 3: 讀取 schema 確認流程

讀取 `openspec/schemas/pm-spec/schema.yaml`，確認 artifact 的依賴順序和每個 artifact 的 `instruction`。
按照 `requires` 欄位決定生成順序（無依賴的先做）。

## Step 4: 建立目錄結構

建立以下目錄和檔案：
- `openspec/specs/<功能名稱>/spec.md` — living spec（正式版）
- `openspec/changes/active/<功能名稱>/proposal.md`
- `openspec/changes/active/<功能名稱>/specs/<功能名稱>/spec.md` — delta spec
- `openspec/changes/active/<功能名稱>/design.md`
- `openspec/changes/active/<功能名稱>/tasks.md`

## Step 5: 依 schema 順序引導 PM 填寫

根據 schema.yaml 裡的 `requires` 依賴順序，逐一引導 PM 完成每份 artifact。
每個 artifact 開始前，讀取 schema 裡對應的 `instruction` 作為生成指引。

### 5.1 proposal.md（requires: 無）
問 PM：
- 「為什麼要做這個功能？」
- 「解決什麼問題？」
- 「做完預期達成什麼效果？」
- 「初步範圍大概是什麼？」

根據回答生成 proposal.md。

### 5.2 spec.md（requires: proposal）
問 PM 以下 6 個 section 的內容，逐一確認：

1. **Summary** — 一句話描述
2. **Users & Use Cases** — 誰在什麼情況下用
3. **Functional Requirements** — 要做到什麼（用使用者故事或自然語言）
4. **Behavioral Spec** — 寫 BDD 場景（Given/When/Then），至少包含：
   - 2-3 個 happy path
   - 2-3 個 error case
5. **Scope** — In Scope / Out of Scope
6. **Dependencies & Open Questions**

living spec 的 frontmatter 必須包含：
```yaml
---
featureId: <功能名稱>
title: <中文標題>
status: draft
owner: PM
lastUpdated: <今天日期>
---
```

delta spec 使用標準化格式，用 `## ADDED Requirements` 標記所有新增內容。
每個 requirement 下用 `#### Scenario:` 搭配 Given/When/Then 格式。

### 5.3 design.md（requires: specs）
基於 spec 產出功能層設計：
- API Design（REST endpoints、request/response 格式）
- Data Model
- State Machine（如適用）
- Data Flow
- Module Boundaries

注意：只描述「對外行為和介面」，不寫內部實作細節。

### 5.4 tasks.md（requires: specs, design）
基於 design 拆解任務，必須使用以下固定格式（標題不能改）：

```markdown
## FE Tasks
- [ ] 任務 1
- [ ] 任務 2

## BE Tasks
- [ ] 任務 1
- [ ] 任務 2

## Integration Tasks
- [ ] 任務 1
```

## Step 6: Commit + Push + 開 PR

```bash
git add -A
git commit -m "feat: <功能名稱> spec"
git push -u origin feature/<功能名稱>
```

然後用 `gh pr create` 開 PR，使用以下格式：

```
標題: feat: <功能名稱> 功能 spec
Body:
## Type
new-feature

## Required Reviewers
PM: @<owner>

## Summary
<一句話描述>

## OpenSpec Change
<功能名稱>

## Checklist
- [x] proposal.md completed
- [x] specs/ delta spec completed (6-section format)
- [x] design.md completed (functional-level design)
- [x] tasks.md completed (FE/BE/Integration 3-section format correct)
- [x] Dependencies section annotated (if applicable)
```

## Step 7: 告知 PM 後續

告訴 PM：
- PR 已建立，請找工程師 review
- 兩方都 approve 後 merge
- Merge 後會自動：同步 spec 到 FE/BE repos、開 issue、歸檔 change
