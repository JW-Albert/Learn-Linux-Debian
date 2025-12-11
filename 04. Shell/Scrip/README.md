# Shell Script 教學

## 簡介

Shell Script 是一種使用 Shell 命令撰寫的腳本程式，可以自動化執行一系列系統操作。Shell Script 結合了命令列指令的便利性和程式設計的邏輯控制能力，是 Linux 系統管理的重要工具。

### Shell Script 的優點

1. **自動化任務**：可以將重複的操作編寫成腳本自動執行
2. **系統管理**：方便進行系統配置、備份、監控等任務
3. **快速開發**：不需要編譯，直接執行
4. **整合工具**：可以組合多個命令和工具
5. **可移植性**：遵循 POSIX 標準的腳本可以在不同系統上運行

## 建立第一個 Shell Script

### 基本結構

```bash
#!/bin/bash
# 這是註解
# Script 的描述

echo "Hello, World!"
```

### 建立和執行腳本

```bash
# 1. 建立腳本檔案
nano hello.sh

# 2. 加入內容（包含 shebang）
#!/bin/bash
echo "Hello, World!"

# 3. 給予執行權限
chmod +x hello.sh

# 4. 執行腳本
./hello.sh

# 或使用 bash 執行
bash hello.sh
```

### Shebang 行

腳本的第一行 `#!/bin/bash` 稱為 shebang，告訴系統使用哪個直譯器執行腳本。

```bash
#!/bin/bash      # 使用 bash
#!/bin/sh        # 使用 sh（POSIX 相容）
#!/usr/bin/zsh   # 使用 zsh
#!/usr/bin/env bash  # 使用環境變數中的 bash
```

## 變數

### 變數定義與使用

```bash
#!/bin/bash

# 定義變數（等號兩邊不能有空格）
name="Alice"
age=25
path="/home/user"

# 使用變數（使用 $ 符號）
echo $name
echo $age
echo $path

# 使用大括號（建議方式，避免歧義）
echo ${name}
echo "My name is ${name}"

# 變數名稱規則
# - 只能包含字母、數字、底線
# - 不能以數字開頭
# - 區分大小寫
```

### 變數類型

#### 字串變數

```bash
str1="Hello"
str2='World'
str3="Hello $name"  # 雙引號內可以展開變數
str4='Hello $name'  # 單引號內不展開變數

echo $str3  # 輸出：Hello Alice
echo $str4  # 輸出：Hello $name
```

#### 數值變數

```bash
num1=10
num2=20

# Bash 中進行算術運算
sum=$((num1 + num2))
echo $sum  # 輸出：30

# 其他運算
diff=$((num2 - num1))
product=$((num1 * num2))
quotient=$((num2 / num1))
remainder=$((num2 % num1))
```

#### 陣列變數

```bash
# 定義陣列
fruits=("apple" "banana" "orange")

# 存取陣列元素（索引從 0 開始）
echo ${fruits[0]}  # apple
echo ${fruits[1]}  # banana
echo ${fruits[2]}  # orange

# 存取所有元素
echo ${fruits[@]}
echo ${fruits[*]}

# 陣列長度
echo ${#fruits[@]}

# 新增元素
fruits[3]="grape"

# 關聯陣列（Bash 4+）
declare -A colors
colors["red"]="#FF0000"
colors["green"]="#00FF00"
echo ${colors["red"]}
```

### 特殊變數

```bash
#!/bin/bash

# $0：腳本名稱
echo "Script name: $0"

# $1, $2, ...：命令列參數
echo "First argument: $1"
echo "Second argument: $2"

# $#：參數個數
echo "Number of arguments: $#"

# $@：所有參數（作為獨立字串）
echo "All arguments: $@"

# $*：所有參數（作為單一字串）
echo "All arguments as one: $*"

# $?：上一個指令的退出狀態碼
ls /tmp
echo "Exit status: $?"

# $$：目前程序的 PID
echo "Process ID: $$"

# $!：最後一個背景程序的 PID
sleep 5 &
echo "Background PID: $!"

# $-：目前 Shell 的選項
echo "Shell options: $-"
```

### 環境變數

