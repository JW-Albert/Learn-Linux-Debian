# 系統資源管理

## 簡介

Linux 系統資源管理是系統管理的重要部分，包括監控 CPU、記憶體、磁碟、網路等資源的使用情況，以及管理程序、服務和工作排程。有效的資源管理可以確保系統穩定運行，並及時發現和解決效能問題。

## 系統資源監控

### CPU 監控

#### top

即時顯示系統程序和資源使用情況。

```bash
# 啟動 top
top

# 指定更新間隔（秒）
top -d 5

# 顯示特定使用者的程序
top -u username

# 只顯示一次（不持續更新）
top -n 1
```

**top 內建指令：**
- `P`：按 CPU 使用率排序
- `M`：按記憶體使用率排序
- `T`：按執行時間排序
- `k`：終止程序（輸入 PID）
- `r`：重新設定優先級
- `q`：退出
- `h`：顯示說明

#### htop

互動式程序監控工具（需要安裝）。

```bash
# 安裝
sudo apt install htop

# 啟動
htop

# 指定更新間隔
htop -d 10
```

**htop 優點：**
- 更直觀的介面
- 支援滑鼠操作
- 彩色顯示
- 更容易的排序和篩選

#### vmstat

顯示虛擬記憶體統計資訊。

```bash
# 顯示一次統計
vmstat

# 每 2 秒更新一次，共 5 次
vmstat 2 5

# 顯示詳細資訊
vmstat -s
```

**輸出說明：**
- `r`：等待執行的程序數
- `b`：不可中斷的程序數
- `swpd`：使用的虛擬記憶體
- `free`：空閒記憶體
- `buff`：緩衝區記憶體
- `cache`：快取記憶體
- `si`：從磁碟交換進來的記憶體
- `so`：交換到磁碟的記憶體
- `us`：使用者 CPU 時間百分比
- `sy`：系統 CPU 時間百分比
- `id`：空閒 CPU 時間百分比

#### mpstat

顯示每個 CPU 核心的統計資訊（需要安裝 sysstat）。

```bash
# 安裝
sudo apt install sysstat

# 顯示所有 CPU 統計
mpstat

# 每 2 秒更新一次
mpstat 2

# 顯示每個 CPU 核心
mpstat -P ALL
```

### 記憶體監控

#### free

顯示記憶體使用情況。

```bash
# 顯示記憶體資訊
free

# 以人類可讀格式顯示
free -h

# 持續監控（每 5 秒更新）
free -s 5

# 顯示總計
free -t

# 顯示詳細資訊
free -l
```

**輸出說明：**
- `total`：總記憶體
- `used`：已使用記憶體
- `free`：空閒記憶體
- `shared`：共享記憶體
- `buff/cache`：緩衝區和快取
- `available`：可用記憶體

#### /proc/meminfo

查看詳細的記憶體資訊。

```bash
# 查看記憶體資訊
cat /proc/meminfo

# 查看特定資訊
grep MemTotal /proc/meminfo
grep MemFree /proc/meminfo
grep MemAvailable /proc/meminfo
```

### 磁碟監控

#### df

顯示檔案系統磁碟空間使用情況。

```bash
# 顯示所有檔案系統
df

# 以人類可讀格式顯示
df -h

# 顯示 inode 使用情況
df -i

# 顯示特定檔案系統類型
df -t ext4

# 顯示總計
df -h --total
```

#### du

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

# 按大小排序
du -h | sort -h

# 找出最大的目錄
du -h --max-depth=1 | sort -hr | head -10
```

#### iostat

顯示 CPU 和 I/O 統計資訊（需要安裝 sysstat）。

```bash
# 安裝
sudo apt install sysstat

# 顯示一次統計
iostat

# 每 2 秒更新一次
iostat 2

# 顯示詳細資訊
iostat -x

# 顯示特定裝置
iostat -x /dev/sda
```

### 網路監控

#### ifconfig

顯示和設定網路介面。

```bash
# 顯示所有網路介面
ifconfig

# 顯示特定介面
ifconfig eth0

