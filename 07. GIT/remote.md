# Git 遠端操作

## 遠端倉庫概念

遠端倉庫是存放在伺服器上的 Git 倉庫，用於：
- 團隊協作開發
- 備份程式碼
- 發布和分享專案
- 同步多台電腦的工作

## 遠端倉庫管理

### 查看遠端倉庫

```bash
# 列出所有遠端倉庫
git remote

# 顯示詳細資訊（包含 URL）
git remote -v

# 顯示特定遠端的詳細資訊
git remote show origin
```

### 新增遠端倉庫

```bash
# 新增遠端倉庫（預設名稱 origin）
git remote add origin https://github.com/user/repo.git

# 新增遠端倉庫（自訂名稱）
git remote add upstream https://github.com/original/repo.git

# 新增多個遠端倉庫
git remote add github https://github.com/user/repo.git
git remote add gitlab https://gitlab.com/user/repo.git
```

### 修改遠端倉庫 URL

```bash
# 修改遠端倉庫 URL
git remote set-url origin https://github.com/user/new-repo.git

# 查看修改後的 URL
git remote -v
```

### 移除遠端倉庫

```bash
# 移除遠端倉庫
git remote remove origin
git remote rm origin
```

### 重新命名遠端倉庫

```bash
# 重新命名遠端倉庫
git remote rename old-name new-name
```

## 克隆倉庫

### 基本克隆

```bash
# 克隆倉庫到目前目錄
git clone https://github.com/user/repo.git

# 克隆到指定目錄
git clone https://github.com/user/repo.git my-project

# 使用 SSH 協定克隆
git clone git@github.com:user/repo.git

# 使用簡短 URL（GitHub）
git clone github.com/user/repo.git
```

### 克隆選項

```bash
# 克隆特定分支
git clone -b branch-name https://github.com/user/repo.git

# 淺克隆（只克隆最新版本，節省空間）
git clone --depth 1 https://github.com/user/repo.git

# 克隆指定深度的歷史
git clone --depth 10 https://github.com/user/repo.git

# 只克隆特定目錄（需要 Git 2.25+）
git clone --filter=blob:none --sparse https://github.com/user/repo.git
cd repo
git sparse-checkout init --cone
git sparse-checkout set directory-name
```

## 推送變更

### 基本推送

```bash
# 推送到預設遠端（origin）的預設分支
git push

# 推送到指定遠端和分支
git push origin main

# 推送到指定遠端的所有分支
git push origin --all

# 推送所有遠端
git push --all
```

### 推送選項

```bash
# 強制推送（危險，會覆蓋遠端歷史）
git push --force
git push -f

# 強制推送（更安全，會檢查）
git push --force-with-lease

# 推送標籤
git push --tags

# 推送特定標籤
git push origin tag-name

# 推送並設定上游分支
git push -u origin branch-name
git push --set-upstream origin branch-name
```

### 首次推送

```bash
# 建立新倉庫後首次推送
git remote add origin https://github.com/user/repo.git
git branch -M main
git push -u origin main
```

## 拉取更新

### git pull

```bash
# 拉取並合併遠端更新
git pull

# 拉取指定遠端和分支
git pull origin main

# 拉取並使用 rebase（保持線性歷史）
git pull --rebase
git pull -r

# 只拉取不自動合併
git pull --no-commit
```

**git pull 等同於：**
```bash
git fetch
git merge
```

### git fetch

```bash
# 從遠端獲取更新（不自動合併）
git fetch

# 從指定遠端獲取
git fetch origin

# 獲取所有遠端
git fetch --all

# 獲取特定分支
git fetch origin branch-name

# 獲取並清理已刪除的遠端分支
git fetch --prune
git fetch -p
```

### 合併遠端變更

```bash
# 手動合併遠端分支
git fetch origin
git merge origin/main

# 使用 rebase 合併
git fetch origin
git rebase origin/main
```

## 分支追蹤

### 設定上游分支

```bash
# 推送時設定上游分支
git push -u origin branch-name

# 手動設定上游分支
git branch --set-upstream-to=origin/branch-name branch-name

# 查看追蹤關係
git branch -vv
```

### 更新追蹤分支

```bash
# 更新所有追蹤分支
git fetch --all --prune

# 更新特定遠端的追蹤分支
git fetch origin --prune
```

## 遠端分支管理

### 查看遠端分支

