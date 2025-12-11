# 常用 Linux 指令

## 檔案與目錄操作

### touch

建立空檔案或更新檔案的時間戳記。

```bash
# 建立空檔案
touch filename.txt

# 建立多個檔案
touch file1.txt file2.txt file3.txt

# 更新檔案時間戳記為目前時間
touch filename.txt

# 設定特定的時間戳記
touch -t 202401011200 filename.txt  # 格式：YYYYMMDDHHMM

# 只更新存取時間
touch -a filename.txt

# 只更新修改時間
touch -m filename.txt

# 參考另一個檔案的時間
touch -r reference.txt filename.txt
```

**常用選項：**
- `-a`：只更新存取時間
- `-m`：只更新修改時間
- `-t`：設定特定時間
- `-r`：參考另一個檔案的時間
- `-c`：不建立檔案（如果檔案不存在）

### ls

列出目錄內容。

```bash
# 列出目前目錄內容
ls

# 列出詳細資訊
ls -l

# 列出所有檔案（包含隱藏檔案）
ls -a

# 組合使用
ls -la

# 以人類可讀格式顯示檔案大小
ls -lh

# 按時間排序
ls -lt        # 按修改時間排序
ls -ltr       # 反向排序（最舊的在前）

# 按大小排序
ls -lS        # 按大小排序
ls -lSr       # 反向排序

# 遞迴列出子目錄
ls -R

# 顯示檔案類型
ls -F         # 在目錄後加 /，可執行檔後加 *

# 只顯示目錄
ls -d */

# 彩色輸出
ls --color=auto
```

**常用選項：**
- `-l`：詳細列表格式
- `-a`：顯示所有檔案（包含隱藏檔案）
- `-h`：人類可讀的檔案大小
- `-t`：按時間排序
- `-S`：按大小排序
- `-r`：反向排序
- `-R`：遞迴列出
- `-F`：顯示檔案類型標記

### cd

切換目錄。

```bash
# 切換到指定目錄
cd /path/to/directory

# 切換到家目錄
cd
cd ~

# 切換到上一個目錄
cd -

# 切換到上層目錄
cd ..

# 切換到上上層目錄
cd ../..

# 使用環境變數
cd $HOME
```

### pwd

顯示目前工作目錄。

```bash
# 顯示目前目錄
pwd

# 顯示實際路徑（解析符號連結）
pwd -P
```

### mkdir

建立目錄。

```bash
# 建立單一目錄
mkdir directory_name

# 建立多個目錄
mkdir dir1 dir2 dir3

# 建立巢狀目錄
mkdir -p parent/child/grandchild

# 設定權限
mkdir -m 755 directory_name

# 顯示建立過程
mkdir -v directory_name
```

**常用選項：**
- `-p`：建立父目錄（如果不存在）
- `-m`：設定權限
- `-v`：顯示詳細資訊

### rmdir

刪除空目錄。

```bash
# 刪除空目錄
rmdir directory_name

# 刪除多個空目錄
rmdir dir1 dir2

# 遞迴刪除空目錄
rmdir -p parent/child/grandchild
```

### rm

刪除檔案或目錄。

```bash
# 刪除檔案
rm filename.txt

# 刪除多個檔案
rm file1.txt file2.txt

# 刪除前確認
rm -i filename.txt

# 強制刪除（不提示）
rm -f filename.txt

# 遞迴刪除目錄
rm -r directory_name

# 刪除目錄（不提示）
rm -rf directory_name

# 顯示刪除過程
rm -v filename.txt
```

**常用選項：**
- `-r, -R`：遞迴刪除
- `-f`：強制刪除（不提示）
- `-i`：刪除前確認
- `-v`：顯示詳細資訊

**警告：** `rm -rf` 非常危險，使用時要特別小心！

### cp

複製檔案或目錄。

