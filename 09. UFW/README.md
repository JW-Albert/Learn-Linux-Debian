# UFW (Uncomplicated Firewall) 防火牆

## 簡介

UFW (Uncomplicated Firewall) 是 Ubuntu 和 Debian 系統中的簡化防火牆管理工具，它提供了簡單的命令列介面來管理 iptables。UFW 讓防火牆設定變得更容易，適合初學者和進階使用者。

### UFW 的特點

1. **簡單易用**：提供直觀的命令列介面
2. **預設安全**：預設拒絕所有連入，允許所有連出
3. **應用程式配置**：支援預設的應用程式配置檔
4. **IPv6 支援**：同時支援 IPv4 和 IPv6
5. **狀態追蹤**：可以追蹤連線狀態

### 為什麼需要防火牆

- **保護系統**：阻擋未授權的存取
- **限制服務**：只開放必要的埠
- **防止攻擊**：減少系統暴露的攻擊面
- **網路安全**：符合安全最佳實踐

## 安裝 UFW

### 安裝

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install ufw

# 檢查版本
ufw --version
```

### 檢查狀態

```bash
# 查看 UFW 狀態
sudo ufw status

# 詳細狀態
sudo ufw status verbose

# 顯示編號的規則
sudo ufw status numbered
```

## 基本操作

### 啟用和停用

```bash
# 啟用 UFW（會提示確認）
sudo ufw enable

# 啟用但不提示
sudo ufw --force enable

# 停用 UFW
sudo ufw disable

# 重新載入規則
sudo ufw reload
```

### 重置規則

```bash
# 重置所有規則（回到預設狀態）
sudo ufw --force reset

# 注意：這會刪除所有自訂規則
```

## 基本規則語法

### 允許連線

```bash
# 允許特定埠（TCP）
sudo ufw allow 22/tcp

# 允許特定埠（UDP）
sudo ufw allow 53/udp

# 允許特定埠（TCP 和 UDP）
sudo ufw allow 80

# 允許埠範圍
sudo ufw allow 8000:9000/tcp

# 允許特定 IP 存取特定埠
sudo ufw allow from 192.168.1.100 to any port 22

# 允許特定網段
sudo ufw allow from 192.168.1.0/24 to any port 22
```

### 拒絕連線

```bash
# 拒絕特定埠
sudo ufw deny 23/tcp

# 拒絕特定 IP
sudo ufw deny from 192.168.1.100

# 拒絕特定 IP 存取特定埠
sudo ufw deny from 192.168.1.100 to any port 80
```

### 刪除規則

```bash
# 方法 1：使用規則編號
sudo ufw status numbered
sudo ufw delete 3

# 方法 2：使用完整規則
sudo ufw delete allow 22/tcp

# 方法 3：刪除允許規則
sudo ufw delete allow 80

# 方法 4：刪除拒絕規則
sudo ufw delete deny 23/tcp
```

## 常用埠設定

### SSH (22)

```bash
# 允許 SSH（重要：在啟用 UFW 前先設定）
sudo ufw allow 22/tcp

# 或使用應用程式名稱
sudo ufw allow ssh

# 只允許特定 IP 使用 SSH
sudo ufw allow from 192.168.1.100 to any port 22
```

### HTTP/HTTPS (80/443)

```bash
# 允許 HTTP
sudo ufw allow 80/tcp
sudo ufw allow http

# 允許 HTTPS
sudo ufw allow 443/tcp
sudo ufw allow https

# 同時允許
sudo ufw allow 80,443/tcp
```

### 其他常用服務

```bash
# FTP (21)
sudo ufw allow 21/tcp

# SMTP (25)
sudo ufw allow 25/tcp

# DNS (53)
sudo ufw allow 53/tcp
sudo ufw allow 53/udp

# DHCP (67/68)
sudo ufw allow 67/udp
sudo ufw allow 68/udp

# MySQL (3306)
sudo ufw allow 3306/tcp

# PostgreSQL (5432)
sudo ufw allow 5432/tcp

# Redis (6379)
sudo ufw allow 6379/tcp
```

## 應用程式配置

### 查看可用應用程式

```bash
# 列出所有應用程式配置
sudo ufw app list

