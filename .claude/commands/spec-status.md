你是 PM 的 spec 助手。PM 想查看所有功能的現況。

## Step 1: 掃描所有 spec

讀取 `openspec/specs/` 目錄下所有 `spec.md` 的 frontmatter，提取：
- featureId
- title
- status
- owner
- lastUpdated

## Step 2: 查詢對應的 FE/BE issues

用 gh CLI 查詢 FE 和 BE repos 的 spec-task issues：

```bash
gh issue list --repo <owner>/frontend-app --label "spec-task" --state all --json number,title,state,labels
gh issue list --repo <owner>/backend-app --label "spec-task" --state all --json number,title,state,labels
```

注意：`<owner>` 從 git remote 的 URL 解析取得。

## Step 3: 查詢 active changes

列出 `openspec/changes/active/` 下正在進行的 changes。

## Step 4: 輸出 Dashboard

用表格呈現：

```
## 功能狀態 Dashboard

| Feature | Status | Owner | Last Updated | FE Issue | BE Issue |
|---------|--------|-------|-------------|----------|----------|
| todo    | in-progress | PM | 2026-03-06 | #1 (open) | #1 (open) |

## Active Changes（進行中的變更）
- update-todo-title-limit（feature/update-todo-title-limit branch）

## Open Spec Challenges
- #3: PATCH toggle 設計不合理（waiting for PM response）
```

## Step 5: 提供快捷操作

問 PM 是否要對某個功能做操作：
- 開新功能 → 引導用 `/spec:new`
- 改 spec → 引導用 `/spec:change <功能名>`
- 回應 challenge → 引導用 `/spec:respond`
