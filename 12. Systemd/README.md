# Systemd 系統管理

## 簡介

Systemd 是現代 Linux 系統的初始化系統和服務管理器，取代了傳統的 SysV init 系統。Systemd 不僅管理系統服務，還負責日誌記錄、裝置管理、網路配置等多項功能。

### Systemd 的特點

1. **並行啟動**：服務可以並行啟動，加快系統啟動速度
2. **依賴管理**：自動處理服務之間的依賴關係
3. **日誌整合**：內建日誌系統（journald）
4. **單元管理**：統一管理服務、掛載點、裝置等
5. **資源控制**：可以限制服務的資源使用

### Systemd 的組成

- **systemd**：核心程序
- **systemctl**：服務管理工具
- **journalctl**：日誌查看工具
- **systemd-analyze**：系統分析工具

## 基本概念

### 單元（Unit）

Systemd 使用「單元」來管理系統資源，每種資源類型對應不同的單元類型：

- **service**：系統服務
- **target**：啟動目標（類似 runlevel）
- **mount**：檔案系統掛載點
- **device**：裝置
- **timer**：定時器（替代 cron）
- **socket**：通訊端
- **path**：路徑監控

### 單元檔案位置

```bash
# 系統單元檔案
/etc/systemd/system/          # 系統管理員建立的單元
/usr/lib/systemd/system/      # 套件安裝的單元
/lib/systemd/system/          # 系統核心單元

# 使用者單元檔案
~/.config/systemd/user/      # 使用者建立的單元
/usr/lib/systemd/user/       # 系統提供的使用者單元
```

## systemctl 服務管理

### 基本操作

```bash
# 啟動服務
sudo systemctl start service-name

# 停止服務
sudo systemctl stop service-name

# 重新啟動服務
sudo systemctl restart service-name

# 重新載入設定（不重啟服務）
sudo systemctl reload service-name

# 查看服務狀態
systemctl status service-name

# 啟用開機自動啟動
sudo systemctl enable service-name

# 停用開機自動啟動
sudo systemctl disable service-name

# 查看是否啟用
systemctl is-enabled service-name

# 查看是否啟用並運行
systemctl is-active service-name
```

### 查看服務

```bash
# 列出所有服務
systemctl list-units --type=service

# 列出所有服務（包含未啟動的）
systemctl list-units --type=service --all

# 列出正在運行的服務
systemctl list-units --type=service --state=running

# 列出失敗的服務
systemctl --failed

# 列出啟用的服務
systemctl list-unit-files --type=service --state=enabled

# 搜尋服務
systemctl list-units --type=service | grep nginx
```

### 服務狀態說明

```bash
# 查看服務詳細狀態
systemctl status service-name
```

**狀態說明：**
- `active (running)`：服務正在運行
- `active (exited)`：服務已執行完成
- `inactive (dead)`：服務未運行
- `failed`：服務啟動失敗
- `activating`：服務正在啟動中

## 單元檔案管理

### 查看單元檔案

```bash
# 查看服務單元檔案位置
systemctl show -p FragmentPath service-name

# 查看單元檔案內容
systemctl cat service-name

# 查看單元檔案依賴
systemctl list-dependencies service-name
```

### 建立自訂服務

#### 基本服務單元檔案

建立 `/etc/systemd/system/my-service.service`：

```ini
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/my-script.sh
Restart=always
RestartSec=10
User=myuser
Group=mygroup

[Install]
WantedBy=multi-user.target
```

#### 單元檔案區段說明

**\[Unit\] 區段：**
- `Description`：服務描述
- `After`：在此服務之後啟動
- `Before`：在此服務之前啟動
- `Requires`：強制依賴（依賴失敗則不啟動）
- `Wants`：弱依賴（依賴失敗仍可啟動）

**\[Service\] 區段：**
- `Type`：服務類型
  - `simple`：直接執行（預設）
  - `forking`：fork 後退出
  - `oneshot`：執行一次後退出
  - `notify`：使用通知機制
- `ExecStart`：啟動命令
- `ExecStop`：停止命令
- `ExecReload`：重新載入命令
- `Restart`：重啟策略
  - `no`：不重啟（預設）
  - `always`：總是重啟
  - `on-failure`：失敗時重啟
  - `on-abort`：異常終止時重啟
