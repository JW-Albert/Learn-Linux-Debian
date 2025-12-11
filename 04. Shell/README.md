# Shell 介紹與使用

## 簡介

Shell 是使用者與作業系統核心（Kernel）之間的介面程式，它接收使用者的指令並執行相應的操作。Shell 既是命令列直譯器，也是強大的程式設計語言，可以用來撰寫腳本自動化系統管理工作。

## Shell 的作用

### 命令列介面

Shell 提供文字介面，讓使用者可以透過鍵盤輸入指令來操作系統。

### 指令直譯與執行

Shell 會解析使用者輸入的指令，執行相應的程式，並將結果回傳給使用者。

### 腳本執行

Shell 可以執行預先撰寫的腳本檔案，實現自動化操作。

### 環境管理

Shell 管理環境變數、工作目錄、指令路徑等執行環境。

## 查看目前的 Shell

```bash
# 查看目前使用的 Shell
echo $SHELL

# 查看 Shell 的版本
$SHELL --version

# 查看所有可用的 Shell
cat /etc/shells

# 查看目前 Shell 的程序名稱
ps -p $$

# 查看所有登入的 Shell
ps aux | grep shell
```

## 常見的 Shell 種類

### Bash (Bourne Again Shell)

Bash 是目前 Linux 系統中最廣泛使用的 Shell，是 Bourne Shell (sh) 的增強版本。

**特點：**
- 功能豐富，支援命令列編輯、命令歷史、自動完成
- 相容於 sh，可以執行大部分 sh 腳本
- 內建許多實用功能（算術運算、陣列、正則表達式等）
- 預設安裝在大多數 Linux 發行版

**版本資訊：**
```bash
bash --version
```

**設定檔：**
- `~/.bashrc`：互動式非登入 Shell 的設定檔
- `~/.bash_profile`：登入 Shell 的設定檔
- `~/.bash_logout`：登出時執行的腳本
- `/etc/bash.bashrc`：系統全域設定檔

### Sh (Bourne Shell)

Sh 是最早的 Unix Shell，是許多現代 Shell 的基礎。

**特點：**
- 輕量級，執行速度快
- 標準化程度高，符合 POSIX 標準
- 適合撰寫可移植性高的腳本

**注意：** 在許多系統中，`/bin/sh` 實際上是連結到其他 Shell（如 bash 或 dash）。

### Zsh (Z Shell)

Zsh 是一個功能強大的 Shell，具有許多進階特性。

**特點：**
- 強大的自動完成功能
- 豐富的主題和插件系統
- 更好的命令列編輯體驗
- 智慧型歷史搜尋
- 支援右側提示（RPROMPT）

**安裝：**
```bash
sudo apt install zsh
```

**設定檔：**
- `~/.zshrc`：主要設定檔
- `~/.zprofile`：登入時執行的設定檔

**熱門框架：**
- Oh My Zsh：Zsh 的配置框架，提供豐富的主題和插件
- Prezto：另一個 Zsh 配置框架

### Fish (Friendly Interactive Shell)

Fish 是一個使用者友善的 Shell，專注於易用性和互動體驗。

**特點：**
- 自動語法高亮
- 智慧型自動完成
- 基於網頁的設定介面
- 更直觀的語法
- 內建幫助系統

**安裝：**
```bash
sudo apt install fish
```

**設定檔：**
- `~/.config/fish/config.fish`：主要設定檔

**注意：** Fish 的語法與 bash/sh 不完全相容，不適合執行傳統 Shell 腳本。

### Dash (Debian Almquist Shell)

Dash 是一個輕量級的 POSIX 相容 Shell，在 Debian 系統中常用作 `/bin/sh` 的實作。

**特點：**
- 執行速度快
- 記憶體佔用小
- 符合 POSIX 標準
- 適合系統啟動腳本

**安裝：**
```bash
sudo apt install dash
```

### Csh / Tcsh (C Shell / TENEX C Shell)

Csh 和 Tcsh 是基於 C 語言語法的 Shell。

