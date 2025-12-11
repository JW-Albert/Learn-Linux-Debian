# Nano 文字編輯器使用指南

## 簡介

Nano 是一個簡單易用的命令列文字編輯器，特別適合初學者。它的最大特點是將常用快捷鍵顯示在畫面底部，使用者不需要記憶複雜的命令就能進行編輯。

## 安裝

```bash
# Debian/Ubuntu 系列
sudo apt install nano

# 檢查版本
nano --version
```

## 啟動 Nano

```bash
# 開啟新檔案
nano

# 開啟現有檔案
nano filename.txt

# 開啟檔案並指定行號
nano +10 filename.txt

# 以唯讀模式開啟
nano -v filename.txt

# 開啟多個檔案
nano file1.txt file2.txt
```

## 畫面說明

Nano 的畫面分為幾個部分：

1. **頂部**：顯示編輯器名稱、版本和目前檔案名稱
2. **中間**：編輯區域，顯示檔案內容
3. **底部**：快捷鍵提示列，顯示可用的命令

## 基本操作

### 游標移動

```
方向鍵          # 上下左右移動游標
Ctrl + F        # 向右移動一個字元
Ctrl + B        # 向左移動一個字元
Ctrl + P        # 向上移動一行
Ctrl + N        # 向下移動一行
Ctrl + A        # 移到行首
Ctrl + E        # 移到行尾
Ctrl + V        # 向下翻頁（Page Down）
Ctrl + Y        # 向上翻頁（Page Up）
Ctrl + _        # 跳到指定行號
```

### 文字輸入與編輯

```
直接輸入        # 在游標位置輸入文字
Backspace       # 刪除游標前的字元
Delete          # 刪除游標下的字元
Ctrl + D        # 刪除游標下的字元
Ctrl + H        # 刪除游標前的字元（同 Backspace）
Ctrl + K        # 刪除（剪下）整行
Ctrl + U        # 貼上（在游標位置）
Ctrl + 6        # 標記文字開始（用於複製/剪下）
```

### 複製與貼上

```
1. Ctrl + 6      # 開始標記文字（游標會變色）
2. 移動游標      # 選擇要複製的文字範圍
3. Alt + 6       # 複製標記的文字
   或
   Ctrl + K      # 剪下標記的文字
4. Ctrl + U      # 在游標位置貼上
```

### 搜尋與取代

```
Ctrl + W        # 搜尋文字
Alt + W         # 搜尋下一個
Ctrl + \        # 取代文字
```

**搜尋操作：**
1. 按 `Ctrl + W` 進入搜尋模式
2. 輸入要搜尋的文字
3. 按 Enter 開始搜尋
4. 使用 `Alt + W` 搜尋下一個匹配項

**取代操作：**
1. 按 `Ctrl + \` 進入取代模式
2. 輸入要搜尋的文字，按 Enter
3. 輸入要取代的文字，按 Enter
4. 選擇是否取代：
   - `Y`：取代目前匹配項
   - `N`：跳過目前匹配項
   - `A`：取代所有匹配項

### 檔案操作

```
Ctrl + O        # 儲存檔案（Write Out）
Ctrl + S        # 儲存檔案（同 Ctrl + O）
Ctrl + X        # 退出編輯器
Ctrl + R        # 讀取檔案插入目前位置
```

**儲存檔案：**
1. 按 `Ctrl + O`
2. 確認或修改檔案名稱
3. 按 Enter 儲存

**退出編輯器：**
1. 按 `Ctrl + X`
2. 如果有未儲存的變更，會提示是否儲存：
   - `Y`：儲存並退出
   - `N`：不儲存退出
   - `C`：取消退出

### 其他功能

```
Ctrl + G        # 顯示說明文件
Ctrl + C        # 顯示游標位置資訊
Ctrl + T        # 檢查拼字（需要拼字檢查器）
Ctrl + J        # 對齊段落
Ctrl + ]        # 跳轉到對應的括號
Alt + A         # 標記所有文字
Alt + D         # 計算字數
Alt + T         # 切換軟體換行
Alt + C         # 顯示行號
Alt + P         # 顯示標記
Alt + S         # 啟用/停用語法高亮
```

## 進階功能

### 多檔案編輯

當開啟多個檔案時：

```
Alt + ,         # 切換到上一個檔案
Alt + .         # 切換到下一個檔案
```

### 語法高亮

Nano 支援語法高亮，需要啟用：

```bash
# 使用 -Y 選項指定語法類型
nano -Y sh script.sh
nano -Y python script.py
nano -Y html index.html
```

或在設定檔中啟用：
```bash
# 編輯 ~/.nanorc
include /usr/share/nano/sh.nanorc
include /usr/share/nano/python.nanorc
```

### 軟體換行

```
Alt + T         # 切換軟體換行模式
```

啟用軟體換行後，長行會自動換行顯示，但不會在檔案中插入換行符號。

### 顯示行號

```
Alt + C         # 切換行號顯示
```

## 設定檔

### 系統設定檔

位置：`/etc/nanorc`

### 使用者設定檔

位置：`~/.nanorc`

### 常用設定範例

建立或編輯 `~/.nanorc`：

```bash
# 啟用語法高亮
include /usr/share/nano/sh.nanorc
include /usr/share/nano/python.nanorc

