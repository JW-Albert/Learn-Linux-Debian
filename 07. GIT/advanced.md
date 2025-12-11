# Git 進階功能

## 暫存變更

### git stash

暫時儲存工作區的變更，以便切換分支或拉取更新。

```bash
# 暫存目前變更
git stash

# 暫存並加入說明
git stash save "說明訊息"

# 暫存包含未追蹤的檔案
git stash -u
git stash --include-untracked

# 暫存包含被忽略的檔案
git stash -a
git stash --all

# 查看暫存列表
git stash list

# 套用最近的暫存
git stash apply

# 套用指定的暫存
git stash apply stash@{0}

# 套用並刪除暫存
git stash pop

# 刪除暫存
git stash drop stash@{0}

# 清空所有暫存
git stash clear

# 查看暫存內容
git stash show
git stash show -p  # 顯示詳細差異
```

### 實用範例

```bash
# 場景：正在開發功能，需要緊急修復錯誤

# 1. 暫存目前工作
git stash save "功能開發中"

# 2. 切換到修復分支
git checkout -b hotfix/critical-bug

# 3. 修復錯誤並提交
git add .
git commit -m "修復關鍵錯誤"
git push origin hotfix/critical-bug

# 4. 切換回功能分支
git checkout feature/new-feature

# 5. 恢復暫存的工作
git stash pop
```

## 重置操作

### git reset

將 HEAD 和分支指標移動到指定提交。

```bash
# 軟重置（保留工作區和暫存區變更）
git reset --soft HEAD~1

# 混合重置（保留工作區變更，清除暫存區）
git reset --mixed HEAD~1
git reset HEAD~1

# 硬重置（清除所有變更，危險）
git reset --hard HEAD~1

# 重置到特定提交
git reset --hard commit-hash

# 重置到遠端分支
git reset --hard origin/main
```

**使用情境：**
- `--soft`：想保留變更並重新提交
- `--mixed`：想保留變更但重新整理暫存區
- `--hard`：想完全放棄變更（危險）

### git revert

建立新提交來撤銷指定提交的變更。

```bash
# 撤銷最後一次提交
git revert HEAD

# 撤銷特定提交
git revert commit-hash

# 撤銷多個提交
git revert commit1 commit2

# 撤銷但不自動提交
git revert --no-commit HEAD
```

**reset vs revert：**
- `reset`：修改歷史（適合未推送的提交）
- `revert`：建立新提交（適合已推送的提交）

## 選擇性提交

### git cherry-pick

選擇特定提交應用到目前分支。

```bash
# 套用特定提交
git cherry-pick commit-hash

# 套用多個提交
git cherry-pick commit1 commit2

# 套用提交範圍
git cherry-pick commit1^..commit2

# 只套用變更不自動提交
git cherry-pick -n commit-hash
git cherry-pick --no-commit commit-hash

# 編輯提交訊息
git cherry-pick -e commit-hash
git cherry-pick --edit commit-hash
```

### 實用範例

```bash
# 場景：將修復提交從主分支套用到功能分支

# 1. 在主分支找到修復提交的 hash
git log --oneline main

# 2. 切換到功能分支
git checkout feature-branch

# 3. 套用修復提交
git cherry-pick abc1234
```

## 搜尋和過濾

### git grep

在版本歷史中搜尋文字。

```bash
# 搜尋目前工作區
git grep "搜尋文字"

# 搜尋特定提交
git grep "搜尋文字" commit-hash

# 搜尋特定分支
git grep "搜尋文字" branch-name

# 顯示行號
git grep -n "搜尋文字"

# 忽略大小寫
git grep -i "搜尋文字"

# 顯示上下文
git grep -C 3 "搜尋文字"
```

### git log 進階

```bash
# 搜尋提交內容
git log -S "搜尋文字"

# 搜尋特定檔案的變更
git log -- file.txt

# 追蹤檔案重新命名
git log --follow -- file.txt

# 顯示變更統計
git log --stat

# 顯示變更內容
git log -p

# 圖形化顯示
git log --graph --oneline --all
```

## 子模組

### 新增子模組