**特點：**
- 語法類似 C 語言
- Tcsh 是 Csh 的增強版本
- 提供命令列編輯功能（Tcsh）

**安裝：**
```bash
sudo apt install tcsh
```

**設定檔：**
- `~/.cshrc` 或 `~/.tcshrc`：主要設定檔

**注意：** 現代系統較少使用，主要用於特定環境或歷史相容性。

### Ksh (Korn Shell)

Ksh 是由 David Korn 開發的 Shell，結合了 sh 和 csh 的優點。

**特點：**
- 相容於 sh
- 支援陣列和關聯陣列
- 強大的命令列編輯功能
- 符合 POSIX 標準

**安裝：**
```bash
sudo apt install ksh
```

**設定檔：**
- `~/.kshrc`：主要設定檔

## 切換 Shell

### 臨時切換

```bash
# 直接執行其他 Shell
/bin/zsh
/bin/fish

# 執行後輸入 exit 回到原來的 Shell
exit
```

### 永久變更預設 Shell

```bash
# 查看可用的 Shell
cat /etc/shells

# 變更預設 Shell（需要該 Shell 的路徑在 /etc/shells 中）
chsh -s /bin/zsh

# 變更其他使用者的 Shell（需要 root 權限）
sudo chsh -s /bin/zsh <使用者名稱>

# 變更後需要重新登入才會生效
```

### 在腳本中指定 Shell

在腳本檔案的第一行使用 shebang 指定使用的 Shell：

```bash
#!/bin/bash
# 使用 bash 執行此腳本

#!/bin/sh
# 使用 sh 執行此腳本

#!/usr/bin/zsh
# 使用 zsh 執行此腳本
```

## Shell 設定檔

### 設定檔的載入順序

不同的 Shell 在不同情況下會載入不同的設定檔：

#### Bash

1. **登入 Shell**（透過 SSH 或 tty 登入）：
   - `/etc/profile`（系統全域）
   - `~/.bash_profile` 或 `~/.profile` 或 `~/.bash_login`（依序載入第一個存在的）
   - `~/.bashrc`（如果 `~/.bash_profile` 中有呼叫）

2. **非登入互動式 Shell**（開啟終端機視窗）：
   - `~/.bashrc`
   - `/etc/bash.bashrc`（系統全域）

3. **非互動式 Shell**（執行腳本）：
   - 通常不載入設定檔，除非明確指定

#### Zsh

1. **登入 Shell**：
   - `/etc/zsh/zshenv`
   - `/etc/zsh/zprofile`
   - `~/.zprofile`
   - `/etc/zsh/zshrc`
   - `~/.zshrc`
   - `/etc/zsh/zlogin`
   - `~/.zlogin`

2. **非登入互動式 Shell**：
   - `/etc/zsh/zshenv`
   - `~/.zshenv`
   - `/etc/zsh/zshrc`
   - `~/.zshrc`

### 常用設定檔內容範例

#### ~/.bashrc 範例

```bash
# 啟用顏色支援
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced

# 設定別名
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'

# 設定提示字元
PS1='\u@\h:\w\$ '

# 設定歷史記錄
HISTSIZE=1000
HISTFILESIZE=2000
HISTCONTROL=ignoreboth

# 新增自訂路徑
export PATH="$HOME/bin:$PATH"
```

#### ~/.zshrc 範例

```bash
# 啟用 Oh My Zsh（如果已安裝）
# export ZSH="$HOME/.oh-my-zsh"
# source $ZSH/oh-my-zsh.sh

# 設定提示字元
PROMPT='%n@%m:%~$ '

# 設定歷史記錄
HISTSIZE=1000
SAVEHIST=2000
HISTFILE=~/.zsh_history

# 啟用自動完成
autoload -Uz compinit
compinit
```

## Shell 基本操作

### 命令列編輯

大多數現代 Shell 支援命令列編輯：