```bash
# 複製檔案
cp source.txt destination.txt

# 複製到目錄
cp file.txt /path/to/directory/

# 複製多個檔案
cp file1.txt file2.txt /path/to/directory/

# 遞迴複製目錄
cp -r source_dir destination_dir

# 保留檔案屬性
cp -p source.txt destination.txt

# 複製前確認
cp -i source.txt destination.txt

# 顯示複製過程
cp -v source.txt destination.txt

# 更新模式（只複製較新的檔案）
cp -u source.txt destination.txt
```

**常用選項：**
- `-r, -R`：遞迴複製目錄
- `-p`：保留檔案屬性（時間、權限等）
- `-i`：覆蓋前確認
- `-v`：顯示詳細資訊
- `-u`：只複製較新的檔案

### mv

移動或重新命名檔案。

```bash
# 移動檔案
mv source.txt /path/to/destination/

# 重新命名檔案
mv oldname.txt newname.txt

# 移動多個檔案
mv file1.txt file2.txt /path/to/directory/

# 移動前確認
mv -i source.txt destination.txt

# 強制移動（不提示）
mv -f source.txt destination.txt

# 顯示移動過程
mv -v source.txt destination.txt
```

**常用選項：**
- `-i`：覆蓋前確認
- `-f`：強制移動（不提示）
- `-v`：顯示詳細資訊

## 檔案內容查看與搜尋

### cat

顯示檔案內容。

```bash
# 顯示檔案內容
cat filename.txt

# 顯示多個檔案
cat file1.txt file2.txt

# 顯示行號
cat -n filename.txt

# 顯示行號（不計算空行）
cat -b filename.txt

# 顯示不可見字元
cat -A filename.txt

# 合併檔案
cat file1.txt file2.txt > combined.txt

# 追加到檔案
cat file1.txt >> file2.txt
```

**常用選項：**
- `-n`：顯示行號
- `-b`：顯示行號（不計算空行）
- `-A`：顯示所有字元（包括不可見字元）
- `-s`：壓縮連續空行

### less

分頁顯示檔案內容（可向前向後瀏覽）。

```bash
# 開啟檔案
less filename.txt

# 從管道讀取
command | less
```

**less 內建指令：**
- `空格` 或 `Page Down`：向下翻頁
- `b` 或 `Page Up`：向上翻頁
- `Enter` 或 `↓`：向下移動一行
- `↑`：向上移動一行
- `/pattern`：搜尋（向下）
- `?pattern`：搜尋（向上）
- `n`：下一個匹配
- `N`：上一個匹配
- `q`：退出
- `h`：顯示說明

### more

分頁顯示檔案內容（只能向前瀏覽）。

```bash
# 開啟檔案
more filename.txt

# 從管道讀取
command | more
```

**more 內建指令：**
- `空格`：向下翻頁
- `Enter`：向下移動一行
- `q`：退出
- `/pattern`：搜尋

### head

顯示檔案開頭部分。

```bash
# 顯示前 10 行（預設）
head filename.txt

# 顯示前 n 行
head -n 20 filename.txt
head -20 filename.txt

# 顯示前 n 個位元組
head -c 100 filename.txt

# 從管道讀取
command | head -n 10
```

**常用選項：**
- `-n`：顯示行數
- `-c`：顯示位元組數

### tail

顯示檔案結尾部分。

```bash
# 顯示後 10 行（預設）
tail filename.txt

# 顯示後 n 行
tail -n 20 filename.txt
tail -20 filename.txt

# 即時監控檔案（跟隨模式）
tail -f filename.txt
tail -F filename.txt  # 檔案被刪除重建後繼續跟隨

# 顯示後 n 個位元組
tail -c 100 filename.txt

# 從指定行開始顯示
tail -n +20 filename.txt  # 從第 20 行開始顯示到結尾
```

**常用選項：**
- `-n`：顯示行數
- `-f`：即時跟隨檔案更新
- `-F`：跟隨模式（檔案重建後繼續）
- `-c`：顯示位元組數