```bash
# 新增子模組
git submodule add https://github.com/user/repo.git path/to/submodule

# 克隆包含子模組的倉庫
git clone --recursive https://github.com/user/repo.git
git clone --recurse-submodules https://github.com/user/repo.git
```

### 更新子模組

```bash
# 初始化子模組
git submodule init

# 更新子模組
git submodule update

# 更新所有子模組
git submodule update --init --recursive
```

### 移除子模組

```bash
# 1. 從 .gitmodules 移除
git rm --cached path/to/submodule

# 2. 從 .git/config 移除
git config -f .git/config --remove-section submodule.path/to/submodule

# 3. 刪除 .git/modules 中的目錄
rm -rf .git/modules/path/to/submodule

# 4. 刪除工作目錄
rm -rf path/to/submodule
```

## 鉤子（Hooks）

### 客戶端鉤子

位置：`.git/hooks/`

**常用鉤子：**

```bash
# pre-commit：提交前執行
#!/bin/sh
# 執行測試
npm test

# commit-msg：檢查提交訊息
#!/bin/sh
# 檢查提交訊息格式

# post-commit：提交後執行
#!/bin/sh
# 發送通知等
```

### 啟用鉤子

```bash
# 建立可執行的鉤子檔案
chmod +x .git/hooks/pre-commit

# 使用 Husky（Node.js 專案）
npm install --save-dev husky
npx husky install
```

## 進階設定

### .gitattributes

控制 Git 對特定檔案的處理方式。

```gitattributes
# 設定行尾符號
* text=auto
*.txt text
*.sh text eol=lf

# 二進位檔案
*.png binary
*.jpg binary

# 差異設定
*.pdf -diff
```

### 別名設定

```bash
# 建立常用別名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

### 進階別名範例

```bash
# 顯示簡潔狀態
git config --global alias.s 'status -sb'

# 顯示圖形化日誌
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# 顯示未推送的提交
git config --global alias.unpushed "log origin/master..HEAD"
```

## 效能優化

### 清理倉庫

```bash
# 清理未追蹤的檔案
git clean -n  # 預覽
git clean -f  # 執行

# 清理包含目錄
git clean -fd

# 清理被忽略的檔案
git clean -fX

# 垃圾回收
git gc

# 積極清理
git gc --aggressive --prune=now
```

### 壓縮歷史

```bash
# 使用 shallow clone
git clone --depth 1 <url>

# 截斷歷史（危險）
git checkout --orphan new-branch
git add .
git commit -m "新的開始"
git branch -D main
git branch -m main
```

## 除錯

### git bisect

使用二元搜尋找出引入錯誤的提交。

```bash
# 開始 bisect
git bisect start

# 標記目前版本為錯誤
git bisect bad

# 標記已知好的版本
git bisect good commit-hash

# Git 會自動切換到中間版本
# 測試後標記 good 或 bad
git bisect good
git bisect bad

# 完成後重置
git bisect reset
```

### 查看物件

```bash
# 查看物件資訊
git cat-file -t object-hash  # 類型
git cat-file -p object-hash  # 內容
git cat-file -s object-hash  # 大小

# 查看提交物件
git cat-file -p HEAD

# 查看樹物件
git ls-tree HEAD
```

## 實用範例

### 清理提交歷史

```bash
# 使用互動式 rebase 整理提交
git rebase -i HEAD~10

# 在編輯器中：
# - 將多個提交合併（squash）
# - 重新排序提交
# - 修改提交訊息（reword）
```

### 找回遺失的提交

```bash
# 查看所有操作記錄
git reflog

# 找到遺失的提交 hash
git reflog | grep "commit message"

# 恢復提交
git checkout commit-hash
git checkout -b recovered-branch commit-hash
```

### 部分暫存

```bash
# 互動式暫存
git add -i
git add --interactive

# 暫存檔案的部分變更
git add -p file.txt
git add --patch file.txt
```

## 常用指令速查

```bash
# 暫存
git stash
git stash pop
git stash list

# 重置
git reset --soft HEAD~1
git reset --hard HEAD~1

# 撤銷
git revert HEAD

# 選擇性提交
git cherry-pick commit-hash

# 搜尋
git grep "文字"
git log -S "文字"

# 清理
git clean -fd
git gc
```