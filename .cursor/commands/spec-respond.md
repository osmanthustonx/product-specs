你是 PM 的 spec 助手。PM 要回應工程師提出的 spec challenge。

## Step 1: 列出待回應的 challenges

用 gh CLI 查詢：

```bash
gh issue list --label "type:spec-challenge" --state open
```

如果沒有 open 的 challenge，告訴 PM「目前沒有待回應的 spec challenge」。

## Step 2: 顯示 challenge 詳情

讓 PM 選一個 issue，然後讀取完整內容：

```bash
gh issue view <issue-number>
```

摘要呈現給 PM：
- 工程師提出的問題
- 影響的功能（featureId）
- 建議的替代方案

## Step 3: PM 決定怎麼回應

問 PM 三選一：

1. **接受** — 同意工程師的建議，需要改 spec
2. **拒絕** — 不同意，需要說明理由
3. **討論** — 需要更多討論

### 如果接受

在 issue 留言說明接受：

```bash
gh issue comment <issue-number> --body "接受此 challenge。將開 spec change PR 來更新 spec。"
```

然後自動執行 `/spec:change <功能名稱>` 的流程（引導 PM 修改 spec）。

### 如果拒絕

問 PM 拒絕的理由，然後：

```bash
gh issue comment <issue-number> --body "## 回應

維持原設計。理由如下：

<PM 的理由>

如果仍有疑慮，歡迎繼續討論。"

gh issue close <issue-number>
```

### 如果討論

問 PM 要回什麼，然後：

```bash
gh issue comment <issue-number> --body "<PM 的回覆>"
```

## Step 4: 確認完成

告訴 PM 已回應，提醒 workflow 要求 2 個工作天內回應。
