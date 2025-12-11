# Git 版本控制系統

## 簡介

Git 是一個分散式版本控制系統（Distributed Version Control System, DVCS），由 Linus Torvalds 於 2005 年開發，用於管理 Linux 核心的開發。Git 是目前最廣泛使用的版本控制系統，被全球數百萬開發者使用。

### 什麼是版本控制

版本控制系統（Version Control System, VCS）是用來追蹤檔案變更歷史的工具，可以：

- **記錄變更歷史**：追蹤檔案的所有修改記錄
- **回溯版本**：回到任何歷史版本
- **協同開發**：多人同時開發而不會互相干擾
- **分支管理**：建立不同的開發分支
- **合併變更**：將不同分支的變更合併
- **備份保護**：避免程式碼遺失

### Git 的特點

1. **分散式架構**：每個開發者都有完整的版本歷史
2. **高效能**：操作速度快，適合大型專案
3. **分支管理**：輕量級分支，切換快速
4. **資料完整性**：使用 SHA-1 雜湊確保資料完整性
5. **開源免費**：完全開源，可自由使用

### Git vs 其他版本控制系統

- **集中式 VCS（如 SVN）**：需要中央伺服器，離線無法工作
- **分散式 VCS（Git）**：每個副本都是完整倉庫，可離線工作
- **Git**：目前最流行，功能強大，社群支援豐富

## 基本概念

### 工作區、暫存區、倉庫

Git 有三個主要區域：

1. **工作區（Working Directory）**
   - 目前正在編輯的檔案
   - 可以看到和修改的檔案

2. **暫存區（Staging Area / Index）**
   - 準備提交的檔案
   - 使用 `git add` 將檔案加入暫存區

3. **倉庫（Repository）**
   - 已提交的版本歷史
   - 使用 `git commit` 將暫存區的變更提交到倉庫

### 提交（Commit）

提交是 Git 中版本的基本單位，包含：
- 變更的檔案內容
- 提交訊息（說明這次變更）
- 提交者資訊
- 時間戳記
- 唯一的 SHA-1 雜湊值

### 分支（Branch）

分支是開發線的獨立副本，允許：
- 同時進行多個功能開發
- 實驗性變更不影響主線
- 輕鬆切換不同版本

### 遠端倉庫（Remote Repository）

遠端倉庫是存放在伺服器上的 Git 倉庫，用於：
- 團隊協作
- 備份程式碼
- 發布版本

常見的遠端倉庫服務：
- GitHub
- GitLab
- Bitbucket
- Gitee

## 安裝 Git

### Debian/Ubuntu

```bash
# 安裝 Git
sudo apt update
sudo apt install git

# 檢查版本
git --version
```

## 初始設定

### 設定使用者資訊

```bash
# 設定全域使用者名稱
git config --global user.name "Your Name"

# 設定全域電子郵件
git config --global user.email "your.email@example.com"

# 查看設定
git config --list
git config user.name
git config user.email
```

### 常用設定

```bash
# 設定預設編輯器
git config --global core.editor "vim"

# 設定預設分支名稱
git config --global init.defaultBranch main

# 設定顏色輸出
git config --global color.ui auto

# 設定行尾符號處理（Windows）
git config --global core.autocrlf true

# 設定行尾符號處理（Linux/Mac）
git config --global core.autocrlf input

# 設定別名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

## 學習路徑

### 初學者

1. **基本操作**：學習建立倉庫、加入檔案、提交變更
   - 參考：[基本操作](basic.md)

2. **遠端操作**：學習與遠端倉庫互動
   - 參考：[遠端操作](remote.md)

3. **分支管理**：學習建立和使用分支
   - 參考：[分支管理](branch.md)

### 進階使用者

4. **進階功能**：學習進階 Git 功能
   - 參考：[進階功能](advanced.md)

## 文件結構

本教學分為以下章節：

1. **[基本操作](basic.md)**
   - 建立倉庫
   - 檔案追蹤
   - 提交變更
   - 查看歷史
   - 撤銷操作

2. **[遠端操作](remote.md)**
   - 克隆倉庫
   - 推送變更
   - 拉取更新
   - 遠端管理

3. **[分支管理](branch.md)**
   - 建立分支
   - 切換分支
   - 合併分支
   - 解決衝突

4. **[進階功能](advanced.md)**
   - 暫存變更
   - 標籤管理
   - 重置操作
   - 其他進階功能

## 快速參考

### 常用指令

```bash
# 初始化倉庫
git init

# 查看狀態
git status

# 加入檔案
git add file.txt
git add .

# 提交變更
git commit -m "提交訊息"

# 查看歷史
git log

# 克隆遠端倉庫
git clone <url>

# 推送變更
git push

# 拉取更新
git pull

# 建立分支
git branch new-branch

# 切換分支
git checkout branch-name
```

## 最佳實踐

1. **頻繁提交**：小步提交，每次提交都有明確目的
2. **清晰的提交訊息**：說明變更的原因和內容
3. **使用分支**：新功能在獨立分支開發
4. **定期同步**：經常從遠端拉取更新
5. **不要提交敏感資訊**：避免提交密碼、API 金鑰等
6. **使用 .gitignore**：忽略不需要版本控制的檔案

## 參考資源

- 官方文件：https://git-scm.com/doc
- 互動式教學：https://learngitbranching.js.org/
- Pro Git 電子書：https://git-scm.com/book
- GitHub 教學：https://guides.github.com/