### grep

搜尋檔案中的文字模式。

```bash
# 搜尋檔案中的文字
grep "pattern" filename.txt

# 搜尋多個檔案
grep "pattern" file1.txt file2.txt

# 遞迴搜尋目錄
grep -r "pattern" /path/to/directory

# 忽略大小寫
grep -i "pattern" filename.txt

# 顯示行號
grep -n "pattern" filename.txt

# 反向搜尋（顯示不匹配的行）
grep -v "pattern" filename.txt

# 只顯示匹配的檔案名稱
grep -l "pattern" *.txt

# 使用正則表達式
grep -E "pattern1|pattern2" filename.txt

# 顯示匹配前後的行
grep -A 3 "pattern" filename.txt  # 顯示匹配後 3 行
grep -B 3 "pattern" filename.txt  # 顯示匹配前 3 行
grep -C 3 "pattern" filename.txt  # 顯示匹配前後 3 行

# 從管道讀取
command | grep "pattern"
```

**常用選項：**
- `-i`：忽略大小寫
- `-n`：顯示行號
- `-r, -R`：遞迴搜尋
- `-v`：反向搜尋
- `-l`：只顯示檔案名稱
- `-E`：使用擴展正則表達式
- `-A n`：顯示匹配後 n 行
- `-B n`：顯示匹配前 n 行
- `-C n`：顯示匹配前後 n 行

### find

搜尋檔案和目錄。

```bash
# 在目前目錄搜尋檔案
find . -name "filename.txt"

# 在指定目錄搜尋
find /path/to/search -name "pattern"

# 搜尋所有 .txt 檔案
find . -name "*.txt"

# 按檔案類型搜尋
find . -type f        # 只搜尋檔案
find . -type d       # 只搜尋目錄

# 按大小搜尋
find . -size +100M   # 大於 100MB
find . -size -1M     # 小於 1MB

# 按時間搜尋
find . -mtime -7     # 7 天內修改的檔案
find . -mtime +30    # 30 天前修改的檔案

# 按權限搜尋
find . -perm 644

# 執行命令
find . -name "*.txt" -exec rm {} \;
find . -name "*.txt" -delete

# 組合條件
find . -name "*.txt" -type f -size +1M
```

**常用選項：**
- `-name`：按名稱搜尋
- `-type`：按類型搜尋（f=檔案, d=目錄）
- `-size`：按大小搜尋
- `-mtime`：按修改時間搜尋
- `-perm`：按權限搜尋
- `-exec`：對找到的檔案執行命令
- `-delete`：刪除找到的檔案

## 系統資訊與幫助

### man

顯示指令的說明文件（Manual）。

```bash
# 查看指令說明
man ls
man grep

# 查看特定章節
man 1 ls        # 第 1 章（使用者指令）
man 5 passwd    # 第 5 章（檔案格式）

# 搜尋說明文件
man -k keyword
apropos keyword

# 顯示簡短說明
whatis command
```

**man 章節說明：**
- 1：使用者指令
- 2：系統呼叫
- 3：C 函式庫
- 4：裝置檔案
- 5：檔案格式
- 6：遊戲
- 7：雜項
- 8：系統管理指令

**man 內建指令：**
- `空格` 或 `Page Down`：向下翻頁
- `b` 或 `Page Up`：向上翻頁
- `/pattern`：搜尋
- `n`：下一個匹配
- `N`：上一個匹配
- `q`：退出
- `h`：顯示說明

### info

顯示資訊文件（Info 格式，比 man 更詳細）。

```bash
# 查看指令資訊
info ls
info grep

# 搜尋資訊文件
info --apropos=keyword
```

### which

顯示指令的完整路徑。

```bash
# 找出指令位置
which ls
which python

# 顯示所有匹配
which -a python
```

### whereis

找出指令、原始碼和說明文件的位置。

