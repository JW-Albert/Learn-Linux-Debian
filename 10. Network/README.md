# Linux 網路設定

## 簡介

Linux 網路設定是系統管理的重要部分，包括網路介面配置、IP 位址設定、路由管理、DNS 配置等。理解網路設定可以幫助您管理伺服器連線、診斷網路問題，以及優化網路效能。

## 網路基本概念

### OSI 模型

網路通訊分為七層：

1. **實體層**：電氣和實體規格
2. **資料連結層**：MAC 位址、交換機
3. **網路層**：IP 位址、路由器
4. **傳輸層**：TCP、UDP、埠號
5. **會話層**：建立和管理會話
6. **表現層**：資料格式轉換
7. **應用層**：HTTP、FTP、SSH 等協定

### TCP/IP 模型

實際使用的網路模型：

1. **網路介面層**：實體和資料連結
2. **網路層**：IP 協定
3. **傳輸層**：TCP、UDP
4. **應用層**：應用程式協定

### IP 位址

IP 位址是網路中裝置的唯一識別碼。

**IPv4：**
- 格式：`192.168.1.100`
- 32 位元，分為 4 個 8 位元組
- 範圍：0.0.0.0 到 255.255.255.255

**IPv6：**
- 格式：`2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- 128 位元，分為 8 個 16 位元組
- 簡寫：`2001:db8:85a3::8a2e:370:7334`

### 子網路遮罩

用於劃分網路和主機部分。

**常見子網路遮罩：**
- `255.255.255.0` = `/24`：254 個主機
- `255.255.0.0` = `/16`：65,534 個主機
- `255.0.0.0` = `/8`：16,777,214 個主機

**CIDR 表示法：**
```
192.168.1.0/24    # 192.168.1.0 到 192.168.1.255
10.0.0.0/8        # 10.0.0.0 到 10.255.255.255
172.16.0.0/12     # 172.16.0.0 到 172.31.255.255
```

### 閘道器（Gateway）

連接不同網路的裝置，通常是路由器。

### DNS（Domain Name System）

將網域名稱轉換為 IP 位址。

**常見 DNS 伺服器：**
- Google：`8.8.8.8`、`8.8.4.4`
- Cloudflare：`1.1.1.1`、`1.0.0.1`
- OpenDNS：`208.67.222.222`、`208.67.220.220`

## 查看網路資訊

### 查看網路介面

```bash
# 使用 ip 指令（推薦）
ip addr show
ip a

# 使用 ifconfig（需要安裝 net-tools）
ifconfig
ifconfig -a

# 查看特定介面
ip addr show eth0
ifconfig eth0
```

### 查看路由表

```bash
# 使用 ip 指令
ip route show
ip r

# 使用 route 指令
route -n

# 查看特定路由
ip route get 8.8.8.8
```

### 查看網路連線

```bash
# 使用 ss 指令（推薦）
ss -tulpn

# 使用 netstat（需要安裝 net-tools）
netstat -tulpn

# 使用 lsof 查看網路連線
sudo lsof -i
sudo lsof -i tcp
sudo lsof -i udp
sudo lsof -i :80
sudo lsof -i tcp:80
sudo lsof -i udp:53
sudo lsof -i -sTCP:LISTEN  # 查看監聽的埠

# 查看 TCP 連線
ss -t
netstat -t
sudo lsof -i tcp

# 查看 UDP 連線
ss -u
netstat -u
sudo lsof -i udp

# 查看監聽的埠
ss -l
netstat -l
sudo lsof -i -sTCP:LISTEN
```

### 查看網路統計

```bash
# 查看介面統計
ip -s link show

# 查看特定介面統計
ip -s link show eth0

# 使用 ifconfig
ifconfig eth0
```

## 網路介面管理

### 啟用和停用介面

```bash
# 使用 ip 指令
sudo ip link set eth0 up
sudo ip link set eth0 down

# 使用 ifconfig
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# 使用 ifup/ifdown
sudo ifup eth0
sudo ifdown eth0
```

### 查看介面狀態

```bash
# 查看所有介面狀態
ip link show

# 查看特定介面
ip link show eth0

# 查看詳細資訊
ip -s link show eth0
```

## IP 位址設定

### 臨時設定 IP（重啟後失效）

```bash
# 使用 ip 指令
sudo ip addr add 192.168.1.100/24 dev eth0

# 使用 ifconfig
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0

# 設定 IPv6
sudo ip addr add 2001:db8::1/64 dev eth0
```

### 刪除 IP 位址

```bash
# 使用 ip 指令
sudo ip addr del 192.168.1.100/24 dev eth0

