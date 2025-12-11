# Learn-Linux-Debian

這是一個關於 Linux 的學習資料庫，著重於 Debian 派系（Debian、Ubuntu 等）的系統管理和操作教學。

## 簡介

本學習資料庫提供完整的 Linux 系統管理教學，從基礎概念到進階應用，涵蓋：

- **系統基礎**：Linux 架構、發行版介紹
- **套件管理**：APT 套件管理工具
- **權限管理**：使用者、群組、檔案權限控制
- **Shell 操作**：命令列操作、腳本撰寫、路徑概念
- **文字編輯器**：Nano、Vim 使用教學
- **系統管理**：資源監控、程序管理、服務管理
- **版本控制**：Git 完整教學
- **網路與安全**：SSH、防火牆（UFW）、網路設定

## 學習路徑

### 初學者路徑

1. **[Linux 簡介](00.%20Linux/README.md)** - 了解 Linux 基本概念和系統架構
2. **[APT 套件管理](01.%20APT/README.md)** - 學習安裝和管理軟體套件
3. **[Shell 介紹](04.%20Shell/README.md)** - 認識命令列介面
4. **[常用指令](04.%20Shell/Command/README.md)** - 掌握基本 Linux 指令
5. **[文字編輯器](05.%20Editer/README.md)** - 學習使用 Nano 或 Vim
6. **[路徑概念](04.%20Shell/Route/README.md)** - 理解檔案系統路徑

### 進階路徑

7. **[權限管理](02.%20Permission/README.md)** - 深入理解檔案權限和使用者管理
8. **[使用者與群組](03.%20User/README.md)** - 管理系統使用者帳號
9. **[Shell 腳本](04.%20Shell/Scrip/README.md)** - 撰寫自動化腳本
10. **[資源管理](06.%20Resource/README.md)** - 監控和管理系統資源
11. **[Git 版本控制](07.%20GIT/README.md)** - 學習程式碼版本管理
12. **[SSH 遠端連線](08.%20SSH/README.md)** - 遠端管理伺服器
13. **[防火牆設定](09.%20UFW/README.md)** - 保護系統安全
14. **[網路設定](10.%20Network/README.md)** - 配置網路連線

## 目錄導航

### 基礎概念

- **[00. Linux](00.%20Linux/README.md)**
  - Linux 簡介
  - 主流發行版分支
  - 系統架構分層

### 套件管理

- **[01. APT](01.%20APT/README.md)**
  - 套件安裝與更新
  - 套件搜尋與管理
  - 套件來源設定

### 權限與使用者

- **[02. Permission](02.%20Permission/README.md)**
  - Root 與 Sudo
  - 檔案權限設定
  - 特殊權限（SUID、SGID、Sticky Bit）

- **[03. User](03.%20User/README.md)**
  - 使用者帳號管理
  - 群組管理
  - 系統檔案說明

### Shell 與命令列

- **[04. Shell](04.%20Shell/README.md)**
  - Shell 介紹與種類
  - Shell 設定檔
  - 基本操作技巧

- **[04. Shell/Command](04.%20Shell/Command/README.md)**
  - 檔案與目錄操作
  - 檔案內容查看
  - 系統資訊指令
  - 文字處理工具

- **[04. Shell/Route](04.%20Shell/Route/README.md)**
  - 絕對路徑與相對路徑
  - 路徑符號（~、.、..）
  - 路徑操作指令

- **[04. Shell/Scrip](04.%20Shell/Scrip/README.md)**
  - 變數與資料類型
  - 條件判斷與迴圈
  - 函式與錯誤處理

### 文字編輯器

- **[05. Editer](05.%20Editer/README.md)**
  - 編輯器選擇建議
  - 基本操作概念

- **[05. Editer/Nano](05.%20Editer/Nano/README.md)**
  - Nano 完整教學
  - 快捷鍵參考

- **[05. Editer/Vim](05.%20Editer/Vim/README.md)**
  - Vim 模式介紹
  - 進階編輯技巧
  - 設定檔配置

### 系統管理

- **[06. Resource](06.%20Resource/README.md)**
  - CPU、記憶體、磁碟監控
  - 程序管理
  - 系統服務管理
  - 工作排程（Cron）

### 版本控制

- **[07. GIT](07.%20GIT/README.md)**
  - Git 基本概念
  - 學習路徑指引

- **[07. GIT/basic.md](07.%20GIT/basic.md)**
  - 基本操作（init、add、commit）
  - 查看歷史與變更
  - 撤銷操作

- **[07. GIT/remote.md](07.%20GIT/remote.md)**
  - 遠端倉庫操作
  - push、pull、fetch
  - 遠端管理

- **[07. GIT/branch.md](07.%20GIT/branch.md)**
  - 分支建立與切換
  - 合併與解決衝突
  - 分支管理策略

- **[07. GIT/advanced.md](07.%20GIT/advanced.md)**
  - 暫存變更（stash）
  - 標籤管理
  - 重置與還原
  - 進階功能

### 網路與安全

- **[08. SSH](08.%20SSH/README.md)**
  - SSH 連線設定
  - 金鑰認證
  - 埠轉發
  - 檔案傳輸（SCP、SFTP）

- **[09. UFW](09.%20UFW/README.md)**
  - 防火牆基本操作
  - 規則設定
  - ICMP 封包控制
  - 應用程式配置

- **[10. Network](10.%20Network/README.md)**
  - 網路基本概念
  - IP 位址設定
  - 路由管理
  - DNS 配置
  - 網路工具使用

## 快速參考

### 常用指令

```bash
# 套件管理
sudo apt update && sudo apt upgrade
sudo apt install <套件名稱>
sudo apt remove <套件名稱>

# 檔案權限
chmod 755 file.txt
chown user:group file.txt

# 使用者管理
sudo useradd -m username
sudo passwd username

# 系統資訊
uname -a
df -h
free -h
ps aux

# 網路
ip addr show
ping google.com
ss -tulpn
```

### 重要檔案位置

- 套件來源：`/etc/apt/sources.list`
- 使用者資訊：`/etc/passwd`
- 群組資訊：`/etc/group`
- 網路設定：`/etc/netplan/` 或 `/etc/network/interfaces`
- DNS 設定：`/etc/resolv.conf`
- SSH 設定：`~/.ssh/config`、`/etc/ssh/sshd_config`
- UFW 設定：`/etc/ufw/`

## 學習建議

1. **循序漸進**：按照學習路徑順序學習，先掌握基礎再學習進階內容
2. **實際操作**：在 Linux 系統上實際練習每個指令和操作
3. **查閱手冊**：使用 `man` 指令查看詳細說明文件
4. **記錄筆記**：記錄常用指令和設定，建立個人知識庫
5. **解決問題**：遇到問題時，先查看相關章節，再查閱官方文件

## 適用對象

- Linux 初學者
- 系統管理員
- 開發者
- 想要學習 Debian 系列系統的使用者

## 授權

本專案採用開源授權，詳見 [LICENSE](LICENSE) 檔案。

## 貢獻

歡迎提出建議和改進，讓這份學習資料更加完善。

---

**開始學習**：建議從 [Linux 簡介](00.%20Linux/README.md) 開始，建立對 Linux 系統的基本認識。