- `RestartSec`：重啟延遲時間（秒）
- `User`：執行使用者
- `Group`：執行群組
- `WorkingDirectory`：工作目錄
- `Environment`：環境變數

**\[Install\] 區段：**
- `WantedBy`：被哪些 target 需要
- `RequiredBy`：被哪些 target 強制需要

#### 套用自訂服務

```bash
# 重新載入 systemd 配置
sudo systemctl daemon-reload

# 啟用服務
sudo systemctl enable my-service.service

# 啟動服務
sudo systemctl start my-service.service

# 查看狀態
systemctl status my-service.service
```

### 服務範例

#### Python 腳本服務

```ini
[Unit]
Description=Python Web Service
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/myapp
ExecStart=/usr/bin/python3 /var/www/myapp/app.py
Restart=always
RestartSec=10
Environment="PATH=/usr/bin:/usr/local/bin"

[Install]
WantedBy=multi-user.target
```

#### Node.js 應用服務

```ini
[Unit]
Description=Node.js Application
After=network.target

[Service]
Type=simple
User=nodejs
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/node /opt/myapp/server.js
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

## Target 管理

Target 類似於傳統的 runlevel，定義系統的運行狀態。

### 常用 Target

```bash
# 查看所有 target
systemctl list-units --type=target

# 查看目前 target
systemctl get-default

# 設定預設 target
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target

# 切換到特定 target
sudo systemctl isolate multi-user.target
```

**常用 Target：**
- `multi-user.target`：多使用者模式（無圖形介面）
- `graphical.target`：圖形介面模式
- `rescue.target`：救援模式
- `emergency.target`：緊急模式

### 查看 Target 依賴

```bash
# 查看 target 包含的服務
systemctl list-dependencies multi-user.target
```

## Timer 定時器

Systemd Timer 可以替代 cron，提供更強大的定時任務功能。

### 建立 Timer

#### 1. 建立 Service 單元

`/etc/systemd/system/backup.service`：

```ini
[Unit]
Description=Backup Service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

#### 2. 建立 Timer 單元

`/etc/systemd/system/backup.timer`：

```ini
[Unit]
Description=Backup Timer

[Timer]
OnCalendar=daily
OnCalendar=Mon..Fri 02:00
OnBootSec=10min
OnUnitActiveSec=1h

[Install]
WantedBy=timers.target
```

#### Timer 設定選項

- `OnCalendar`：日曆時間觸發
  - `daily`：每天
  - `weekly`：每週
  - `Mon..Fri 02:00`：週一到週五 02:00
  - `*-*-* 00:00:00`：每天午夜
- `OnBootSec`：開機後觸發
- `OnUnitActiveSec`：服務啟動後觸發
- `OnUnitInactiveSec`：服務停止後觸發

### 管理 Timer

```bash
# 啟用 timer
sudo systemctl enable backup.timer

# 啟動 timer
sudo systemctl start backup.timer

# 查看 timer 狀態
systemctl status backup.timer

# 列出所有 timer
systemctl list-timers

# 列出所有 timer（包含未啟動的）
systemctl list-timers --all

# 查看下次執行時間
systemctl list-timers --all | grep backup
```

## journalctl 日誌管理

### 查看日誌

```bash
# 查看所有日誌
sudo journalctl

# 查看特定服務的日誌
sudo journalctl -u service-name

# 查看最近的日誌
sudo journalctl -n 50

# 即時跟隨日誌
sudo journalctl -f

# 查看特定時間範圍的日誌
sudo journalctl --since "2024-01-01 00:00:00"
sudo journalctl --until "2024-01-01 23:59:59"
sudo journalctl --since today
sudo journalctl --since yesterday

# 查看錯誤日誌
sudo journalctl -p err

# 查看警告及以上級別
sudo journalctl -p warning

# 查看特定優先級
sudo journalctl -p 0..3  # 0=emerg, 1=alert, 2=crit, 3=err
```

### 日誌選項

```bash
# 顯示完整輸出（不截斷）
sudo journalctl --full

# 顯示時間戳記
sudo journalctl -o short-precise

# 以 JSON 格式輸出
sudo journalctl -o json

# 反向顯示（最新的在前）
sudo journalctl -r

# 只顯示核心訊息
sudo journalctl -k

# 只顯示系統訊息
sudo journalctl -b
```

