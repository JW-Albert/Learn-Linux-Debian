# Git 分支管理

## 分支概念

分支是 Git 的核心功能之一，允許：
- 同時進行多個功能開發
- 實驗性變更不影響主線
- 輕鬆切換不同版本
- 協同開發時避免衝突

### 分支的優勢

1. **隔離開發**：新功能在獨立分支開發，不影響主線
2. **快速切換**：輕量級分支，切換幾乎瞬間完成
3. **並行開發**：多人可以同時在不同分支工作
4. **安全實驗**：可以隨意實驗，失敗不影響主線

## 分支操作

### 查看分支

```bash
# 列出所有本地分支
git branch

# 列出所有分支（包含遠端）
git branch -a

# 列出遠端分支
git branch -r

# 顯示分支追蹤關係
git branch -vv

# 顯示已合併的分支
git branch --merged

# 顯示未合併的分支
git branch --no-merged
```

### 建立分支

```bash
# 建立新分支（不切換）
git branch branch-name

# 建立並切換到新分支
git checkout -b branch-name

# 從特定提交建立分支
git branch branch-name commit-hash

# 從遠端分支建立本地分支
git branch branch-name origin/remote-branch

# 建立並追蹤遠端分支
git checkout -b branch-name origin/remote-branch
```

### 切換分支

```bash
# 切換到指定分支
git checkout branch-name

# 切換到新分支（建立並切換）
git checkout -b new-branch

# 切換到上一個分支
git checkout -

# 使用 switch 指令（Git 2.23+，更直觀）
git switch branch-name
git switch -c new-branch  # 建立並切換
```

### 重新命名分支

```bash
# 重新命名目前分支
git branch -m new-name

# 重新命名指定分支
git branch -m old-name new-name

# 強制重新命名（即使目標名稱已存在）
git branch -M new-name
```

### 刪除分支

```bash
# 刪除已合併的分支
git branch -d branch-name

# 強制刪除分支（即使未合併）
git branch -D branch-name

# 刪除遠端分支
git push origin --delete branch-name
git push origin :branch-name
```

## 合併分支

### 基本合併

```bash
# 合併指定分支到目前分支
git merge branch-name

# 合併時建立合併提交
git merge --no-ff branch-name

# 合併時只快轉（如果可能）
git merge --ff-only branch-name

# 合併時不自動提交
git merge --no-commit branch-name
```

### 合併策略

```bash
# 使用特定合併策略
git merge -s ours branch-name    # 使用我們的版本
git merge -s theirs branch-name  # 使用他們的版本
git merge -s recursive branch-name  # 遞迴合併（預設）
```

### 解決合併衝突

當兩個分支修改了同一檔案的同一部分時會產生衝突。

**衝突標記：**
```
<<<<<<< HEAD
目前的變更
=======
要合併的變更
>>>>>>> branch-name
```

**解決步驟：**
```bash
# 1. 查看衝突檔案
git status

# 2. 編輯檔案，解決衝突
# 選擇保留的變更，刪除衝突標記

# 3. 標記衝突已解決
git add resolved-file.txt

# 4. 完成合併
git commit
```

**使用工具解決衝突：**
```bash
# 使用 mergetool
git mergetool

# 設定 mergetool
git config --global merge.tool vimdiff
git config --global merge.tool meld
```

### 中止合併

```bash
# 如果合併出錯，可以中止
git merge --abort
```

## Rebase

Rebase 是另一種整合分支變更的方式，會重新應用提交到新的基礎上。

### 基本 Rebase

```bash
# 將目前分支 rebase 到指定分支
git rebase branch-name

# 互動式 rebase
git rebase -i HEAD~3  # 重新整理最近 3 個提交

# 繼續 rebase（解決衝突後）
git rebase --continue

# 中止 rebase
git rebase --abort

# 跳過目前提交
git rebase --skip
```

### 互動式 Rebase

```bash
# 重新整理最近 3 個提交
git rebase -i HEAD~3
```

**互動式選項：**
- `pick`：保留提交
- `reword`：修改提交訊息
- `edit`：編輯提交
- `squash`：合併到上一個提交
- `fixup`：合併到上一個提交（不保留訊息）
- `drop`：刪除提交

### Rebase vs Merge

**Merge：**
- 保留完整歷史
- 建立合併提交
- 歷史圖形較複雜

**Rebase：**
- 線性歷史
- 不建立合併提交
- 歷史較清晰

**建議：**
- 個人分支：使用 rebase 保持歷史清晰
- 共享分支：使用 merge 保留完整歷史

## 分支策略

### Git Flow

常見的分支策略：

```
main/master    # 主分支，穩定版本
develop        # 開發分支
feature/*      # 功能分支
release/*      # 發布分支
hotfix/*       # 緊急修復分支
```

### GitHub Flow

簡化的分支策略：

```
main           # 主分支
feature/*      # 功能分支
```

### 實作範例

```bash
# 建立功能分支
git checkout -b feature/new-feature

# 開發和提交
git add .
git commit -m "實作新功能"

# 推送到遠端
git push -u origin feature/new-feature

# 在 GitHub 建立 Pull Request

# 合併後刪除本地分支
git checkout main
git pull origin main
git branch -d feature/new-feature
```

## 標籤管理

### 建立標籤

```bash
# 建立輕量級標籤
git tag v1.0.0

# 建立附註標籤（推薦）
git tag -a v1.0.0 -m "版本 1.0.0"

# 在特定提交建立標籤
git tag -a v1.0.0 commit-hash -m "版本 1.0.0"

# 建立帶簽名的標籤
git tag -s v1.0.0 -m "版本 1.0.0"
```

### 查看標籤

```bash
# 列出所有標籤
git tag

# 搜尋標籤
git tag -l "v1.*"

# 查看標籤詳細資訊
git show v1.0.0
```

### 推送標籤

```bash
# 推送所有標籤
git push --tags

# 推送特定標籤
git push origin v1.0.0

# 推送並刪除遠端標籤
git push origin :refs/tags/v1.0.0
```

### 刪除標籤

```bash
# 刪除本地標籤
git tag -d v1.0.0

# 刪除遠端標籤
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0
```

## 實用範例

### 功能開發流程

```bash
# 1. 從主分支建立功能分支
git checkout main
git pull origin main
git checkout -b feature/user-login

# 2. 開發功能
git add .
git commit -m "實作使用者登入功能"

# 3. 推送到遠端
git push -u origin feature/user-login

# 4. 建立 Pull Request

# 5. 合併後清理
git checkout main
git pull origin main
git branch -d feature/user-login
```

### 修復錯誤流程

```bash
# 1. 從主分支建立修復分支
git checkout main
git checkout -b hotfix/critical-bug

# 2. 修復錯誤
git add .
git commit -m "修復關鍵錯誤"

# 3. 合併回主分支
git checkout main
git merge hotfix/critical-bug

# 4. 推送到遠端
git push origin main
```

### 整理提交歷史

```bash
# 使用互動式 rebase 整理最近 5 個提交
git rebase -i HEAD~5

# 在編輯器中：
# - 將多個提交合併為一個（使用 squash）
# - 修改提交訊息（使用 reword）
# - 重新排序提交

# 完成後推送到遠端（需要強制推送）
git push --force-with-lease
```

## 常用指令速查

```bash
# 分支操作
git branch
git branch branch-name
git checkout branch-name
git checkout -b branch-name
git branch -d branch-name

# 合併
git merge branch-name
git merge --no-ff branch-name

# Rebase
git rebase branch-name
git rebase -i HEAD~3

# 標籤
git tag
git tag -a v1.0.0 -m "訊息"
git push --tags
```