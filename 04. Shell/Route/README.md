# Linux 路徑概念

## 簡介

路徑（Path）是 Linux 系統中用來指定檔案或目錄位置的字串。理解路徑概念是使用 Linux 命令列的基礎，正確使用路徑可以準確地存取和管理檔案系統中的資源。

## 路徑類型

### 絕對路徑（Absolute Path）

絕對路徑是從根目錄（/）開始的完整路徑，無論目前在哪個目錄，絕對路徑都指向同一個位置。

```bash
# 絕對路徑範例
/home/user/documents/file.txt
/etc/passwd
/usr/bin/ls
/var/log/syslog
```

**特點：**
- 以 `/` 開頭
- 完整指定從根目錄到目標的路徑
- 不依賴目前工作目錄
- 唯一且明確

### 相對路徑（Relative Path）

相對路徑是相對於目前工作目錄的路徑，會根據目前所在位置而改變。

```bash
# 相對路徑範例
file.txt              # 目前目錄下的檔案
documents/file.txt    # 目前目錄下的子目錄中的檔案
../file.txt           # 上層目錄中的檔案
../../file.txt        # 上上層目錄中的檔案
```

**特點：**
- 不以 `/` 開頭
- 相對於目前工作目錄
- 簡潔但依賴上下文
- 需要知道目前位置

## 路徑符號

### 根目錄（/）

`/` 是檔案系統的根目錄，是所有路徑的起點。

```bash
# 根目錄
/

# 根目錄下的檔案和目錄
/etc
/home
/usr
/bin
```

**注意：**
- `/` 單獨使用表示根目錄
- `/` 在路徑開頭表示絕對路徑
- `/` 在路徑中間作為目錄分隔符號

### 家目錄（~）

`~` 代表目前使用者的家目錄。

```bash
# 家目錄
~

# 展開為完整路徑
echo ~
# 輸出：/home/username

# 使用範例
cd ~
cd ~/documents
ls ~/Downloads

# 指定使用者的家目錄
~username  # 特定使用者的家目錄
```

**等價關係：**
```bash
~          # 等同於 $HOME
~/doc      # 等同於 $HOME/doc
```

### 目前目錄（.）

`.` 代表目前工作目錄。

```bash
# 目前目錄
.

# 使用範例
ls .              # 列出目前目錄內容
./script.sh       # 執行目前目錄下的腳本
cp file.txt .     # 複製檔案到目前目錄
```

**常見用途：**
- 執行目前目錄下的可執行檔
- 明確指定目前目錄
- 在腳本中引用目前目錄

### 上層目錄（..）

`..` 代表上層目錄（父目錄）。

```bash
# 上層目錄
..

# 使用範例
cd ..             # 切換到上層目錄
ls ..             # 列出上層目錄內容
../script.sh      # 執行上層目錄下的腳本
../../file.txt    # 上上層目錄的檔案
```

**多層上移：**
```bash
../              # 上層目錄
../../           # 上上層目錄
../../../        # 上上上層目錄
```

### 上上層目錄（../..）

`../..` 代表上上層目錄，可以繼續往上層級。

```bash
# 上上層目錄
../..

# 使用範例
cd ../..          # 切換到上上層目錄
ls ../../         # 列出上上層目錄內容
```

## 路徑操作

### 查看目前路徑

```bash
# 顯示目前工作目錄（絕對路徑）
pwd

# 顯示目前工作目徑（解析符號連結）
pwd -P
```

### 切換路徑

```bash
# 使用絕對路徑
cd /home/user/documents

# 使用相對路徑
cd documents
cd ../documents

# 使用家目錄
cd ~
cd ~/documents

# 切換到上層目錄
cd ..

# 切換到上一個目錄
cd -
```

### 路徑展開

Shell 會自動展開某些路徑符號：

```bash
# ~ 展開為家目錄
echo ~
# 輸出：/home/username

# 環境變數展開
echo $HOME
# 輸出：/home/username

# 路徑組合
cd ~/documents
# 展開為：/home/username/documents
```

## 路徑組成

### 目錄分隔符號

Linux 使用 `/` 作為目錄分隔符號。

```bash
# 正確的路徑
/home/user/file.txt
/usr/local/bin

# 多個連續的 / 會被視為單一 /
/home//user//file.txt  # 等同於 /home/user/file.txt
```