# 使用 ifconfig
sudo ifconfig eth0 0.0.0.0
```

### 永久設定 IP

#### Debian/Ubuntu (Netplan)

Ubuntu 18.04+ 使用 Netplan：

```bash
# 編輯 Netplan 設定檔
sudo nano /etc/netplan/01-netcfg.yaml
```

**設定範例：**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: true
      # 或靜態 IP
      # addresses:
      #   - 192.168.1.100/24
      # gateway4: 192.168.1.1
      # nameservers:
      #   addresses:
      #     - 8.8.8.8
      #     - 8.8.4.4
```

**套用設定：**
```bash
# 測試設定
sudo netplan try

# 套用設定
sudo netplan apply
```

#### Debian (傳統方式)

編輯 `/etc/network/interfaces`：

```bash
sudo nano /etc/network/interfaces
```

**設定範例：**
```
# DHCP
auto eth0
iface eth0 inet dhcp

# 靜態 IP
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

**套用設定：**
```bash
sudo systemctl restart networking
# 或
sudo ifdown eth0 && sudo ifup eth0
```

## 路由設定

### 查看路由表

```bash
# 查看所有路由
ip route show
route -n

# 查看預設路由
ip route show default
route -n | grep '^0.0.0.0'
```

### 新增路由

```bash
# 新增預設閘道器
sudo ip route add default via 192.168.1.1

# 新增特定網路路由
sudo ip route add 192.168.2.0/24 via 192.168.1.1

# 新增主機路由
sudo ip route add 192.168.2.100 via 192.168.1.1
```

### 刪除路由

```bash
# 刪除預設路由
sudo ip route del default

# 刪除特定路由
sudo ip route del 192.168.2.0/24
```

### 永久設定路由

#### 使用 Netplan

在 Netplan 設定檔中加入：

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: 192.168.2.0/24
          via: 192.168.1.1
      gateway4: 192.168.1.1
```

#### 使用 /etc/network/interfaces

```bash
sudo nano /etc/network/interfaces
```

```
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    up ip route add 192.168.2.0/24 via 192.168.1.1
```

## DNS 設定

### 查看 DNS 設定

```bash
# 查看 resolv.conf
cat /etc/resolv.conf

# 查看 systemd-resolved 設定
systemd-resolve --status
resolvectl status
```

### 臨時設定 DNS

```bash
# 編輯 resolv.conf（不建議直接編輯）
sudo nano /etc/resolv.conf
```

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### 永久設定 DNS

#### 使用 Netplan

```yaml
network:
  version: 2
  ethernets:
    eth0:
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

#### 使用 systemd-resolved

```bash
# 編輯設定檔
sudo nano /etc/systemd/resolved.conf
```

```
[Resolve]
DNS=8.8.8.8 8.8.4.4
FallbackDNS=1.1.1.1
```

```bash
# 重新啟動服務
sudo systemctl restart systemd-resolved
```

#### 使用 resolvconf

```bash
# 編輯設定檔
sudo nano /etc/resolvconf/resolv.conf.d/head
```

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

```bash
# 更新設定
sudo resolvconf -u
```

## 網路工具

### ping

測試網路連線。

```bash
# 基本 ping
ping google.com
ping 8.8.8.8

# 指定次數
ping -c 4 google.com

# 指定間隔（秒）
ping -i 2 google.com

# 指定資料包大小
ping -s 1000 google.com

# 設定超時
ping -W 5 google.com
```

### traceroute

追蹤封包路徑。

```bash
# 基本使用
traceroute google.com

# 使用 IP
traceroute 8.8.8.8

# 指定最大跳數
traceroute -m 20 google.com

# 使用 TCP（需要 root）
sudo traceroute -T google.com
```

### mtr

結合 ping 和 traceroute 的工具。

```bash
# 安裝
sudo apt install mtr

# 基本使用
mtr google.com

# 報告模式
mtr -r -c 10 google.com
```

### dig

DNS 查詢工具。

```bash
# 查詢 A 記錄
dig google.com

# 查詢特定 DNS 伺服器
dig @8.8.8.8 google.com

# 查詢 MX 記錄
dig google.com MX

# 簡潔輸出
dig +short google.com
```

### nslookup

DNS 查詢工具（較舊）。

```bash
# 基本查詢
nslookup google.com

# 查詢特定 DNS 伺服器
nslookup google.com 8.8.8.8

# 互動模式
nslookup
> google.com
> exit
```

### host

簡單的 DNS 查詢工具。

```bash
# 基本查詢
host google.com

# 反向查詢
host 8.8.8.8

# 查詢特定記錄類型
host -t MX google.com
```

### curl

傳輸資料的工具。

```bash
# 下載檔案
curl -O https://example.com/file.txt

# 顯示標頭
curl -I https://example.com

# 跟隨重新導向
curl -L https://example.com

# 顯示詳細資訊
curl -v https://example.com
```

### wget

下載工具。

```bash
# 下載檔案
wget https://example.com/file.txt

# 遞迴下載
wget -r https://example.com