# 啟用/停用介面
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# 設定 IP 位址
sudo ifconfig eth0 192.168.1.100
```

#### ip

現代網路管理工具（推薦使用）。

```bash
# 顯示所有介面
ip addr show
ip a

# 顯示路由表
ip route show
ip r

# 顯示網路統計
ip -s link show

# 顯示特定介面
ip addr show eth0
```

#### netstat

顯示網路連線、路由表和介面統計。

```bash
# 顯示所有連線
netstat -a

# 顯示 TCP 連線
netstat -t

# 顯示 UDP 連線
netstat -u

# 顯示監聽的埠
netstat -l

# 顯示程序資訊
netstat -p

# 顯示數字格式（不解析主機名）
netstat -n

# 組合使用
netstat -tulpn
```

#### ss

現代網路統計工具（推薦使用，比 netstat 更快）。

```bash
# 顯示所有連線
ss -a

# 顯示 TCP 連線
ss -t

# 顯示 UDP 連線
ss -u

# 顯示監聽的埠
ss -l

# 顯示程序資訊
ss -p

# 顯示數字格式
ss -n

# 組合使用
ss -tulpn
```

#### nethogs

按程序顯示網路使用情況（需要安裝）。

```bash
# 安裝
sudo apt install nethogs

# 監控網路使用
sudo nethogs
```

## 程序管理

### ps

顯示執行中的程序。

```bash
# 顯示目前終端機的程序
ps

# 顯示所有程序
ps aux

# 顯示程序樹
ps auxf

# 顯示特定使用者的程序
ps -u username

# 顯示程序詳細資訊
ps -ef

# 搜尋特定程序
ps aux | grep process_name

# 按 CPU 使用率排序
ps aux --sort=-%cpu | head -10

# 按記憶體使用率排序
ps aux --sort=-%mem | head -10
```

**輸出欄位說明：**
- `USER`：程序擁有者
- `PID`：程序 ID
- `%CPU`：CPU 使用率
- `%MEM`：記憶體使用率
- `VSZ`：虛擬記憶體大小
- `RSS`：實體記憶體大小
- `TTY`：終端機
- `STAT`：程序狀態
- `START`：啟動時間
- `TIME`：CPU 時間
- `COMMAND`：命令

### kill

終止程序。

```bash
# 終止程序（預設 SIGTERM）
kill PID

# 強制終止（SIGKILL）
kill -9 PID

# 終止多個程序
kill PID1 PID2 PID3

# 終止所有同名程序
killall process_name

# 強制終止所有同名程序
killall -9 process_name

# 終止特定使用者的程序
pkill -u username process_name
```

**常用信號：**
- `SIGTERM (15)`：正常終止（預設）
- `SIGKILL (9)`：強制終止（無法被忽略）
- `SIGHUP (1)`：掛起信號
- `SIGINT (2)`：中斷信號（Ctrl+C）

### jobs

管理背景工作。

```bash
# 顯示背景工作
jobs

# 顯示工作 ID
jobs -l

# 將工作移到前景
fg %1
fg 1

# 將工作移到背景
bg %1
bg 1

# 終止背景工作
kill %1
```

### nohup

讓程序在終端關閉後繼續執行。

```bash
# 執行程序並忽略 SIGHUP 信號
nohup command &

# 輸出重導向
nohup command > output.log 2>&1 &
```

### nice 和 renice

設定程序優先級。

```bash
# 以較低優先級執行（nice 值 10）
nice -n 10 command

# 以較高優先級執行（需要 root）
sudo nice -n -10 command

# 修改執行中程序的優先級
renice 10 PID

# 修改所有同名程序的優先級
renice 10 -p PID1 PID2
renice 10 -u username
```

**優先級說明：**
- nice 值範圍：-20 到 19
- 較小的值 = 較高優先級
- 預設 nice 值：0
- 一般使用者只能提高 nice 值（降低優先級）

## 資源限制

### ulimit

設定和顯示 shell 資源限制。

```bash
# 顯示所有限制
ulimit -a

# 顯示特定限制
ulimit -n    # 開啟檔案數
ulimit -u    # 程序數
ulimit -s    # 堆疊大小
ulimit -v    # 虛擬記憶體