### 路徑元素

路徑由多個元素組成：

```
/home/user/documents/file.txt
│  │    │     │         │
│  │    │     │         └─ 檔案名稱
│  │    │     └─────────── 目錄名稱
│  │    └───────────────── 使用者名稱
│  └────────────────────── 家目錄
└────────────────────────── 根目錄
```

### 檔案名稱和副檔名

```bash
# 完整路徑
/home/user/document.txt

# 檔案名稱：document.txt
# 目錄路徑：/home/user/
# 副檔名：.txt
```

## 特殊路徑

### 當前目錄的父目錄

```bash
# 目前目錄
.

# 上層目錄
..

# 上上層目錄
../..

# 組合使用
./script.sh        # 目前目錄下的腳本
../config.txt      # 上層目錄的設定檔
../../data/        # 上上層目錄的 data 目錄
```

### 路徑組合

```bash
# 組合路徑
/home/user/documents/file.txt

# 可以分解為
/home/user/        # 基礎路徑
documents/         # 子目錄
file.txt           # 檔案名稱

# 使用變數
BASE="/home/user"
FILE="file.txt"
FULL_PATH="$BASE/documents/$FILE"
```

## 路徑相關指令

### basename

取得路徑中的檔案名稱部分。

```bash
# 取得檔案名稱
basename /home/user/file.txt
# 輸出：file.txt

# 移除副檔名
basename /home/user/file.txt .txt
# 輸出：file

# 從管道讀取
echo "/home/user/file.txt" | basename
```

### dirname

取得路徑中的目錄部分。

```bash
# 取得目錄路徑
dirname /home/user/file.txt
# 輸出：/home/user

# 多層目錄
dirname /home/user/documents/file.txt
# 輸出：/home/user/documents

# 從管道讀取
echo "/home/user/file.txt" | dirname
```

### realpath

取得檔案的絕對路徑（解析符號連結）。

```bash
# 取得絕對路徑
realpath file.txt

# 解析符號連結
realpath -s symlink

# 檢查檔案是否存在
realpath -e file.txt
```

### readlink

讀取符號連結的目標。

```bash
# 讀取符號連結目標
readlink symlink

# 解析所有符號連結
readlink -f symlink

# 顯示完整路徑
readlink -e symlink
```

## 路徑模式匹配

### 萬用字元

```bash
# * 匹配任意字元
ls *.txt           # 所有 .txt 檔案
ls /home/*/file    # 所有使用者家目錄下的 file

# ? 匹配單一字元
ls file?.txt       # file1.txt, file2.txt 等

# [] 匹配字元集合
ls file[0-9].txt   # file0.txt 到 file9.txt
ls [a-z]*.txt      # 以小寫字母開頭的 .txt 檔案

# {} 展開多個模式
ls {file1,file2,file3}.txt
```

### 路徑展開

```bash
# ~ 展開
ls ~/documents/*.txt

# 變數展開
DOCS="$HOME/documents"
ls $DOCS/*.txt

# 命令替換
ls $(pwd)/*.txt
```

## 環境變數中的路徑

### PATH

系統搜尋可執行檔的路徑。

```bash
# 查看 PATH
echo $PATH

# PATH 通常包含
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin

# 新增路徑到 PATH
export PATH="$PATH:/new/path"

# 永久設定（加入 ~/.bashrc）
echo 'export PATH="$PATH:/new/path"' >> ~/.bashrc
```

### 其他路徑變數

```bash
# 家目錄
echo $HOME

# 目前目錄
echo $PWD

# 上一個目錄
echo $OLDPWD

# 使用者名稱
echo $USER
```

## 路徑操作範例

### 取得檔案所在目錄

```bash
# 方法 1：使用 dirname
DIR=$(dirname /home/user/file.txt)
echo $DIR
# 輸出：/home/user

# 方法 2：使用字串操作
PATH="/home/user/file.txt"
DIR="${PATH%/*}"
echo $DIR
# 輸出：/home/user
```

### 取得檔案名稱

```bash
# 方法 1：使用 basename
FILE=$(basename /home/user/file.txt)
echo $FILE
# 輸出：file.txt

# 方法 2：使用字串操作
PATH="/home/user/file.txt"
FILE="${PATH##*/}"
echo $FILE
# 輸出：file.txt
```