```bash
# 找出指令相關檔案
whereis ls
whereis python
```

### whatis

顯示指令的簡短說明。

```bash
# 顯示指令說明
whatis ls
whatis grep
```

### apropos

搜尋指令說明。

```bash
# 搜尋相關指令
apropos "search pattern"
man -k "search pattern"  # 同義
```

## 文字處理

### wc

計算檔案的行數、字數和字元數。

```bash
# 顯示行數、字數、字元數
wc filename.txt

# 只顯示行數
wc -l filename.txt

# 只顯示字數
wc -w filename.txt

# 只顯示字元數
wc -c filename.txt

# 從管道讀取
command | wc -l
```

**常用選項：**
- `-l`：行數
- `-w`：字數
- `-c`：字元數
- `-m`：字元數（多位元組字元）

### sort

排序檔案內容。

```bash
# 排序檔案
sort filename.txt

# 反向排序
sort -r filename.txt

# 數值排序
sort -n filename.txt

# 忽略大小寫
sort -f filename.txt

# 去除重複行
sort -u filename.txt

# 指定欄位排序
sort -k 2 filename.txt  # 按第 2 欄排序

# 從管道讀取
command | sort
```

**常用選項：**
- `-r`：反向排序
- `-n`：數值排序
- `-f`：忽略大小寫
- `-u`：去除重複
- `-k`：指定排序欄位

### uniq

去除或顯示重複行。

```bash
# 去除連續重複行
uniq filename.txt

# 顯示重複行
uniq -d filename.txt

# 顯示所有重複行（包含出現次數）
uniq -c filename.txt

# 只顯示唯一行
uniq -u filename.txt

# 通常與 sort 配合使用
sort filename.txt | uniq
```

**常用選項：**
- `-d`：只顯示重複行
- `-u`：只顯示唯一行
- `-c`：顯示出現次數
- `-i`：忽略大小寫

### cut

從檔案中提取欄位。

```bash
# 提取特定欄位（以空格分隔）
cut -d' ' -f1 filename.txt

# 提取多個欄位
cut -d',' -f1,3 filename.txt

# 提取字元範圍
cut -c1-10 filename.txt

# 從管道讀取
command | cut -d',' -f1
```

**常用選項：**
- `-d`：指定分隔符號
- `-f`：指定欄位
- `-c`：指定字元範圍

### sed

串流編輯器，用於文字替換和處理。

```bash
# 替換文字
sed 's/old/new/g' filename.txt

# 替換並儲存
sed -i 's/old/new/g' filename.txt

# 刪除行
sed '5d' filename.txt        # 刪除第 5 行
sed '/pattern/d' filename.txt  # 刪除匹配行

# 插入文字
sed '2i\New line' filename.txt  # 在第 2 行前插入
sed '2a\New line' filename.txt  # 在第 2 行後插入

# 從管道讀取
command | sed 's/old/new/g'
```

### awk

文字處理和資料提取工具。

```bash
# 顯示特定欄位
awk '{print $1}' filename.txt

# 顯示多個欄位
awk '{print $1, $3}' filename.txt

# 使用分隔符號
awk -F',' '{print $1}' filename.txt

# 條件處理
awk '$1 > 100 {print $0}' filename.txt

# 從管道讀取
command | awk '{print $1}'
```

## 檔案權限與屬性

### chmod

變更檔案權限。

```bash
# 使用數字設定權限
chmod 755 filename.txt

# 使用符號設定權限
chmod u+x filename.txt
chmod g+w filename.txt
chmod o-r filename.txt

# 遞迴變更
chmod -R 755 directory/

# 參考另一個檔案
chmod --reference=ref.txt filename.txt
```

### chown

變更檔案擁有者。

```bash
# 變更擁有者
sudo chown user filename.txt

# 變更擁有者和群組
sudo chown user:group filename.txt

# 遞迴變更
sudo chown -R user:group directory/
```

### chgrp