# 查看應用程式詳細資訊
sudo ufw app info 'Apache'

# 查看應用程式包含的規則
sudo ufw app info 'Apache Full'
```

### 使用應用程式配置

```bash
# 允許應用程式（使用預設配置）
sudo ufw allow 'Apache'

# 允許應用程式（完整配置）
sudo ufw allow 'Apache Full'

# 允許應用程式（僅 HTTP）
sudo ufw allow 'Apache Secure'

# 其他常用應用程式
sudo ufw allow 'Nginx Full'
sudo ufw allow 'OpenSSH'
sudo ufw allow 'MySQL'
```

### 建立自訂應用程式配置

```bash
# 建立應用程式配置檔
sudo nano /etc/ufw/applications.d/myapp

# 配置檔格式
[MyApp]
title=My Application
description=My custom application
ports=8080/tcp|9090/tcp

# 重新載入應用程式列表
sudo ufw app update MyApp

# 使用自訂應用程式
sudo ufw allow 'MyApp'
```

## 進階規則

### 指定介面

```bash
# 允許特定介面的連線
sudo ufw allow in on eth0 to any port 22

# 允許特定介面的連出
sudo ufw allow out on eth0
```

### 指定協定

```bash
# 只允許 TCP
sudo ufw allow 22/tcp

# 只允許 UDP
sudo ufw allow 53/udp

# 允許 TCP 和 UDP
sudo ufw allow 80
```

### ICMP 封包控制

ICMP (Internet Control Message Protocol) 是網路層協定，用於網路診斷和錯誤報告。常見的 ICMP 應用包括 ping 和 traceroute。

#### ICMP 類型說明

常見的 ICMP 類型：
- `echo-request` (8)：ping 請求
- `echo-reply` (0)：ping 回應
- `destination-unreachable` (3)：目標不可達
- `time-exceeded` (11)：時間超時（traceroute 使用）
- `parameter-problem` (12)：參數問題

#### 允許 ICMP

```bash
# 允許所有 ICMP 封包
sudo ufw allow icmp

# 允許 ping（echo-request 和 echo-reply）
sudo ufw allow in icmp type echo-request
sudo ufw allow out icmp type echo-reply

# 允許特定 ICMP 類型
sudo ufw allow in icmp type destination-unreachable
sudo ufw allow in icmp type time-exceeded
sudo ufw allow in icmp type parameter-problem

# 允許特定來源的 ICMP
sudo ufw allow from 192.168.1.100 proto icmp
```

#### 拒絕 ICMP

```bash
# 拒絕所有 ICMP 封包
sudo ufw deny icmp

# 拒絕 ping（防止 ping 掃描）
sudo ufw deny in icmp type echo-request

# 拒絕特定 ICMP 類型
sudo ufw deny in icmp type destination-unreachable
```

#### ICMP 類型完整列表

```bash
# 查看可用的 ICMP 類型
sudo ufw allow help | grep icmp

# 常用 ICMP 類型
echo-request              # Ping 請求
echo-reply                # Ping 回應
destination-unreachable    # 目標不可達
source-quench             # 源站抑制
redirect                  # 重定向
time-exceeded             # 時間超時
parameter-problem         # 參數問題
timestamp-request         # 時間戳請求
timestamp-reply           # 時間戳回應
```

#### ICMP 設定範例

**允許 ping（基本設定）：**
```bash
# 允許接收 ping 請求
sudo ufw allow in icmp type echo-request

# 允許發送 ping 回應
sudo ufw allow out icmp type echo-reply
```

**完全阻止 ping（提高安全性）：**
```bash
# 拒絕 ping 請求（防止主機被發現）
sudo ufw deny in icmp type echo-request

# 仍允許其他必要的 ICMP 類型
sudo ufw allow in icmp type destination-unreachable
sudo ufw allow in icmp type time-exceeded
sudo ufw allow in icmp type parameter-problem
```

**允許特定網段使用 ping：**
```bash
# 只允許內部網路使用 ping
sudo ufw allow from 192.168.1.0/24 proto icmp type echo-request
sudo ufw allow from 192.168.1.0/24 proto icmp type echo-reply
```

**允許 traceroute 所需 ICMP：**
```bash
# traceroute 需要 time-exceeded 和 destination-unreachable
sudo ufw allow in icmp type time-exceeded
sudo ufw allow in icmp type destination-unreachable
```

#### 查看 ICMP 規則

```bash
# 查看所有規則（包含 ICMP）
sudo ufw status verbose