# 設定限制（目前 shell）
ulimit -n 1024

# 設定軟限制
ulimit -Sn 1024

# 設定硬限制（需要 root）
sudo ulimit -Hn 2048
```

**常用限制：**
- `-n`：開啟檔案描述符數量
- `-u`：使用者程序數
- `-s`：堆疊大小（KB）
- `-v`：虛擬記憶體（KB）
- `-t`：CPU 時間（秒）
- `-f`：檔案大小（KB）

### /etc/security/limits.conf

系統級資源限制設定檔。

```bash
# 編輯設定檔
sudo nano /etc/security/limits.conf
```

**設定格式：**
```
<domain> <type> <item> <value>
```

**範例：**
```
# 限制使用者 albert 的開啟檔案數
albert soft nofile 1024
albert hard nofile 2048

# 限制所有使用者的程序數
* soft nproc 512
* hard nproc 1024
```

**設定項目：**
- `nofile`：開啟檔案數
- `nproc`：程序數
- `memlock`：鎖定記憶體
- `stack`：堆疊大小

## 系統服務管理

### systemd

現代 Linux 系統的服務管理系統。

#### systemctl

管理 systemd 服務。

```bash
# 啟動服務
sudo systemctl start service_name

# 停止服務
sudo systemctl stop service_name

# 重新啟動服務
sudo systemctl restart service_name

# 重新載入設定（不重啟）
sudo systemctl reload service_name

# 查看服務狀態
systemctl status service_name

# 啟用開機自動啟動
sudo systemctl enable service_name

# 停用開機自動啟動
sudo systemctl disable service_name

# 查看是否啟用
systemctl is-enabled service_name

# 查看所有服務
systemctl list-units --type=service

# 查看啟用的服務
systemctl list-units --type=service --state=running

# 查看失敗的服務
systemctl --failed
```

#### journalctl

查看 systemd 日誌。

```bash
# 查看所有日誌
sudo journalctl

# 查看特定服務的日誌
sudo journalctl -u service_name

# 查看最近的日誌
sudo journalctl -n 50

# 即時跟隨日誌
sudo journalctl -f

# 查看特定時間範圍的日誌
sudo journalctl --since "2024-01-01 00:00:00"
sudo journalctl --until "2024-01-01 23:59:59"

# 查看今天的日誌
sudo journalctl --since today

# 查看錯誤日誌
sudo journalctl -p err
```

## 工作排程

### cron

定時執行任務的系統。

#### crontab

管理 cron 工作。

```bash
# 編輯目前使用者的 crontab
crontab -e

# 列出目前使用者的 crontab
crontab -l

# 刪除目前使用者的 crontab
crontab -r

# 編輯特定使用者的 crontab（需要 root）
sudo crontab -u username -e
```

#### crontab 語法

```
分 時 日 月 星期 命令
*  *  *  *  *   command
│  │  │  │  │
│  │  │  │  └── 星期幾 (0-7, 0 和 7 都是星期日)
│  │  │  └───── 月份 (1-12)
│  │  └──────── 日期 (1-31)
│  └─────────── 小時 (0-23)
└────────────── 分鐘 (0-59)
```

**特殊字元：**
- `*`：任何值
- `,`：分隔多個值（例如：1,3,5）
- `-`：範圍（例如：1-5）
- `/`：間隔（例如：*/5 表示每 5 個單位）

**範例：**
```bash
# 每分鐘執行
* * * * * command

# 每小時執行（每小時的第 0 分）
0 * * * * command

# 每天執行（每天 00:00）
0 0 * * * command

# 每週執行（每週日 00:00）
0 0 * * 0 command

# 每月執行（每月 1 日 00:00）
0 0 1 * * command

# 每 5 分鐘執行
*/5 * * * * command

# 工作時間執行（週一到週五 9:00-17:00）
0 9-17 * * 1-5 command

# 特定時間執行
30 14 * * * command  # 每天 14:30
0 0 1 1 * command    # 每年 1 月 1 日 00:00
```

#### anacron

處理錯過的 cron 工作（適合筆記型電腦）。

```bash
# 查看 anacron 設定
cat /etc/anacrontab