# 繼續中斷的下載
wget -c https://example.com/file.txt
```

## 網路故障排除

### 檢查網路連線

```bash
# 檢查介面是否啟用
ip link show

# 檢查 IP 位址
ip addr show

# 檢查路由
ip route show

# 測試連線
ping -c 4 8.8.8.8
ping -c 4 google.com
```

### 檢查 DNS

```bash
# 測試 DNS 解析
nslookup google.com
dig google.com
host google.com

# 檢查 DNS 設定
cat /etc/resolv.conf
resolvectl status
```

### 檢查埠和服務

```bash
# 檢查服務是否監聽
sudo ss -tlnp | grep :80
sudo netstat -tlnp | grep :80
sudo lsof -i :80

# 檢查特定埠
sudo ss -tlnp | grep :22
sudo lsof -i :22
sudo lsof -i tcp:22

# 查看所有監聽的埠
sudo lsof -i -sTCP:LISTEN

# 查看特定程序使用的埠
sudo lsof -i -p PID
```

### 常見問題診斷

**問題：無法連線到網路**

```bash
# 1. 檢查介面狀態
ip link show

# 2. 檢查 IP 位址
ip addr show

# 3. 檢查路由
ip route show

# 4. 測試閘道器
ping -c 4 192.168.1.1

# 5. 測試外部連線
ping -c 4 8.8.8.8
```

**問題：無法解析網域名稱**

```bash
# 1. 檢查 DNS 設定
cat /etc/resolv.conf

# 2. 測試 DNS 伺服器
dig @8.8.8.8 google.com

# 3. 檢查 DNS 服務
systemctl status systemd-resolved
```

**問題：無法連線到特定服務**

```bash
# 1. 檢查服務是否運行
systemctl status service-name

# 2. 檢查防火牆
sudo ufw status
sudo iptables -L

# 3. 檢查埠是否監聽
sudo ss -tlnp | grep :port
```

## 網路效能優化

### 查看網路統計

```bash
# 查看介面統計
ip -s link show

# 持續監控
watch -n 1 'ip -s link show eth0'

# 使用 iftop（需要安裝）
sudo apt install iftop
sudo iftop -i eth0
```

### 調整網路參數

```bash
# 查看 TCP 參數
sysctl net.ipv4.tcp_*

# 調整 TCP 緩衝區大小
sudo sysctl -w net.core.rmem_max=16777216
sudo sysctl -w net.core.wmem_max=16777216

# 永久設定（編輯 /etc/sysctl.conf）
sudo nano /etc/sysctl.conf
```

## 網路安全

### 檢查開放埠

```bash
# 查看監聽的埠
sudo ss -tlnp
sudo netstat -tlnp
sudo lsof -i -sTCP:LISTEN

# 查看所有連線
sudo ss -tulpn
sudo lsof -i

# 查看特定埠的使用情況
sudo lsof -i :80
sudo lsof -i tcp:80
sudo lsof -i udp:53
```

### 設定防火牆

```bash
# 使用 UFW
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw status
```

### 監控網路流量

```bash
# 使用 tcpdump
sudo apt install tcpdump
sudo tcpdump -i eth0

# 使用 Wireshark（圖形介面）
sudo apt install wireshark
```

## 實用範例

### 設定靜態 IP

```bash
# 1. 編輯 Netplan 設定
sudo nano /etc/netplan/01-netcfg.yaml

# 2. 加入設定
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4

# 3. 套用設定
sudo netplan apply
```

### 設定多個 IP

```bash
# 使用 Netplan
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
        - 192.168.1.101/24
```

### 設定 VLAN

```bash
# 使用 Netplan
network:
  version: 2
  vlans:
    vlan100:
      id: 100
      link: eth0
      addresses:
        - 192.168.100.1/24
```

### 網路診斷腳本

```bash
#!/bin/bash
echo "=== 網路介面 ==="
ip addr show

echo "=== 路由表 ==="
ip route show

echo "=== DNS 設定 ==="
cat /etc/resolv.conf

echo "=== 測試連線 ==="
ping -c 2 8.8.8.8
ping -c 2 google.com
```

## 常用指令速查

```bash
# 查看網路資訊
ip addr show
ip route show
ip link show

# 設定 IP
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up

# 路由管理
sudo ip route add default via 192.168.1.1
sudo ip route del default

# 網路工具
ping google.com
traceroute google.com
dig google.com
nslookup google.com

# 查看連線
ss -tulpn
netstat -tulpn
```

## 相關檔案位置

- Netplan 設定：`/etc/netplan/`
- 傳統網路設定：`/etc/network/interfaces`
- DNS 設定：`/etc/resolv.conf`
- 主機名稱：`/etc/hostname`
- 主機對應：`/etc/hosts`
- 系統路由：`/proc/sys/net/`

## 參考資源

- Netplan 文件：https://netplan.io/
- ip 指令手冊：`man ip`
- 網路設定文件：`man interfaces`