```bash
# 游標移動
Ctrl + A  # 移到行首
Ctrl + E  # 移到行尾
Ctrl + B  # 向左移動一個字元
Ctrl + F  # 向右移動一個字元

# 刪除
Ctrl + D  # 刪除游標下的字元
Ctrl + H  # 刪除游標前的字元（Backspace）
Ctrl + U  # 刪除到行首
Ctrl + K  # 刪除到行尾

# 歷史記錄
Ctrl + P  # 上一條指令（或 ↑）
Ctrl + N  # 下一條指令（或 ↓）
Ctrl + R  # 反向搜尋歷史記錄

# 其他
Ctrl + L  # 清空螢幕
Ctrl + C  # 取消目前指令
Ctrl + Z  # 暫停目前程序
```

### 自動完成

大多數 Shell 支援 Tab 鍵自動完成：

```bash
# 按一次 Tab：自動完成
# 按兩次 Tab：顯示所有可能的選項
```

### 指令別名 (Alias)

```bash
# 建立別名
alias ll='ls -alF'
alias grep='grep --color=auto'

# 查看所有別名
alias

# 移除別名
unalias ll

# 永久設定：將別名加入 ~/.bashrc 或對應的設定檔
```

### 環境變數

```bash
# 查看環境變數
env
printenv

# 設定環境變數
export MY_VAR="value"

# 查看特定環境變數
echo $PATH
echo $HOME

# 在設定檔中設定環境變數
export PATH="$HOME/bin:$PATH"
```

## Shell 腳本基礎

### 建立簡單腳本

```bash
#!/bin/bash
# 這是註解

echo "Hello, World!"
echo "目前目錄：$(pwd)"
echo "目前使用者：$(whoami)"
```

### 執行腳本

```bash
# 方法 1：使用 bash 執行
bash script.sh

# 方法 2：給予執行權限後直接執行
chmod +x script.sh
./script.sh

# 方法 3：使用絕對路徑
/path/to/script.sh
```

## 選擇適合的 Shell

### 一般使用者

- **Bash**：預設選擇，功能完整，相容性好
- **Zsh**：想要更強大的功能和自訂性
- **Fish**：重視易用性和互動體驗

### 系統管理員

- **Bash**：標準選擇，適合撰寫可移植腳本
- **Sh/Dash**：系統啟動腳本，需要快速執行

### 開發者

- **Zsh**：配合 Oh My Zsh 等框架，提供豐富的開發工具
- **Bash**：撰寫跨平台腳本

## 實用技巧

### 檢查腳本相容性

```bash
# 使用 sh 檢查腳本語法
sh -n script.sh

# 使用 bash 檢查語法
bash -n script.sh
```

### 除錯模式

```bash
# 執行時顯示每條指令
bash -x script.sh

# 在腳本中啟用除錯
set -x  # 啟用
set +x  # 停用
```

### 測試不同 Shell

```bash
# 在腳本中測試多個 Shell
for shell in bash sh zsh; do
    echo "Testing with $shell:"
    $shell script.sh
done
```

## 常用指令速查

```bash
# 查看目前 Shell
echo $SHELL
ps -p $$

# 查看可用 Shell
cat /etc/shells

# 切換 Shell
chsh -s /bin/zsh

# 查看 Shell 版本
bash --version
zsh --version

# 執行其他 Shell
/bin/zsh
/bin/fish

# 查看設定檔
cat ~/.bashrc
cat ~/.zshrc

# 重新載入設定檔
source ~/.bashrc
. ~/.bashrc  # 簡寫
```

## 注意事項

1. 變更預設 Shell 前，確認新 Shell 已安裝且路徑正確
2. 備份原有的設定檔，避免遺失自訂設定
3. 不同 Shell 的語法可能不同，撰寫腳本時要注意相容性
4. 系統腳本通常使用 `/bin/sh` 以確保可移植性
5. 某些 Shell（如 Fish）與傳統 Shell 語法不相容
6. 定期更新 Shell 以獲得安全修補和功能改進
7. 學習 Shell 腳本時，建議從 Bash 開始，因為它最廣泛使用且文件豐富