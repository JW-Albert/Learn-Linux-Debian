# Git 基本操作

## 建立倉庫

### 初始化新倉庫

```bash
# 在目前目錄初始化 Git 倉庫
git init

# 在指定目錄初始化
git init project-name

# 初始化並設定預設分支名稱
git init -b main
```

初始化後會建立 `.git` 目錄，包含所有版本控制資訊。

### 克隆現有倉庫

```bash
# 克隆倉庫到目前目錄
git clone https://github.com/user/repo.git

# 克隆到指定目錄
git clone https://github.com/user/repo.git my-project

# 克隆特定分支
git clone -b branch-name https://github.com/user/repo.git

# 淺克隆（只克隆最新版本）
git clone --depth 1 https://github.com/user/repo.git
```

## 檔案追蹤

### 查看狀態

```bash
# 查看工作區狀態
git status

# 簡潔格式
git status -s
git status --short

# 顯示詳細資訊
git status -v
```

**狀態說明：**
- `Untracked`：未追蹤的檔案
- `Modified`：已修改的檔案
- `Staged`：已加入暫存區的檔案
- `Deleted`：已刪除的檔案

### 加入檔案到暫存區

```bash
# 加入單一檔案
git add file.txt

# 加入多個檔案
git add file1.txt file2.txt

# 加入所有檔案
git add .

# 加入所有變更（包含刪除）
git add -A
git add --all

# 加入所有變更（目前目錄及子目錄）
git add -u

# 互動式加入
git add -i
git add --interactive

# 加入符合模式的檔案
git add *.txt
git add src/*.js
```

### 移除檔案

```bash
# 從 Git 追蹤中移除檔案（保留工作區檔案）
git rm --cached file.txt

# 從 Git 追蹤中移除檔案（同時刪除工作區檔案）
git rm file.txt

# 強制移除
git rm -f file.txt

# 遞迴移除目錄
git rm -r directory/
```

### .gitignore

建立 `.gitignore` 檔案來忽略不需要版本控制的檔案。

```bash
# 建立 .gitignore
nano .gitignore
```

**常用規則：**

```gitignore
# 忽略特定檔案
secret.txt
config.ini

# 忽略特定目錄
node_modules/
build/
dist/

# 忽略特定副檔名
*.log
*.tmp
*.swp

# 忽略但保留目錄結構
*.class
!important.class

# 忽略特定路徑
/path/to/file.txt

# 使用註解
# 這是註解
```

**常用 .gitignore 範例：**

```gitignore
# 編譯產物
*.o
*.class
*.exe
*.dll

# 編輯器檔案
.vscode/
.idea/
*.swp
*.swo
*~

# 作業系統檔案
.DS_Store
Thumbs.db

# 日誌檔案
*.log
logs/

# 依賴目錄
node_modules/
vendor/
```

## 提交變更

### 建立提交

```bash
# 提交暫存區的變更
git commit

# 提交並加入提交訊息
git commit -m "提交訊息"

# 提交並加入詳細訊息
git commit -m "標題" -m "詳細說明"

# 加入所有已追蹤的變更並提交（跳過暫存區）
git commit -a -m "提交訊息"
git commit -am "提交訊息"

# 修改最後一次提交
git commit --amend

# 修改最後一次提交訊息
git commit --amend -m "新的提交訊息"

# 加入檔案到最後一次提交
git add forgotten-file.txt
git commit --amend --no-edit
```

### 提交訊息規範

良好的提交訊息應該：

1. **簡潔的標題**（第一行，50 字以內）
2. **空一行**
3. **詳細說明**（可選，72 字換行）

**範例：**

```
修復登入頁面的驗證錯誤

- 修正密碼驗證邏輯
- 加入錯誤訊息顯示
- 更新相關測試

Fixes #123
```

**常見前綴：**
- `feat:`：新功能
- `fix:`：修復錯誤
- `docs:`：文件變更
- `style:`：程式碼格式
- `refactor:`：重構
- `test:`：測試
- `chore:`：雜項

## 查看歷史

### git log