```bash
# 列出所有遠端分支
git branch -r

# 列出所有分支（本地和遠端）
git branch -a

# 查看遠端分支詳細資訊
git remote show origin
```

### 建立本地分支追蹤遠端分支

```bash
# 從遠端分支建立本地分支並追蹤
git checkout -b local-branch origin/remote-branch

# 使用簡短語法（Git 2.23+）
git checkout remote-branch

# 建立並追蹤
git checkout --track origin/remote-branch
```

### 刪除遠端分支

```bash
# 刪除遠端分支
git push origin --delete branch-name
git push origin :branch-name
```

## 常見工作流程

### 標準協作流程

```bash
# 1. 從遠端拉取最新變更
git pull origin main

# 2. 建立功能分支
git checkout -b feature-branch

# 3. 進行開發和提交
git add .
git commit -m "實作新功能"

# 4. 推送到遠端
git push -u origin feature-branch

# 5. 在 GitHub/GitLab 建立 Pull Request

# 6. 合併後更新本地主分支
git checkout main
git pull origin main
```

### Fork 工作流程

```bash
# 1. Fork 原始倉庫到自己的帳號

# 2. 克隆自己的 Fork
git clone https://github.com/your-username/repo.git

# 3. 新增原始倉庫為 upstream
git remote add upstream https://github.com/original/repo.git

# 4. 從 upstream 獲取更新
git fetch upstream

# 5. 合併 upstream 的變更
git checkout main
git merge upstream/main

# 6. 推送到自己的 Fork
git push origin main
```

### 同步多個遠端

```bash
# 新增多個遠端
git remote add github https://github.com/user/repo.git
git remote add gitlab https://gitlab.com/user/repo.git

# 推送到所有遠端
git push github main
git push gitlab main

# 或使用腳本自動推送
```

## 認證設定

### HTTPS 認證

```bash
# 使用個人存取令牌（Personal Access Token）
# 在 GitHub/GitLab 設定中建立令牌
# 推送時輸入使用者名稱和令牌作為密碼

# 快取認證資訊（15 分鐘）
git config --global credential.helper cache

# 永久儲存認證資訊（不建議）
git config --global credential.helper store
```

### SSH 認證

```bash
# 1. 產生 SSH 金鑰
ssh-keygen -t ed25519 -C "your.email@example.com"

# 2. 將公鑰加入 GitHub/GitLab
cat ~/.ssh/id_ed25519.pub
# 複製內容到 GitHub/GitLab 的 SSH Keys 設定

# 3. 測試連線
ssh -T git@github.com

# 4. 使用 SSH URL 克隆
git clone git@github.com:user/repo.git
```

## 實用範例

### 更新 Fork 的倉庫

```bash
# 確保有 upstream 遠端
git remote add upstream https://github.com/original/repo.git

# 獲取原始倉庫的更新
git fetch upstream

# 切換到主分支
git checkout main

# 合併原始倉庫的變更
git merge upstream/main

# 推送到自己的 Fork
git push origin main
```

### 推送新分支

```bash
# 建立並切換到新分支
git checkout -b new-feature

# 進行開發和提交
git add .
git commit -m "新功能"

# 首次推送並設定追蹤
git push -u origin new-feature

# 後續推送
git push
```

### 解決推送衝突

```bash
# 如果遠端有新的提交，先拉取
git pull origin main

# 如果有衝突，解決後提交
git add .
git commit -m "解決衝突"

# 再次推送
git push origin main
```

## 常見問題

### 推送被拒絕

```bash
# 錯誤：遠端包含您本地沒有的提交
# 解決：先拉取再推送
git pull origin main
git push origin main
```

### 遠端分支已刪除

```bash
# 清理本地追蹤已刪除的遠端分支
git fetch --prune
git remote prune origin
```

### 變更遠端 URL

```bash
# 查看目前 URL
git remote -v

# 修改 URL
git remote set-url origin https://github.com/user/new-repo.git
```

## 常用指令速查

```bash
# 遠端管理
git remote -v
git remote add origin <url>
git remote remove origin
git remote set-url origin <url>

# 克隆
git clone <url>
git clone -b <branch> <url>

# 推送
git push
git push origin main
git push -u origin branch-name

# 拉取
git pull
git pull origin main
git pull --rebase

# 獲取
git fetch
git fetch origin
git fetch --all --prune
```

