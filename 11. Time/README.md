da# Linux 時間管理

## 簡介

Linux 系統的時間管理是系統運作的重要基礎，正確的時間設定對於日誌記錄、檔案時間戳記、網路通訊協定（如 HTTPS）等都至關重要。本教學將介紹如何設定和管理 Linux 系統時間。

## 時間概念

### 系統時間類型

Linux 系統中有兩種時間：

1. **硬體時間（Hardware Clock / RTC）**
   - 儲存在主機板上的時鐘晶片中
   - 即使關機也會繼續運作
   - 也稱為 RTC（Real-Time Clock）

2. **系統時間（System Clock）**
   - 由 Linux 核心維護的時間
   - 開機時從硬體時間讀取
   - 系統運作時使用系統時間

### 時區（Time Zone）

時區決定了本地時間與 UTC（協調世界時）的偏移量。

**常見時區：**
- UTC（協調世界時）：標準參考時間
- Asia/Taipei：台灣時間（UTC+8）
- Asia/Shanghai：中國時間（UTC+8）
- America/New_York：美國東部時間（UTC-5）
- Europe/London：英國時間（UTC+0）

## 查看時間資訊

### 查看目前時間

```bash
# 查看目前系統時間
date

# 查看詳細時間資訊
date -R
date +"%Y-%m-%d %H:%M:%S"

# 查看硬體時間
sudo hwclock
sudo hwclock --show

# 查看時區資訊
timedatectl
timedatectl status

# 查看所有可用時區
timedatectl list-timezones

# 搜尋特定時區
timedatectl list-timezones | grep Asia
timedatectl list-timezones | grep Taipei
```

### 查看時區設定檔

```bash
# 查看目前時區連結
ls -l /etc/localtime

# 查看時區資料庫
ls /usr/share/zoneinfo/

# 查看時區資料庫中的亞洲時區
ls /usr/share/zoneinfo/Asia/
```

## 設定系統時間

### 使用 date 指令（臨時設定）

```bash
# 設定系統時間（格式：MMDDhhmm[[CC]YY][.ss]）
sudo date 010112002024.00  # 2024年1月1日 12:00:00

# 使用完整格式設定
sudo date -s "2024-01-01 12:00:00"

# 只設定日期
sudo date -s "2024-01-01"

# 只設定時間
sudo date -s "12:00:00"
```

**注意：** 使用 `date` 設定的時間在重啟後可能會遺失，建議使用 NTP 同步。

### 使用 timedatectl（推薦）

`timedatectl` 是 systemd 提供的時間管理工具，功能更強大。

```bash
# 查看時間狀態
timedatectl

# 設定系統時間
sudo timedatectl set-time "2024-01-01 12:00:00"

# 設定日期
sudo timedatectl set-time "2024-01-01"

# 設定時間
sudo timedatectl set-time "12:00:00"
```

## 設定時區

### 使用 timedatectl（推薦）

```bash
# 設定時區（例如：台灣時間）
sudo timedatectl set-timezone Asia/Taipei

# 設定時區（例如：中國時間）
sudo timedatectl set-timezone Asia/Shanghai

# 設定時區（例如：UTC）
sudo timedatectl set-timezone UTC

# 查看可用時區
timedatectl list-timezones
```

### 使用傳統方式

```bash
# 方法 1：建立符號連結
sudo ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime

# 方法 2：使用 tzselect（互動式選擇）
tzselect

# 方法 3：設定 TZ 環境變數（僅對目前 session 有效）
export TZ=Asia/Taipei
```

### 設定時區後驗證

```bash
# 查看時區是否正確設定
timedatectl

# 查看時間（應該顯示正確的本地時間）
date

# 查看 UTC 時間
date -u
```

## 硬體時間管理

### 查看硬體時間

```bash
# 查看硬體時間
sudo hwclock

# 查看硬體時間（詳細格式）
sudo hwclock --show

# 以 ISO 格式顯示
sudo hwclock --iso-8601
```

### 同步硬體時間與系統時間