# 顯示行號
set linenumbers

# 啟用軟體換行
set softwrap

# 自動縮排
set autoindent

# 使用空格代替 Tab
set tabsize 4
set tabstospaces

# 顯示空白字元
set whitespace ".,"

# 備份檔案
set backup

# 備份目錄
set backupdir "/home/user/.nano-backups"
```

## 快捷鍵完整列表

### 檔案操作

| 快捷鍵 | 功能 |
|--------|------|
| Ctrl + O | 儲存檔案 |
| Ctrl + S | 儲存檔案 |
| Ctrl + X | 退出 |
| Ctrl + R | 讀取檔案插入 |

### 編輯操作

| 快捷鍵 | 功能 |
|--------|------|
| Ctrl + K | 刪除（剪下）整行 |
| Ctrl + U | 貼上 |
| Ctrl + 6 | 開始標記文字 |
| Alt + 6 | 複製標記的文字 |
| Ctrl + ] | 跳轉到對應括號 |

### 游標移動

| 快捷鍵 | 功能 |
|--------|------|
| Ctrl + F | 向右一個字元 |
| Ctrl + B | 向左一個字元 |
| Ctrl + P | 向上一行 |
| Ctrl + N | 向下一行 |
| Ctrl + A | 移到行首 |
| Ctrl + E | 移到行尾 |
| Ctrl + V | 向下翻頁 |
| Ctrl + Y | 向上翻頁 |
| Ctrl + _ | 跳到指定行號 |

### 搜尋與取代

| 快捷鍵 | 功能 |
|--------|------|
| Ctrl + W | 搜尋 |
| Alt + W | 搜尋下一個 |
| Ctrl + \ | 取代 |

### 其他功能

| 快捷鍵 | 功能 |
|--------|------|
| Ctrl + G | 顯示說明 |
| Ctrl + C | 顯示游標位置 |
| Alt + A | 標記所有文字 |
| Alt + C | 切換行號 |
| Alt + T | 切換軟體換行 |
| Alt + S | 切換語法高亮 |

## 實用範例

### 編輯系統設定檔

```bash
# 編輯 hosts 檔案
sudo nano /etc/hosts

# 編輯網路設定
sudo nano /etc/network/interfaces
```

### 建立 Shell 腳本

```bash
# 建立新腳本
nano myscript.sh

# 加入內容
#!/bin/bash
echo "Hello, World!"

# 儲存並退出
Ctrl + O
Enter
Ctrl + X
```

### 編輯多個檔案

```bash
# 同時開啟多個檔案
nano file1.txt file2.txt

# 在檔案間切換
Alt + .  # 下一個檔案
Alt + ,  # 上一個檔案
```

## 常見問題

### 如何顯示行號？

按 `Alt + C` 切換行號顯示，或在設定檔中加入 `set linenumbers`。

### 如何啟用語法高亮？

使用 `-Y` 選項指定語法類型，或在 `~/.nanorc` 中載入語法檔案。

### 如何複製多行文字？

1. 按 `Ctrl + 6` 開始標記
2. 移動游標選擇文字
3. 按 `Alt + 6` 複製
4. 移動到目標位置
5. 按 `Ctrl + U` 貼上

### 如何搜尋並取代所有匹配項？

使用 `Ctrl + \` 進入取代模式，輸入搜尋和取代文字後，選擇 `A` 取代所有。

### 如何跳到指定行號？

按 `Ctrl + _`，然後輸入行號，按 Enter。

## 與其他編輯器比較

### Nano vs Vim

**Nano 優點：**
- 學習曲線平緩
- 快捷鍵顯示在畫面上
- 操作直觀

**Nano 缺點：**
- 功能相對簡單
- 編輯大型檔案效率較低
- 自訂性較低

**適用場景：**
- 初學者
- 快速編輯小型檔案
- 簡單的文字編輯任務

## 最佳實踐

1. **學習基本快捷鍵**：掌握常用的 Ctrl + O、Ctrl + X、Ctrl + W 等
2. **使用設定檔**：根據需求自訂 `~/.nanorc`
3. **啟用語法高亮**：編輯程式碼時啟用語法高亮提高可讀性
4. **定期儲存**：編輯重要檔案時經常按 Ctrl + O 儲存
5. **使用標記功能**：編輯多行時使用標記和複製功能提高效率

## 參考資源

- 官方文件：`nano --help`
- 內建說明：在 Nano 中按 `Ctrl + G`
- 線上文件：GNU Nano 官方網站