變更檔案群組。

```bash
# 變更群組
sudo chgrp group filename.txt

# 遞迴變更
sudo chgrp -R group directory/
```

## 系統監控

### ps

顯示執行中的程序。

```bash
# 顯示目前終端機的程序
ps

# 顯示所有程序
ps aux

# 顯示程序樹
ps auxf

# 搜尋特定程序
ps aux | grep process_name
```

### top

即時顯示系統程序資訊。

```bash
# 啟動 top
top

# 顯示特定使用者的程序
top -u username

# 更新間隔
top -d 5  # 每 5 秒更新
```

**top 內建指令：**
- `q`：退出
- `k`：終止程序
- `r`：重新設定優先級
- `h`：顯示說明

### htop

互動式程序監控工具（需要安裝）。

```bash
# 安裝
sudo apt install htop

# 啟動
htop
```

### free

顯示記憶體使用情況。

```bash
# 顯示記憶體資訊
free

# 以人類可讀格式顯示
free -h

# 持續監控
free -s 5  # 每 5 秒更新
```

### df

顯示檔案系統磁碟空間使用情況。

```bash
# 顯示所有檔案系統
df

# 以人類可讀格式顯示
df -h

# 顯示特定檔案系統類型
df -t ext4
```

### du

顯示目錄或檔案的磁碟使用量。

```bash
# 顯示目前目錄大小
du

# 以人類可讀格式顯示
du -h

# 顯示總計
du -sh directory/

# 顯示詳細資訊
du -ah directory/

# 只顯示目錄
du -d 1 -h
```

**常用選項：**
- `-h`：人類可讀格式
- `-s`：只顯示總計
- `-a`：顯示所有檔案
- `-d`：指定深度

## 網路相關

### ping

測試網路連線。

```bash
# 測試連線
ping google.com

# 指定次數
ping -c 4 google.com

# 指定間隔
ping -i 2 google.com
```

### wget

從網路下載檔案。

```bash
# 下載檔案
wget https://example.com/file.txt

# 下載到指定檔名
wget -O output.txt https://example.com/file.txt

# 繼續中斷的下載
wget -c https://example.com/file.txt

# 遞迴下載
wget -r https://example.com/
```

### curl

傳輸資料的工具。

```bash
# 下載檔案
curl -O https://example.com/file.txt

# 顯示內容
curl https://example.com/

# 儲存到檔案
curl -o output.txt https://example.com/file.txt

# 顯示標頭資訊
curl -I https://example.com/
```

## 壓縮與解壓縮

### tar

打包和壓縮檔案。

```bash
# 建立 tar 檔案
tar -cf archive.tar file1 file2

# 建立 gzip 壓縮檔案
tar -czf archive.tar.gz directory/

# 建立 bzip2 壓縮檔案
tar -cjf archive.tar.bz2 directory/

# 解壓縮
tar -xzf archive.tar.gz

# 列出內容
tar -tzf archive.tar.gz

# 解壓縮到指定目錄
tar -xzf archive.tar.gz -C /path/to/directory/
```

**常用選項：**
- `-c`：建立檔案
- `-x`：解壓縮
- `-t`：列出內容
- `-z`：使用 gzip
- `-j`：使用 bzip2
- `-f`：指定檔案名稱
- `-v`：顯示詳細資訊
- `-C`：指定解壓縮目錄

### zip / unzip

建立和解壓縮 zip 檔案。

```bash
# 建立 zip 檔案
zip archive.zip file1 file2

# 遞迴壓縮目錄
zip -r archive.zip directory/

# 解壓縮
unzip archive.zip

# 解壓縮到指定目錄
unzip archive.zip -d /path/to/directory/

# 列出內容
unzip -l archive.zip
```

## 其他實用指令

### history

顯示命令歷史記錄。