```bash
# 將系統時間寫入硬體時間
sudo hwclock --systohc
# 或
sudo hwclock -w

# 將硬體時間讀取到系統時間
sudo hwclock --hctosys
# 或
sudo hwclock -s

# 使用 UTC 時間（推薦）
sudo hwclock --utc --systohc

# 使用本地時間
sudo hwclock --localtime --systohc
```

**建議：** 使用 UTC 時間儲存硬體時間，系統會根據時區自動轉換為本地時間。

## NTP 時間同步

NTP（Network Time Protocol）用於透過網路同步系統時間，確保時間準確。

### 使用 systemd-timesyncd（預設）

現代 Linux 系統通常使用 systemd-timesyncd 進行時間同步。

```bash
# 查看時間同步狀態
timedatectl status

# 啟用 NTP 同步
sudo timedatectl set-ntp true

# 停用 NTP 同步
sudo timedatectl set-ntp false

# 查看時間同步詳細資訊
timedatectl timesync-status

# 查看 NTP 伺服器
timedatectl show-timesync
```

### 使用 chrony（進階）

chrony 是更強大的 NTP 客戶端，適合需要更精確時間同步的環境。

#### 安裝 chrony

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install chrony
```

#### 配置 chrony

```bash
# 編輯設定檔
sudo nano /etc/chrony/chrony.conf
```

**設定範例：**
```
# 使用本地時間伺服器
pool 0.debian.pool.ntp.org iburst
pool 1.debian.pool.ntp.org iburst
pool 2.debian.pool.ntp.org iburst
pool 3.debian.pool.ntp.org iburst

# 或使用特定 NTP 伺服器
server time.google.com iburst
server time.cloudflare.com iburst

# 允許本地網路同步
allow 192.168.1.0/24
```

#### 管理 chrony 服務

```bash
# 啟動 chrony
sudo systemctl start chrony

# 啟用開機自動啟動
sudo systemctl enable chrony

# 查看服務狀態
sudo systemctl status chrony

# 重新載入設定
sudo systemctl reload chrony
```

#### chrony 常用指令

```bash
# 查看時間同步狀態
chronyc tracking

# 查看 NTP 伺服器狀態
chronyc sources

# 查看詳細統計
chronyc sourcestats

# 強制立即同步
sudo chronyd -q

# 手動設定時間伺服器
chronyc add server time.google.com
```

### 使用 ntpd（傳統方式）

```bash
# 安裝 ntpd
sudo apt install ntp

# 編輯設定檔
sudo nano /etc/ntp.conf

# 啟動服務
sudo systemctl start ntp
sudo systemctl enable ntp

# 查看狀態
ntpq -p
```

## 時間格式

### date 指令格式

```bash
# 顯示完整日期時間
date +"%Y-%m-%d %H:%M:%S"
# 輸出：2024-01-01 12:00:00

# 顯示日期
date +"%Y-%m-%d"
# 輸出：2024-01-01

# 顯示時間
date +"%H:%M:%S"
# 輸出：12:00:00

# 顯示星期
date +"%A"
# 輸出：Monday

# 顯示時間戳記（Unix 時間）
date +%s
# 輸出：1704067200

# 從時間戳記轉換
date -d @1704067200
```

**常用格式符號：**
- `%Y`：四位數年份
- `%m`：月份（01-12）
- `%d`：日期（01-31）
- `%H`：小時（00-23）
- `%M`：分鐘（00-59）
- `%S`：秒數（00-59）
- `%A`：完整星期名稱
- `%B`：完整月份名稱

### 時間戳記轉換

```bash
# 取得目前時間戳記
date +%s

# 從時間戳記轉換為日期時間
date -d @1704067200

# 從日期時間轉換為時間戳記
date -d "2024-01-01 12:00:00" +%s

# 計算時間差
date -d "2024-01-01" +%s
date -d "2024-01-02" +%s
```

## 時間相關設定檔

### /etc/localtime

系統時區設定檔（符號連結）。

```bash
# 查看目前時區
ls -l /etc/localtime

# 手動設定時區
sudo ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```

### /etc/timezone

Debian/Ubuntu 系統的時區設定檔。

```bash
# 查看時區
cat /etc/timezone