```bash
# 查看環境變數
echo $HOME      # 使用者家目錄
echo $USER      # 使用者名稱
echo $PATH      # 指令搜尋路徑
echo $PWD       # 目前目錄
echo $SHELL     # 目前 Shell

# 設定環境變數（僅在目前 Shell 有效）
export MY_VAR="value"

# 在腳本中使用環境變數
echo "Home directory: $HOME"
```

### 變數預設值

```bash
# ${變數:-預設值}：如果變數未設定或為空，使用預設值
name=${1:-"Guest"}
echo "Hello, $name"

# ${變數:=預設值}：如果變數未設定或為空，設定為預設值
count=${COUNT:=0}

# ${變數:+替代值}：如果變數已設定且非空，使用替代值
message=${DEBUG:+"Debug mode"}

# ${變數:?錯誤訊息}：如果變數未設定或為空，顯示錯誤並退出
required=${REQUIRED:?"REQUIRED must be set"}
```

### 字串操作

```bash
str="Hello World"

# 字串長度
echo ${#str}  # 11

# 字串截取
echo ${str:0:5}    # Hello（從位置 0 開始，長度 5）
echo ${str:6}      # World（從位置 6 到結尾）

# 字串取代
echo ${str/World/Unix}  # Hello Unix（只取代第一個）
echo ${str//l/L}        # HeLLo WorLd（取代所有）

# 字串前綴移除
path="/usr/local/bin"
echo ${path#/usr}        # /local/bin（移除最短匹配）
echo ${path##/usr}       # /local/bin（移除最長匹配）

# 字串後綴移除
file="script.sh"
echo ${file%.sh}         # script（移除最短匹配）
echo ${file%%.sh}        # script（移除最長匹配）

# 大小寫轉換（Bash 4+）
str="Hello"
echo ${str,,}    # hello（轉小寫）
echo ${str^^}    # HELLO（轉大寫）
```

## 輸入輸出

### 輸出

```bash
# echo：輸出文字
echo "Hello, World!"
echo -n "No newline"  # -n 不換行
echo -e "Line 1\nLine 2"  # -e 啟用轉義字元

# printf：格式化輸出
printf "Name: %s, Age: %d\n" "Alice" 25
printf "Number: %05d\n" 42  # 輸出：00042
```

### 輸入

```bash
# read：讀取使用者輸入
echo "Enter your name:"
read name
echo "Hello, $name!"

# read 進階用法
read -p "Enter name: " name          # 提示訊息
read -s password                      # 隱藏輸入（密碼）
read -t 10 timeout_input              # 設定超時（秒）
read -n 1 char                        # 只讀取一個字元
read -a array                         # 讀取為陣列
```

### 重導向

```bash
# 輸出重導向
echo "Hello" > file.txt          # 覆蓋寫入
echo "World" >> file.txt         # 追加寫入

# 輸入重導向
cat < file.txt
sort < input.txt > output.txt

# 錯誤重導向
command 2> error.log             # 錯誤輸出到檔案
command 2>> error.log            # 追加錯誤
command > output.log 2>&1        # 標準輸出和錯誤都重導向
command &> output.log            # 同上（簡寫）

# Here Document
cat << EOF
This is a multi-line
text block
EOF

# Here String
grep "pattern" <<< "text to search"
```

## 條件判斷

### if 語句

```bash
#!/bin/bash

# 基本語法
if [ condition ]; then
    commands
fi

# if-else
if [ condition ]; then
    commands1
else
    commands2
fi

# if-elif-else
if [ condition1 ]; then
    commands1
elif [ condition2 ]; then
    commands2
else
    commands3
fi
```

### 測試條件

#### 檔案測試

```bash
if [ -f file.txt ]; then
    echo "File exists"
fi

# 常用檔案測試
[ -f file ]      # 檔案存在且為一般檔案
[ -d dir ]       # 目錄存在
[ -e path ]      # 路徑存在（檔案或目錄）
[ -r file ]      # 檔案可讀
[ -w file ]      # 檔案可寫
[ -x file ]      # 檔案可執行
[ -s file ]      # 檔案存在且不為空
[ -L file ]      # 符號連結
```

#### 字串比較

```bash
str1="hello"
str2="world"

[ "$str1" = "$str2" ]    # 相等
[ "$str1" != "$str2" ]   # 不相等
[ -z "$str1" ]           # 字串為空
[ -n "$str1" ]           # 字串不為空
[ "$str1" < "$str2" ]    # 字串小於（字典順序）
[ "$str1" > "$str2" ]    # 字串大於
```

