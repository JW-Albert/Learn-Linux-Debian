# Linux 作業系統簡介

## 什麼是 Linux

Linux 是一個開源的類 Unix 作業系統核心（Kernel），由 Linus Torvalds 於 1991 年首次發布。Linux 核心本身只是一個核心程式，但通常我們所說的 "Linux" 指的是包含核心、系統工具、函式庫和應用程式的完整作業系統，更準確的名稱是 GNU/Linux。

### Linux 的特點

1. **開源免費**：Linux 採用 GPL 授權，可以自由使用、修改和分發
2. **多使用者多工**：支援多個使用者同時使用，可以執行多個程式
3. **穩定性高**：適合長時間運行的伺服器環境
4. **安全性強**：權限管理機制完善，安全性較高
5. **可移植性**：可以在多種硬體平台上運行
6. **社群支援**：擁有龐大的開源社群和豐富的文件資源

## Linux 發行版的主流分支

Linux 發行版（Distribution）是將 Linux 核心、系統工具、應用程式和套件管理系統打包在一起的完整作業系統。雖然有數百個 Linux 發行版，但主要可以分為兩大分支：

### Debian 分支

Debian 分支以 Debian 為基礎，使用 `.deb` 套件格式和 APT 套件管理系統。

**主要發行版：**

1. **Debian**
   - 穩定性優先的發行版
   - 採用嚴格的測試流程
   - 提供三個版本：Stable（穩定版）、Testing（測試版）、Unstable（不穩定版）
   - 適合伺服器和進階使用者

2. **Ubuntu**
   - 基於 Debian 的桌面導向發行版
   - 每六個月發布一個版本，每兩年發布一個 LTS（長期支援）版本
   - 使用者友善，適合初學者
   - 擁有龐大的社群和豐富的文件

3. **Linux Mint**
   - 基於 Ubuntu 的發行版
   - 提供類似 Windows 的使用者介面
   - 預裝多媒體編解碼器
   - 適合從 Windows 轉換的使用者

4. **其他 Debian 分支**
   - Raspbian（現為 Raspberry Pi OS）：樹莓派專用
   - Kali Linux：滲透測試和安全審計
   - Elementary OS：注重美觀的桌面環境

**特點：**
- 套件管理：APT（apt, apt-get, apt-cache）
- 套件格式：`.deb`
- 套件資料庫：`/var/lib/dpkg/`
- 設定檔：`/etc/apt/sources.list`

### Red Hat 分支

Red Hat 分支以 Red Hat Enterprise Linux (RHEL) 為基礎，使用 `.rpm` 套件格式和 YUM/DNF 套件管理系統。

**主要發行版：**

1. **Red Hat Enterprise Linux (RHEL)**
   - 商業發行版，需要訂閱授權
   - 企業級支援和長期維護
   - 穩定性極高，適合企業環境
   - 每三年發布一個主要版本

2. **CentOS**
   - 基於 RHEL 的免費版本（CentOS 8 後改為 CentOS Stream）
   - 與 RHEL 二進位相容
   - 適合需要 RHEL 相容性但不需要商業支援的環境

3. **Fedora**
   - Red Hat 的社群發行版
   - 採用最新技術和軟體版本
   - 每六個月發布一個版本
   - 適合開發者和技術愛好者

4. **Rocky Linux / AlmaLinux**
   - CentOS 的替代品
   - 提供與 RHEL 相容的免費版本
   - 適合需要長期穩定支援的環境

5. **openSUSE**
   - 獨立的發行版，但使用 RPM 套件格式
   - 提供 Tumbleweed（滾動更新）和 Leap（穩定版）
   - 擁有優秀的系統管理工具（YaST）

**特點：**
- 套件管理：YUM（舊版）或 DNF（新版）
- 套件格式：`.rpm`
- 套件資料庫：`/var/lib/rpm/`
- 設定檔：`/etc/yum.repos.d/`

### 其他重要分支

1. **Arch Linux**
   - 滾動更新發行版
   - 採用 KISS（Keep It Simple, Stupid）原則
   - 適合進階使用者和學習者
   - 套件管理：Pacman

2. **Gentoo**
   - 從原始碼編譯的發行版
   - 高度可自訂
   - 適合追求效能和自訂性的使用者
   - 套件管理：Portage

3. **Slackware**
   - 最古老的 Linux 發行版之一
   - 簡潔的設計理念
   - 適合學習 Linux 內部運作

## Linux 系統架構分層

Linux 系統採用分層架構，從底層到上層可以分為以下幾個層次：

### 1. 硬體層 (Hardware Layer)

最底層是實體硬體，包括：
- CPU（處理器）
- 記憶體（RAM）
- 儲存裝置（硬碟、SSD）
- 網路介面卡
- 其他周邊設備

### 2. 核心層 (Kernel Layer)

Linux 核心是系統的核心部分，負責：

**主要功能：**
- **程序管理**：建立、排程、終止程序
- **記憶體管理**：分配和管理系統記憶體
- **檔案系統管理**：管理檔案和目錄的存取
- **裝置驅動程式**：與硬體裝置通訊
- **網路管理**：處理網路通訊協定
- **系統呼叫介面**：提供應用程式與核心的介面

**核心位置：**
- 核心檔案通常位於 `/boot/vmlinuz-*`
- 核心模組位於 `/lib/modules/`
- 核心版本資訊：`uname -a`

**查看核心資訊：**
```bash
# 查看核心版本
uname -r

# 查看完整系統資訊
uname -a

# 查看核心模組
lsmod

# 載入核心模組
sudo modprobe <模組名稱>
```

### 3. 系統呼叫介面 (System Call Interface)

