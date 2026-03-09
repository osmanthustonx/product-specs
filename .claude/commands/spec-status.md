你是 PM 的 spec 助手。PM 想查看所有功能的現況。

## Step 1: 讀取 schema 定義

讀取 `openspec/schemas/pm-spec/schema.yaml`，取得 artifact 清單和 `requires` 依賴關係。

## Step 2: 掃描所有 spec

讀取 `openspec/specs/` 目錄下所有 `spec.md` 的 frontmatter，提取：
- featureId
- title
- status
- owner
- lastUpdated

## Step 3: 查詢對應的 FE/BE issues

用 gh CLI 查詢 FE 和 BE repos 的 spec-task issues：

```bash
gh issue list --repo <owner>/frontend-app --label "spec-task" --state all --json number,title,state,labels
gh issue list --repo <owner>/backend-app --label "spec-task" --state all --json number,title,state,labels
```

注意：`<owner>` 從 git remote 的 URL 解析取得。

## Step 4: 分析 active changes 的 artifact 狀態

列出 `openspec/changes/active/` 下正在進行的 changes。

對每個 active change，根據 schema 的 artifact 定義和 `requires` 依賴關係，判斷每個 artifact 的狀態：
- **done** — 檔案存在且內容非空（非純 template）
- **ready** — 所有 `requires` 的 artifact 都是 done，但本身尚未完成
- **blocked** — 有 `requires` 的 artifact 還沒 done

## Step 5: 輸出 Dashboard

用表格呈現：

```
## 功能狀態 Dashboard

| Feature | Status | Owner | Last Updated | FE Issue | BE Issue |
|---------|--------|-------|-------------|----------|----------|
| todo    | in-progress | PM | 2026-03-06 | #1 (open) | #1 (open) |

## Active Changes（進行中的變更）

### change-name
| Artifact | Status | Depends On |
|----------|--------|------------|
| proposal | done   | —          |
| specs    | done   | proposal   |

## Open Spec Challenges
- #3: PATCH toggle 設計不合理（waiting for PM response）
```

## Step 6: 提供快捷操作

問 PM 是否要對某個功能做操作：
- 開新功能 → 引導用 `/spec:new`
- 改 spec → 引導用 `/spec:change <功能名>`
- 回應 challenge → 引導用 `/spec:respond`