#### 數值比較

```bash
num1=10
num2=20

[ $num1 -eq $num2 ]    # 相等
[ $num1 -ne $num2 ]    # 不相等
[ $num1 -lt $num2 ]    # 小於
[ $num1 -le $num2 ]    # 小於等於
[ $num1 -gt $num2 ]    # 大於
[ $num1 -ge $num2 ]    # 大於等於
```

#### 邏輯運算

```bash
# &&：AND
if [ condition1 ] && [ condition2 ]; then
    commands
fi

# ||：OR
if [ condition1 ] || [ condition2 ]; then
    commands
fi

# !：NOT
if [ ! condition ]; then
    commands
fi

# 組合使用
if [ -f file.txt ] && [ -r file.txt ]; then
    echo "File exists and is readable"
fi
```

### case 語句

```bash
#!/bin/bash

case $1 in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
esac

# 多個模式
case $answer in
    y|Y|yes|YES)
        echo "Yes"
        ;;
    n|N|no|NO)
        echo "No"
        ;;
    *)
        echo "Invalid answer"
        ;;
esac
```

## 迴圈

### for 迴圈

```bash
# 基本語法
for item in list; do
    commands
done

# 範例
for fruit in apple banana orange; do
    echo $fruit
done

# 使用變數
fruits=("apple" "banana" "orange")
for fruit in "${fruits[@]}"; do
    echo $fruit
done

# 數字範圍
for i in {1..10}; do
    echo $i
done

# C 風格語法
for ((i=1; i<=10; i++)); do
    echo $i
done

# 遍歷檔案
for file in *.txt; do
    echo "Processing $file"
done
```

### while 迴圈

```bash
# 基本語法
while [ condition ]; do
    commands
done

# 範例
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done

# 讀取檔案
while read line; do
    echo "Line: $line"
done < file.txt

# 無限迴圈
while true; do
    echo "Running..."
    sleep 1
done
```

### until 迴圈

```bash
# until 與 while 相反，條件為假時執行
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### 迴圈控制

```bash
# break：跳出迴圈
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done

# continue：跳過本次迭代
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue
    fi
    echo $i
done

# break n：跳出 n 層迴圈
for i in {1..3}; do
    for j in {1..3}; do
        if [ $j -eq 2 ]; then
            break 2  # 跳出兩層迴圈
        fi
        echo "$i-$j"
    done
done
```

## 函式

### 函式定義

```bash
# 方法 1
function function_name() {
    commands
}

# 方法 2
function_name() {
    commands
}

# 範例
greet() {
    echo "Hello, $1!"
}

greet "Alice"  # 輸出：Hello, Alice!
```

### 函式參數

```bash
# 函式參數使用 $1, $2, ...（與腳本參數獨立）
add() {
    local sum=$(( $1 + $2 ))
    echo $sum
}

result=$(add 10 20)
echo "Sum: $result"
```

### 函式返回值

```bash
# 使用 return（退出狀態碼 0-255）
check_file() {
    if [ -f "$1" ]; then
        return 0  # 成功
    else
        return 1  # 失敗
    fi
}

check_file "test.txt"
if [ $? -eq 0 ]; then
    echo "File exists"
fi

# 使用 echo 輸出（可以返回任意值）
get_name() {
    echo "Alice"
}

name=$(get_name)
echo "Name: $name"
```

### 區域變數

```bash
global_var="global"

my_function() {
    local local_var="local"
    global_var="modified"
    echo "Local: $local_var"
    echo "Global: $global_var"
}

my_function
echo "Outside: $global_var"  # modified
echo "Outside: $local_var"    # 空（未定義）
```

## 錯誤處理

### 退出狀態碼

```bash
# 成功：0
# 失敗：非 0（1-255）

# 設定退出狀態碼
exit 0   # 成功
exit 1   # 一般錯誤
exit 2   # 用法錯誤

# 檢查命令是否成功
if command; then
    echo "Success"
else
    echo "Failed"
fi
```

### 錯誤處理選項

```bash
#!/bin/bash

# set -e：遇到錯誤立即退出
set -e
command_that_might_fail