系統呼叫是應用程式與核心通訊的介面，允許使用者空間的程式請求核心服務。

**常見系統呼叫：**
- `open()`：開啟檔案
- `read()`：讀取資料
- `write()`：寫入資料
- `fork()`：建立新程序
- `exec()`：執行程式

### 4. 函式庫層 (Library Layer)

函式庫提供可重複使用的程式碼，讓應用程式可以呼叫預先實作的功能。

**主要函式庫：**

1. **GNU C Library (glibc)**
   - 標準 C 函式庫
   - 提供系統呼叫的包裝函式
   - 位置：`/lib/x86_64-linux-gnu/libc.so.*`

2. **動態連結函式庫**
   - 共享函式庫（`.so` 檔案）
   - 多個程式可以共享同一份函式庫
   - 位置：`/lib/`, `/usr/lib/`, `/usr/local/lib/`

3. **靜態函式庫**
   - 編譯時連結到程式中
   - 位置：`/usr/lib/*.a`

**查看函式庫：**
```bash
# 查看程式使用的函式庫
ldd /bin/ls

# 查看系統函式庫
ls /lib/
ls /usr/lib/

# 查看函式庫搜尋路徑
echo $LD_LIBRARY_PATH
cat /etc/ld.so.conf
```

### 5. Shell 層 (Shell Layer)

Shell 是使用者與系統互動的命令列介面。

**主要功能：**
- 解析使用者輸入的指令
- 執行程式和腳本
- 管理環境變數
- 提供管道和重導向功能

**常見 Shell：**
- Bash（最常用）
- Zsh
- Fish
- Sh

**Shell 位置：**
- Shell 程式：`/bin/bash`, `/bin/sh`, `/bin/zsh`
- Shell 設定檔：`~/.bashrc`, `~/.zshrc`

### 6. 系統工具層 (System Utilities)

系統工具是執行系統管理任務的程式。

**主要工具分類：**

1. **檔案操作工具**
   - `ls`, `cp`, `mv`, `rm`, `mkdir`, `find`, `grep`

2. **程序管理工具**
   - `ps`, `top`, `kill`, `jobs`, `bg`, `fg`

3. **網路工具**
   - `ping`, `ifconfig`, `netstat`, `ssh`, `curl`, `wget`

4. **系統資訊工具**
   - `uname`, `df`, `du`, `free`, `uptime`

5. **權限管理工具**
   - `chmod`, `chown`, `sudo`, `su`

**工具位置：**
- 系統工具：`/bin/`, `/sbin/`
- 使用者工具：`/usr/bin/`, `/usr/sbin/`
- 本地工具：`/usr/local/bin/`

### 7. 應用程式層 (Application Layer)

應用程式是使用者直接使用的軟體。

**應用程式類型：**
- 文字編輯器：`vim`, `nano`, `gedit`
- 網頁瀏覽器：`firefox`, `chromium`
- 辦公室軟體：`libreoffice`
- 開發工具：`gcc`, `python`, `git`
- 伺服器軟體：`apache`, `nginx`, `mysql`

**應用程式位置：**
- `/usr/bin/`：一般應用程式
- `/usr/local/bin/`：本地安裝的應用程式
- `/opt/`：第三方軟體
- `~/.local/bin/`：使用者個人的應用程式

## 系統目錄結構

Linux 遵循檔案系統階層標準（FHS, Filesystem Hierarchy Standard）：

```
/                   根目錄
├── bin/            基本指令（所有使用者）
├── boot/           開機相關檔案
├── dev/            裝置檔案
├── etc/            系統設定檔
├── home/           使用者家目錄
├── lib/            系統函式庫
├── media/          可移除媒體掛載點
├── mnt/            臨時掛載點
├── opt/            第三方軟體
├── proc/           程序資訊（虛擬檔案系統）
├── root/           root 使用者家目錄
├── run/            執行時資料
├── sbin/           系統管理指令
├── srv/            服務資料
├── sys/            系統資訊（虛擬檔案系統）
├── tmp/            臨時檔案
├── usr/            使用者程式和資料
│   ├── bin/        應用程式
│   ├── lib/        函式庫
│   ├── local/      本地安裝的軟體
│   └── src/        原始碼
└── var/            變動資料
    ├── log/        日誌檔案
    ├── cache/      快取
    └── lib/        應用程式資料
```

## 系統啟動流程

了解系統啟動流程有助於理解各層級的關係：

1. **BIOS/UEFI**：硬體初始化
2. **Boot Loader**（GRUB）：載入核心
3. **Kernel**：初始化硬體，載入驅動程式
4. **Init 程序**（systemd/sysvinit）：第一個使用者空間程序
5. **系統服務**：啟動各種系統服務
6. **登入管理員**：提供登入介面
7. **Shell/桌面環境**：使用者介面

## 查看系統資訊

```bash
# 查看系統架構
uname -m

# 查看核心版本
uname -r

# 查看發行版資訊
cat /etc/os-release
lsb_release -a

# 查看系統資源
free -h          # 記憶體使用
df -h            # 磁碟使用
top              # 程序和資源監控

# 查看已安裝的套件
dpkg -l          # Debian 系列
rpm -qa          # Red Hat 系列
```

## 總結

Linux 系統採用分層架構設計，從底層的硬體和核心，到上層的應用程式，每一層都有其特定的職責。理解這些層級和它們之間的關係，有助於：

1. **系統管理**：知道問題可能出現在哪一層
2. **效能調校**：針對特定層級進行優化
3. **安全設定**：在各層級實施適當的安全措施
4. **故障排除**：系統化地診斷和解決問題

無論是 Debian 分支還是 Red Hat 分支，都遵循相同的系統架構原則，只是在套件管理和某些工具上有所不同。