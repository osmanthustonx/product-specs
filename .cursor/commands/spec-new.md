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

注意：PM 不寫 design.md 和 tasks.md，那是工程師的職責。

## Step 5: 依 schema 順序引導 PM 填寫

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
- [x] Dependencies section annotated (if applicable)
```

## Step 7: 告知 PM 後續

告訴 PM：
- PR 已建立，請找工程師 review
- 兩方都 approve 後 merge
- Merge 後會自動：同步 spec 到 FE/BE repos、在 FE/BE 開 issue
- 工程師收到 issue 後，會用 `/eng:start` 自行做 design 和 tasks 拆解