### 取得副檔名

```bash
# 使用字串操作
FILE="document.txt"
EXT="${FILE##*.}"
echo $EXT
# 輸出：txt

# 移除副檔名
NAME="${FILE%.*}"
echo $NAME
# 輸出：document
```

### 組合路徑

```bash
# 方法 1：直接組合
BASE="/home/user"
FILE="document.txt"
FULL="$BASE/documents/$FILE"

# 方法 2：使用變數
DIR="/home/user"
SUBDIR="documents"
FILE="document.txt"
FULL="$DIR/$SUBDIR/$FILE"

# 方法 3：在腳本中
BASE_DIR="${1:-$HOME}"  # 使用參數或預設值
FILE_NAME="file.txt"
FULL_PATH="$BASE_DIR/$FILE_NAME"
```

## 常見路徑問題

### 路徑中的空格

路徑包含空格時需要用引號包圍。

```bash
# 錯誤：空格會被視為分隔符號
cd /home/user/my documents

# 正確：使用引號
cd "/home/user/my documents"
cd '/home/user/my documents'

# 或使用跳脫字元
cd /home/user/my\ documents
```

### 路徑中的特殊字元

```bash
# 包含特殊字元的路徑
cd "/path/with/special (chars)"
cd '/path/with/$dollar'
cd /path/with/\$dollar

# 使用引號保護
ls "file with spaces.txt"
```

### 相對路徑的陷阱

```bash
# 問題：相對路徑依賴目前目錄
./script.sh  # 如果不在正確目錄會失敗

# 解決：使用絕對路徑或先切換目錄
cd /path/to/script
./script.sh

# 或使用完整路徑
/path/to/script/script.sh
```

## 實用範例

### 在腳本中使用路徑

```bash
#!/bin/bash
# 取得腳本所在目錄
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# 使用相對路徑
CONFIG_FILE="$SCRIPT_DIR/config.txt"
LOG_FILE="$SCRIPT_DIR/logs/app.log"

# 建立相對路徑的目錄
mkdir -p "$SCRIPT_DIR/logs"
```

### 路徑驗證

```bash
# 檢查路徑是否存在
if [ -e "/path/to/file" ]; then
    echo "檔案存在"
fi

# 檢查是否為目錄
if [ -d "/path/to/dir" ]; then
    echo "是目錄"
fi

# 檢查是否為檔案
if [ -f "/path/to/file" ]; then
    echo "是檔案"
fi
```

### 路徑標準化

```bash
# 移除多餘的斜線
PATH="/home//user//file.txt"
NORMALIZED=$(echo "$PATH" | sed 's|//*|/|g')
echo $NORMALIZED
# 輸出：/home/user/file.txt

# 移除結尾的斜線
DIR="/home/user/"
CLEAN="${DIR%/}"
echo $CLEAN
# 輸出：/home/user
```

## 路徑最佳實踐

1. **使用絕對路徑**：在腳本中使用絕對路徑，避免依賴目前目錄
2. **引用變數**：路徑變數使用引號，避免空格問題
3. **檢查存在性**：操作前檢查路徑是否存在
4. **使用 ~**：引用家目錄時使用 `~` 而非硬編碼路徑
5. **避免特殊字元**：檔案和目錄名稱避免使用特殊字元
6. **標準化路徑**：移除多餘的斜線和結尾斜線

## 常用指令速查

```bash
# 查看目前路徑
pwd

# 切換路徑
cd /path/to/dir
cd ~
cd ..
cd -

# 取得路徑部分
basename /path/to/file.txt
dirname /path/to/file.txt

# 路徑展開
echo ~
echo $HOME
echo $PWD

# 路徑操作
realpath file.txt
readlink symlink
```

## 總結

理解路徑概念是使用 Linux 的基礎：

- **絕對路徑**：從根目錄開始的完整路徑
- **相對路徑**：相對於目前目錄的路徑
- **路徑符號**：`/`（根目錄）、`~`（家目錄）、`.`（目前目錄）、`..`（上層目錄）
- **路徑操作**：使用 `pwd`、`cd`、`basename`、`dirname` 等指令
- **最佳實踐**：在腳本中使用絕對路徑，引用變數時使用引號

掌握這些概念可以更有效地管理和操作檔案系統。