# 查看特定 ICMP 規則
sudo ufw status | grep icmp
```

### 指定來源和目標

```bash
# 允許特定 IP 連線
sudo ufw allow from 192.168.1.100

# 允許特定 IP 存取特定埠
sudo ufw allow from 192.168.1.100 to any port 22

# 允許特定 IP 範圍
sudo ufw allow from 192.168.1.0/24

# 允許連線到特定 IP
sudo ufw allow to 192.168.1.100 port 22
```

### 記錄規則

```bash
# 允許並記錄
sudo ufw allow 22/tcp comment 'Allow SSH'

# 拒絕並記錄
sudo ufw deny 23/tcp comment 'Deny Telnet'
```

## 預設規則

### 查看預設規則

```bash
# 查看預設規則
sudo ufw default

# 查看詳細預設規則
sudo ufw default verbose
```

### 設定預設規則

```bash
# 設定預設連入政策（拒絕）
sudo ufw default deny incoming

# 設定預設連出政策（允許）
sudo ufw default allow outgoing

# 設定預設轉發政策
sudo ufw default deny forwarding
```

**預設規則說明：**
- `deny incoming`：預設拒絕所有連入連線
- `allow outgoing`：預設允許所有連出連線
- `deny forwarding`：預設拒絕轉發

## 日誌記錄

### 啟用日誌

```bash
# 啟用日誌（低詳細度）
sudo ufw logging on

# 啟用日誌（中等詳細度）
sudo ufw logging medium

# 啟用日誌（高詳細度）
sudo ufw logging high

# 啟用日誌（低詳細度）
sudo ufw logging low

# 停用日誌
sudo ufw logging off
```

### 查看日誌

```bash
# 查看 UFW 日誌
sudo tail -f /var/log/ufw.log

# 查看最近的日誌
sudo tail -n 100 /var/log/ufw.log

# 搜尋特定 IP
sudo grep "192.168.1.100" /var/log/ufw.log
```

## 實用範例

### 基本伺服器設定

```bash
# 1. 設定預設規則
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 2. 允許 SSH（重要：先設定）
sudo ufw allow 22/tcp

# 3. 允許 HTTP 和 HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# 4. 啟用 UFW
sudo ufw enable

# 5. 檢查狀態
sudo ufw status verbose
```

### Web 伺服器設定

```bash
# 允許 HTTP
sudo ufw allow 'Apache'
# 或
sudo ufw allow 80/tcp

# 允許 HTTPS
sudo ufw allow 'Apache Secure'
# 或
sudo ufw allow 443/tcp

# 允許 SSH（從特定 IP）
sudo ufw allow from 192.168.1.0/24 to any port 22
```

### 資料庫伺服器設定

```bash
# 允許 SSH
sudo ufw allow 22/tcp

# 允許 MySQL（僅從內部網路）
sudo ufw allow from 192.168.1.0/24 to any port 3306

# 允許 PostgreSQL（僅從內部網路）
sudo ufw allow from 192.168.1.0/24 to any port 5432
```

### 限制 SSH 存取

```bash
# 只允許特定 IP 使用 SSH
sudo ufw delete allow 22/tcp
sudo ufw allow from 192.168.1.100 to any port 22

# 或允許整個網段
sudo ufw allow from 192.168.1.0/24 to any port 22
```

### ICMP 控制範例

```bash
# 範例 1：允許 ping（基本設定）
sudo ufw allow in icmp type echo-request
sudo ufw allow out icmp type echo-reply

# 範例 2：完全阻止 ping（提高安全性）
sudo ufw deny in icmp type echo-request

# 範例 3：允許內部網路使用 ping
sudo ufw allow from 192.168.1.0/24 proto icmp type echo-request
sudo ufw allow from 192.168.1.0/24 proto icmp type echo-reply

