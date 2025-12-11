# Advanced Package Tool (APT)

## 簡介

APT (Advanced Package Tool) 是 Debian 系列系統中所使用的軟體資源管理工具。它提供了簡單易用的命令列介面，用於安裝、更新、移除和管理軟體套件。

APT 會自動處理套件之間的相依性關係，確保安裝的軟體能夠正常運作。

## 基本概念

### 套件來源 (Repository)

APT 從預先設定的套件來源（repository）下載軟體套件。套件來源的設定檔位於 `/etc/apt/sources.list` 或 `/etc/apt/sources.list.d/` 目錄中。

### 套件索引

APT 會維護一份本地套件索引，記錄所有可用套件的資訊。在安裝或更新套件前，需要先更新這份索引。

## 常用指令

### 更新套件索引

```bash
sudo apt update
```

此指令會從套件來源下載最新的套件資訊，更新本地的套件索引。建議在安裝新套件前先執行此指令。

### 升級已安裝的套件

```bash
sudo apt upgrade
```

升級所有已安裝的套件到最新版本。此指令不會安裝新套件，也不會移除已安裝的套件。

### 完整升級

```bash
sudo apt full-upgrade
```

執行完整升級，可能會安裝或移除套件以解決相依性問題。

### 安裝套件

```bash
sudo apt install <套件名稱>
```

安裝指定的套件。可以一次安裝多個套件，用空格分隔。

範例：
```bash
sudo apt install vim git curl
```

### 移除套件

```bash
sudo apt remove <套件名稱>
```

移除指定的套件，但保留設定檔。

```bash
sudo apt purge <套件名稱>
```

完全移除套件，包括設定檔。

### 搜尋套件

```bash
apt search <關鍵字>
```

在套件名稱和描述中搜尋包含關鍵字的套件。

### 查看套件資訊

```bash
apt show <套件名稱>
```

顯示指定套件的詳細資訊，包括版本、描述、相依性等。

### 列出已安裝的套件

```bash
apt list --installed
```

列出所有已安裝的套件。

### 列出可升級的套件

```bash
apt list --upgradable
```

列出所有可以升級的套件。

### 清理套件快取

```bash
sudo apt clean
```

清除下載的套件檔案快取。

```bash
sudo apt autoclean
```

只清除不再需要的舊套件快取。

```bash
sudo apt autoremove
```

自動移除不再需要的相依套件。

## 實用範例

### 完整的系統更新流程

```bash
# 1. 更新套件索引
sudo apt update

# 2. 升級所有套件
sudo apt upgrade

# 3. 清理不需要的套件
sudo apt autoremove
```

### 安裝並移除套件

```bash
# 安裝套件
sudo apt install nginx

# 查看套件資訊
apt show nginx

# 移除套件（保留設定檔）
sudo apt remove nginx

# 完全移除套件（包括設定檔）
sudo apt purge nginx
```

### 搜尋並安裝套件

```bash
# 搜尋包含 "editor" 的套件
apt search editor

# 安裝找到的套件
sudo apt install vim
```

## 注意事項

1. 大部分 APT 指令需要 root 權限，因此需要使用 `sudo`。
2. 執行 `apt update` 後再執行 `apt upgrade` 是良好的習慣。
3. 使用 `apt full-upgrade` 時要特別小心，可能會移除某些套件。
4. 定期執行 `apt autoremove` 可以保持系統整潔。

## 相關檔案位置

- 套件來源設定：`/etc/apt/sources.list`
- 套件來源設定目錄：`/etc/apt/sources.list.d/`
- 套件快取目錄：`/var/cache/apt/`
- 套件索引：`/var/lib/apt/lists/`