### 日誌管理

```bash
# 查看日誌磁碟使用
sudo journalctl --disk-usage

# 限制日誌大小
sudo journalctl --vacuum-size=100M

# 限制日誌保留時間
sudo journalctl --vacuum-time=7d

# 查看日誌設定
cat /etc/systemd/journald.conf
```

## systemd-analyze 系統分析

### 分析啟動時間

```bash
# 查看系統啟動時間
systemd-analyze

# 查看各服務啟動時間
systemd-analyze blame

# 查看關鍵路徑（最慢的服務）
systemd-analyze critical-chain

# 查看特定服務的啟動時間
systemd-analyze critical-chain service-name

# 生成啟動時間圖（SVG）
systemd-analyze plot > boot-time.svg
```

### 驗證單元檔案

```bash
# 檢查單元檔案語法
systemd-analyze verify service-name

# 檢查所有單元檔案
systemd-analyze verify
```

## 實用範例

### 建立簡單的服務

```bash
# 1. 建立服務檔案
sudo nano /etc/systemd/system/myapp.service

# 2. 加入內容
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/myapp
Restart=always

[Install]
WantedBy=multi-user.target

# 3. 重新載入配置
sudo systemctl daemon-reload

# 4. 啟用並啟動
sudo systemctl enable myapp.service
sudo systemctl start myapp.service
```

### 建立定時備份

```bash
# 1. 建立備份腳本
sudo nano /usr/local/bin/backup.sh
# 加入備份指令

# 2. 給予執行權限
sudo chmod +x /usr/local/bin/backup.sh

# 3. 建立服務
sudo nano /etc/systemd/system/backup.service
[Unit]
Description=Backup Service
[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh

# 4. 建立 Timer
sudo nano /etc/systemd/system/backup.timer
[Unit]
Description=Daily Backup
[Timer]
OnCalendar=daily
OnCalendar=02:00
[Install]
WantedBy=timers.target

# 5. 啟用 Timer
sudo systemctl daemon-reload
sudo systemctl enable backup.timer
sudo systemctl start backup.timer
```

## 故障排除

### 服務無法啟動

```bash
# 1. 查看服務狀態
systemctl status service-name

# 2. 查看詳細日誌
sudo journalctl -u service-name -n 50

# 3. 檢查單元檔案語法
systemd-analyze verify service-name

# 4. 手動測試執行命令
sudo -u user /path/to/command
```

### 服務不斷重啟

```bash
# 查看重啟原因
systemctl status service-name

# 查看詳細日誌
sudo journalctl -u service-name -f

# 暫時停用自動重啟
sudo systemctl edit service-name
# 加入：
[Service]
Restart=no
```

### 依賴問題

```bash
# 查看服務依賴
systemctl list-dependencies service-name

# 查看反向依賴
systemctl list-dependencies --reverse service-name
```

## 最佳實踐

1. **使用 systemctl 管理服務**：避免直接使用 service 指令
2. **定期檢查失敗的服務**：`systemctl --failed`
3. **限制日誌大小**：避免日誌佔用過多空間
4. **使用 Timer 替代 cron**：更強大的定時任務功能
5. **驗證單元檔案**：建立後使用 `systemd-analyze verify` 檢查
6. **記錄服務變更**：便於問題排查

## 常用指令速查

```bash
# 服務管理
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name
sudo systemctl status service-name
sudo systemctl enable service-name
sudo systemctl disable service-name

# 查看服務
systemctl list-units --type=service
systemctl --failed

# 日誌
sudo journalctl -u service-name
sudo journalctl -f
sudo journalctl --since today

# 系統分析
systemd-analyze
systemd-analyze blame

# Timer
systemctl list-timers
sudo systemctl enable timer-name.timer
```

## 相關檔案位置

- 系統單元：`/etc/systemd/system/`
- 套件單元：`/usr/lib/systemd/system/`
- 日誌設定：`/etc/systemd/journald.conf`
- 系統設定：`/etc/systemd/system.conf`

## 參考資源

- systemctl 手冊：`man systemctl`
- journalctl 手冊：`man journalctl`
- systemd.unit 手冊：`man systemd.unit`
- systemd.service 手冊：`man systemd.service`
- systemd.timer 手冊：`man systemd.timer`