# 範例 4：允許 traceroute 所需 ICMP
sudo ufw allow in icmp type time-exceeded
sudo ufw allow in icmp type destination-unreachable

# 範例 5：允許所有必要的 ICMP 類型（平衡安全與功能）
sudo ufw allow in icmp type echo-request
sudo ufw allow in icmp type destination-unreachable
sudo ufw allow in icmp type time-exceeded
sudo ufw allow in icmp type parameter-problem
```

### 允許特定服務

```bash
# 允許 FTP
sudo ufw allow 21/tcp

# 允許 SMTP
sudo ufw allow 25/tcp

# 允許 DNS
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
```

## 故障排除

### 檢查規則

```bash
# 查看所有規則
sudo ufw status verbose

# 查看編號規則
sudo ufw status numbered

# 查看應用程式
sudo ufw app list
```

### 測試連線

```bash
# 從另一台機器測試
telnet server-ip 22
nc -zv server-ip 80

# 檢查服務是否監聽
sudo netstat -tlnp | grep :22
sudo ss -tlnp | grep :22
```

### 常見問題

**問題：啟用 UFW 後無法 SSH 連線**

```bash
# 解決：在啟用前先允許 SSH
sudo ufw allow 22/tcp
sudo ufw enable
```

**問題：規則順序錯誤**

```bash
# 解決：使用編號刪除並重新加入
sudo ufw status numbered
sudo ufw delete [編號]
sudo ufw insert [位置] allow 22/tcp
```

**問題：無法刪除規則**

```bash
# 解決：使用完整規則語法
sudo ufw delete allow 22/tcp
# 或使用編號
sudo ufw delete [編號]
```

## 最佳實踐

1. **先允許 SSH**：在啟用 UFW 前先允許 SSH，避免被鎖在外面
2. **最小權限原則**：只開放必要的埠
3. **限制來源 IP**：盡可能限制特定 IP 或網段
4. **ICMP 控制**：
   - 生產環境可以拒絕 ping 請求以提高安全性
   - 開發環境可以允許 ping 以便診斷
   - 至少允許必要的 ICMP 類型（destination-unreachable, time-exceeded）
5. **定期檢查規則**：定期檢查和清理不需要的規則
6. **啟用日誌**：啟用日誌記錄以便追蹤問題
7. **測試規則**：在生產環境前先測試規則
8. **備份規則**：備份 UFW 設定檔

## 相關檔案

### 設定檔位置

```bash
# UFW 設定檔
/etc/ufw/ufw.conf

# 應用程式配置
/etc/ufw/applications.d/

# 規則檔案
/etc/ufw/before.rules
/etc/ufw/after.rules
/etc/ufw/user.rules
/etc/ufw/user6.rules

# 日誌檔案
/var/log/ufw.log
```

### 備份和還原

```bash
# 備份規則
sudo cp -r /etc/ufw /backup/ufw-backup

# 還原規則
sudo cp -r /backup/ufw-backup/* /etc/ufw/
sudo ufw reload
```

## 常用指令速查

```bash
# 狀態
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered

# 啟用/停用
sudo ufw enable
sudo ufw disable
sudo ufw reload

# 基本規則
sudo ufw allow 22/tcp
sudo ufw deny 23/tcp
sudo ufw delete allow 22/tcp

# ICMP 控制
sudo ufw allow icmp
sudo ufw allow in icmp type echo-request
sudo ufw deny in icmp type echo-request

# 應用程式
sudo ufw app list
sudo ufw allow 'Apache'

# 預設規則
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 日誌
sudo ufw logging on
sudo tail -f /var/log/ufw.log
```

## 與 iptables 的關係

UFW 是 iptables 的前端工具，實際上 UFW 會產生 iptables 規則。

```bash
# 查看 iptables 規則（UFW 產生的）
sudo iptables -L -n -v

# 查看 iptables 規則（包含 UFW）
sudo iptables -S

# 注意：不建議直接修改 iptables，應該使用 UFW
```

## 參考資源

- UFW 官方文件：https://help.ubuntu.com/community/UFW
- UFW 手冊：`man ufw`
- iptables 文件：`man iptables`