# set -u：使用未定義變數時報錯
set -u
echo $undefined_var  # 會報錯

# set -x：顯示執行的命令
set -x
echo "Debug mode"

# set -o pipefail：管道中任何命令失敗都視為失敗
set -o pipefail
command1 | command2 | command3

# 組合使用
set -euo pipefail
```

### 錯誤處理範例

```bash
#!/bin/bash

# 檢查命令是否存在
if ! command -v git &> /dev/null; then
    echo "Error: git is not installed"
    exit 1
fi

# 檢查檔案是否存在
if [ ! -f "config.txt" ]; then
    echo "Error: config.txt not found"
    exit 1
fi

# 使用 trap 處理錯誤
cleanup() {
    echo "Cleaning up..."
    rm -f temp_file.txt
}

trap cleanup EXIT  # 腳本退出時執行 cleanup
trap 'echo "Error on line $LINENO"' ERR  # 錯誤時執行
```

## 實用範例

### 備份腳本

```bash
#!/bin/bash

BACKUP_DIR="/backup"
SOURCE_DIR="/home/user"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

# 建立備份目錄
mkdir -p $BACKUP_DIR

# 執行備份
tar -czf $BACKUP_FILE $SOURCE_DIR

if [ $? -eq 0 ]; then
    echo "Backup successful: $BACKUP_FILE"
else
    echo "Backup failed!"
    exit 1
fi
```

### 系統監控腳本

```bash
#!/bin/bash

# 檢查磁碟使用率
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ $DISK_USAGE -gt 80 ]; then
    echo "Warning: Disk usage is ${DISK_USAGE}%"
fi

# 檢查記憶體使用率
MEM_USAGE=$(free | grep Mem | awk '{printf "%.0f", $3/$2 * 100}')

if [ $MEM_USAGE -gt 90 ]; then
    echo "Warning: Memory usage is ${MEM_USAGE}%"
fi
```

### 檔案處理腳本

```bash
#!/bin/bash

# 處理目錄中的所有檔案
for file in *.txt; do
    if [ -f "$file" ]; then
        echo "Processing $file..."
        # 執行處理操作
        sed 's/old/new/g' "$file" > "${file}.new"
    fi
done
```

### 使用者輸入驗證

```bash
#!/bin/bash

read -p "Enter a number (1-10): " num

# 驗證輸入
if ! [[ "$num" =~ ^[0-9]+$ ]]; then
    echo "Error: Not a number"
    exit 1
fi

if [ $num -lt 1 ] || [ $num -gt 10 ]; then
    echo "Error: Number out of range"
    exit 1
fi

echo "Valid number: $num"
```

## 除錯技巧

```bash
#!/bin/bash

# 啟用除錯模式
set -x  # 顯示每條執行的命令
set +x  # 關閉除錯模式

# 在特定位置啟用除錯
set -x
# 需要除錯的程式碼
set +x

# 使用 bash -x 執行腳本
# bash -x script.sh

# 檢查語法
bash -n script.sh

# 詳細模式
bash -v script.sh
```

## 最佳實踐

1. **使用 shebang**：每個腳本都應該有正確的 shebang
2. **加入註解**：說明腳本的目的和複雜邏輯
3. **錯誤處理**：檢查命令執行結果，適當處理錯誤
4. **使用引號**：變數使用時加上引號，避免空格問題
5. **使用區域變數**：函式中使用 `local` 關鍵字
6. **檢查輸入**：驗證使用者輸入和檔案存在性
7. **使用有意義的變數名**：提高可讀性
8. **遵循編碼規範**：保持一致的縮排和風格
9. **測試腳本**：在不同情況下測試腳本
10. **記錄日誌**：重要操作記錄到日誌檔案

## 常用指令速查

```bash
# 變數操作
${var}              # 變數值
${var:-default}     # 預設值
${#var}             # 字串長度
${var:start:length} # 字串截取

# 條件測試
[ condition ]       # 測試條件
[[ condition ]]     # Bash 擴展測試（推薦）

# 算術運算
$((expression))     # 算術展開
let "var=1+1"       # let 指令

# 字串操作
${str/old/new}      # 字串取代
${str#pattern}      # 移除前綴
${str%pattern}      # 移除後綴
```