```bash
# 查看提交歷史
git log

# 簡潔格式（一行）
git log --oneline

# 圖形化顯示分支
git log --graph

# 顯示所有分支
git log --all

# 顯示統計資訊
git log --stat

# 顯示變更內容
git log -p
git log --patch

# 限制顯示數量
git log -n 5
git log -5

# 搜尋提交訊息
git log --grep="關鍵字"

# 按作者搜尋
git log --author="作者名稱"

# 按日期範圍
git log --since="2024-01-01"
git log --until="2024-12-31"
git log --since="2 weeks ago"

# 按檔案
git log -- file.txt
git log --follow -- file.txt  # 追蹤檔案重新命名

# 組合使用
git log --oneline --graph --all
git log --stat --author="Alice" --since="1 month ago"
```

### git show

```bash
# 顯示最新提交的詳細資訊
git show

# 顯示特定提交
git show commit-hash

# 顯示特定提交的特定檔案
git show commit-hash:file.txt

# 顯示統計資訊
git show --stat
```

### git diff

```bash
# 查看工作區與暫存區的差異
git diff

# 查看暫存區與倉庫的差異
git diff --staged
git diff --cached

# 查看兩個提交之間的差異
git diff commit1 commit2

# 查看特定檔案的差異
git diff file.txt

# 查看統計資訊
git diff --stat

# 查看特定提交的變更
git diff HEAD~1 HEAD
```

## 撤銷操作

### 撤銷工作區變更

```bash
# 撤銷單一檔案的變更
git checkout -- file.txt
git restore file.txt

# 撤銷所有工作區變更
git checkout -- .
git restore .

# 撤銷特定檔案的變更
git restore file.txt
```

### 撤銷暫存區變更

```bash
# 將檔案從暫存區移除（保留工作區變更）
git reset HEAD file.txt
git restore --staged file.txt

# 將所有檔案從暫存區移除
git reset HEAD
git restore --staged .
```

### 修改最後一次提交

```bash
# 修改提交訊息
git commit --amend -m "新的提交訊息"

# 加入遺漏的檔案
git add forgotten-file.txt
git commit --amend --no-edit

# 修改提交者資訊
git commit --amend --author="Name <email>"
```

### 撤銷提交（保留變更）

```bash
# 撤銷最後一次提交（變更保留在工作區）
git reset --soft HEAD~1

# 撤銷最後一次提交（變更保留在工作區和暫存區）
git reset --mixed HEAD~1
git reset HEAD~1

# 撤銷多個提交
git reset --soft HEAD~3
```

### 撤銷提交（不保留變更）

```bash
# 警告：這會永久刪除變更
git reset --hard HEAD~1

# 恢復到特定提交
git reset --hard commit-hash
```

## 檔案操作

### 重新命名檔案

```bash
# 使用 Git 重新命名（會自動加入暫存區）
git mv old-name.txt new-name.txt

# 等同於
mv old-name.txt new-name.txt
git add new-name.txt
git rm old-name.txt
```

### 查看檔案內容

```bash
# 查看工作區檔案
cat file.txt

# 查看特定版本的檔案
git show HEAD:file.txt
git show commit-hash:file.txt

# 查看特定分支的檔案
git show branch-name:file.txt
```

## 實用範例

### 建立新專案

```bash
# 1. 建立專案目錄
mkdir my-project
cd my-project

# 2. 初始化 Git 倉庫
git init

# 3. 建立 .gitignore
echo "*.log" > .gitignore
echo "node_modules/" >> .gitignore

# 4. 建立初始檔案
echo "# My Project" > README.md

# 5. 加入檔案
git add .

# 6. 建立第一次提交
git commit -m "Initial commit"
```

### 日常工作流程

```bash
# 1. 查看狀態
git status

# 2. 加入變更
git add file.txt

# 3. 提交變更
git commit -m "描述變更"

# 4. 查看歷史
git log --oneline -5
```

### 撤銷錯誤操作

```bash
# 如果不小心修改了檔案
git restore file.txt

# 如果不小心加入了不需要的檔案
git restore --staged file.txt

# 如果提交訊息寫錯了
git commit --amend -m "正確的訊息"
```

## 常用指令速查

```bash
# 初始化
git init

# 狀態
git status
git status -s

# 加入檔案
git add file.txt
git add .

# 提交
git commit -m "訊息"
git commit -am "訊息"

# 查看歷史
git log
git log --oneline
git log --graph --all

# 查看差異
git diff
git diff --staged

# 撤銷
git restore file.txt
git restore --staged file.txt
git commit --amend
```