# 設定時區
echo "Asia/Taipei" | sudo tee /etc/timezone
```

### /etc/chrony/chrony.conf

chrony 的設定檔。

### /etc/ntp.conf

ntpd 的設定檔。

## 實用範例

### 檢查時間同步狀態

```bash
#!/bin/bash
echo "=== 系統時間 ==="
date

echo "=== 時區資訊 ==="
timedatectl

echo "=== 硬體時間 ==="
sudo hwclock

echo "=== NTP 同步狀態 ==="
timedatectl timesync-status
```

### 設定台灣時區並啟用 NTP

```bash
# 1. 設定時區
sudo timedatectl set-timezone Asia/Taipei

# 2. 啟用 NTP 同步
sudo timedatectl set-ntp true

# 3. 驗證設定
timedatectl status
```

### 手動同步時間

```bash
# 使用 chrony 強制同步
sudo chronyd -q

# 或使用 ntpdate（如果已安裝）
sudo ntpdate -s time.google.com
```

### 在腳本中使用時間

```bash
#!/bin/bash
# 取得目前時間戳記
TIMESTAMP=$(date +%s)

# 格式化時間
FORMATTED_TIME=$(date +"%Y-%m-%d %H:%M:%S")

# 建立以時間命名的檔案
BACKUP_FILE="backup_$(date +%Y%m%d_%H%M%S).tar.gz"
echo "建立備份：$BACKUP_FILE"
```

## 常見問題

### 問題：時間不準確

**解決方法：**
```bash
# 1. 啟用 NTP 同步
sudo timedatectl set-ntp true

# 2. 檢查 NTP 服務狀態
systemctl status systemd-timesyncd

# 3. 手動同步（使用 chrony）
sudo chronyd -q
```

### 問題：時區設定錯誤

**解決方法：**
```bash
# 1. 查看目前時區
timedatectl

# 2. 設定正確時區
sudo timedatectl set-timezone Asia/Taipei

# 3. 驗證設定
date
```

### 問題：硬體時間與系統時間不同步

**解決方法：**
```bash
# 將系統時間寫入硬體時間
sudo hwclock --systohc

# 或使用 UTC 時間
sudo hwclock --utc --systohc
```

### 問題：NTP 同步失敗

**解決方法：**
```bash
# 1. 檢查網路連線
ping -c 4 time.google.com

# 2. 檢查防火牆設定
sudo ufw status

# 3. 檢查 NTP 服務
sudo systemctl status systemd-timesyncd

# 4. 使用不同的 NTP 伺服器
sudo nano /etc/systemd/timesyncd.conf
```

## 最佳實踐

1. **使用 UTC 時間儲存硬體時間**：避免時區轉換問題
2. **啟用 NTP 同步**：確保時間準確
3. **選擇可靠的 NTP 伺服器**：使用多個 NTP 伺服器提高可靠性
4. **定期檢查時間**：確保系統時間準確
5. **在腳本中使用時間戳記**：方便時間計算和比較
6. **記錄時間設定變更**：便於問題排查

## 常用指令速查

```bash
# 查看時間
date
timedatectl

# 設定時區
sudo timedatectl set-timezone Asia/Taipei

# 啟用 NTP
sudo timedatectl set-ntp true

# 硬體時間
sudo hwclock
sudo hwclock --systohc

# 時間格式
date +"%Y-%m-%d %H:%M:%S"
date +%s

# chrony
chronyc tracking
chronyc sources
```

## 相關檔案位置

- 時區設定：`/etc/localtime`、`/etc/timezone`
- 時區資料庫：`/usr/share/zoneinfo/`
- chrony 設定：`/etc/chrony/chrony.conf`
- ntpd 設定：`/etc/ntp.conf`
- systemd-timesyncd 設定：`/etc/systemd/timesyncd.conf`

## 參考資源

- timedatectl 手冊：`man timedatectl`
- date 手冊：`man date`
- hwclock 手冊：`man hwclock`
- chrony 文件：https://chrony.tuxfamily.org/