```bash
# 顯示歷史記錄
history

# 顯示最後 n 條記錄
history 20

# 搜尋歷史記錄
history | grep "pattern"

# 執行歷史記錄中的指令
!n          # 執行第 n 條指令
!!          # 執行上一條指令
!pattern    # 執行最近匹配的指令
```

### alias

建立指令別名。

```bash
# 建立別名
alias ll='ls -alF'
alias grep='grep --color=auto'

# 列出所有別名
alias

# 移除別名
unalias ll

# 永久設定（加入 ~/.bashrc）
echo "alias ll='ls -alF'" >> ~/.bashrc
```

### export

設定環境變數。

```bash
# 設定環境變數
export PATH="$PATH:/new/path"
export MY_VAR="value"

# 查看環境變數
export

# 移除環境變數
unset MY_VAR
```

### date

顯示或設定系統日期時間。

```bash
# 顯示目前日期時間
date

# 顯示特定格式
date +"%Y-%m-%d %H:%M:%S"

# 設定日期時間（需要 root）
sudo date -s "2024-01-01 12:00:00"
```

### cal

顯示月曆。

```bash
# 顯示本月月曆
cal

# 顯示指定年月
cal 1 2024

# 顯示整年
cal 2024
```

### whoami

顯示目前使用者名稱。

```bash
whoami
```

### id

顯示使用者 ID 和群組資訊。

```bash
# 顯示目前使用者資訊
id

# 顯示指定使用者資訊
id username
```

### uname

顯示系統資訊。

```bash
# 顯示系統資訊
uname -a

# 顯示核心名稱
uname -s

# 顯示核心版本
uname -r

# 顯示硬體架構
uname -m
```

## 管道與重導向

### 管道 (|)

將一個指令的輸出作為另一個指令的輸入。

```bash
# 範例
ls -l | grep "txt"
ps aux | grep "process"
cat file.txt | sort | uniq
```

### 重導向

```bash
# 輸出重導向
command > file.txt        # 覆蓋寫入
command >> file.txt       # 追加寫入

# 錯誤重導向
command 2> error.log      # 錯誤輸出到檔案
command 2>> error.log    # 追加錯誤

# 同時重導向標準輸出和錯誤
command > output.log 2>&1
command &> output.log

# 輸入重導向
command < input.txt

# Here Document
command << EOF
text
EOF
```

## 指令組合技巧

### 指令鏈接

```bash
# 執行多個指令（全部執行）
command1; command2; command3

# 邏輯 AND（前一個成功才執行下一個）
command1 && command2

# 邏輯 OR（前一個失敗才執行下一個）
command1 || command2
```

### 背景執行

```bash
# 在背景執行
command &

# 查看背景工作
jobs

# 將程序移到前景
fg %1

# 將程序移到背景
bg %1

# 終止背景程序
kill %1
```

## 實用範例

### 搜尋並替換多個檔案

```bash
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;
```

### 計算檔案總數

```bash
find . -type f | wc -l
```

### 找出最大的檔案

```bash
find . -type f -exec ls -lh {} \; | sort -k5 -hr | head -10
```

### 監控日誌檔案

```bash
tail -f /var/log/syslog
```

### 備份目錄

```bash
tar -czf backup_$(date +%Y%m%d).tar.gz /path/to/directory
```

### 找出並刪除舊檔案

```bash
find . -type f -mtime +30 -delete
```

## 學習建議

1. **從基本指令開始**：先掌握 ls、cd、pwd、mkdir、rm 等基本指令
2. **善用幫助文件**：使用 `man` 和 `--help` 查看指令說明
3. **練習管道和重導向**：這是提高效率的關鍵
4. **建立別名**：將常用指令組合設為別名
5. **閱讀系統日誌**：了解系統運作狀況
6. **實作練習**：透過實際操作加深理解

## 參考資源

- 指令說明：`man command` 或 `command --help`
- 線上文件：GNU Coreutils 文件
- 練習環境：在自己的 Linux 系統上實際操作