# 執行 anacron
sudo anacron
```

### at

執行一次性排程任務。

```bash
# 安裝
sudo apt install at

# 啟動服務
sudo systemctl start atd
sudo systemctl enable atd

# 排程任務
at 14:30
at 14:30 tomorrow
at now + 1 hour
at 14:30 Jan 1 2024

# 查看排程
atq

# 刪除排程
atrm job_number
```

## 系統資訊

### uptime

顯示系統運行時間和負載。

```bash
# 顯示系統資訊
uptime

# 輸出說明：
# 目前時間、運行時間、使用者數、平均負載（1、5、15 分鐘）
```

### w

顯示登入使用者和他們執行的程序。

```bash
# 顯示使用者資訊
w

# 顯示特定使用者
w username

# 簡潔格式
w -s
```

### who

顯示目前登入的使用者。

```bash
# 顯示登入使用者
who

# 顯示詳細資訊
who -a

# 顯示登入時間
who -T
```

### lscpu

顯示 CPU 架構資訊。

```bash
# 顯示 CPU 資訊
lscpu

# 顯示特定資訊
lscpu | grep "Model name"
lscpu | grep "CPU(s)"
```

### lsblk

列出所有區塊裝置。

```bash
# 列出所有區塊裝置
lsblk

# 顯示詳細資訊
lsblk -f

# 顯示樹狀結構
lsblk -t
```

### lsof

列出開啟的檔案和程序。

```bash
# 列出所有開啟的檔案
sudo lsof

# 列出特定程序的檔案
sudo lsof -p PID

# 列出特定使用者開啟的檔案
sudo lsof -u username

# 列出特定檔案被哪些程序使用
sudo lsof /path/to/file

# 列出特定埠的使用情況
sudo lsof -i :80
sudo lsof -i tcp:80
```

## 實用範例

### 監控腳本

```bash
#!/bin/bash
# 系統資源監控腳本

echo "=== CPU 使用率 ==="
top -bn1 | grep "Cpu(s)" | awk '{print $2}'

echo "=== 記憶體使用 ==="
free -h | grep Mem | awk '{print $3 "/" $2}'

echo "=== 磁碟使用 ==="
df -h / | awk 'NR==2 {print $5}'

echo "=== 前 5 個 CPU 使用程序 ==="
ps aux --sort=-%cpu | head -6
```

### 清理舊檔案

```bash
# 找出並刪除 30 天前的日誌檔案
find /var/log -type f -mtime +30 -delete

# 清理暫存檔案
find /tmp -type f -atime +7 -delete
```

### 監控磁碟空間

```bash
# 檢查磁碟使用率，超過 80% 時警告
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 80 ]; then
    echo "Warning: Disk usage is ${DISK_USAGE}%"
fi
```

### 自動備份腳本

```bash
#!/bin/bash
# 每日備份腳本

BACKUP_DIR="/backup"
SOURCE_DIR="/home/user"
DATE=$(date +%Y%m%d)
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

mkdir -p $BACKUP_DIR
tar -czf $BACKUP_FILE $SOURCE_DIR

# 刪除 7 天前的備份
find $BACKUP_DIR -name "backup_*.tar.gz" -mtime +7 -delete
```

## 最佳實踐

1. **定期監控**：建立定期監控機制，及時發現問題
2. **設定警報**：當資源使用率超過閾值時發送警報
3. **資源限制**：適當設定資源限制，防止單一程序消耗過多資源
4. **日誌管理**：定期清理日誌檔案，避免佔用過多磁碟空間
5. **備份策略**：建立自動備份機制，保護重要資料
6. **效能調校**：根據監控結果進行系統調校
7. **文件記錄**：記錄系統配置和變更，便於問題排查

## 相關檔案位置

- 系統日誌：`/var/log/`
- 程序資訊：`/proc/`
- 系統資訊：`/sys/`
- Cron 設定：`/etc/crontab`, `/var/spool/cron/`
- 資源限制：`/etc/security/limits.conf`
- Systemd 服務：`/etc/systemd/system/`