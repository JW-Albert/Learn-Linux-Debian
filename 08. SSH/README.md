# SSH (Secure Shell) 遠端連線

## 簡介

SSH (Secure Shell) 是一個網路通訊協定，用於在不安全的網路環境中提供安全的遠端登入和命令執行。SSH 透過加密技術保護資料傳輸，是目前最常用的遠端管理工具。

### SSH 的特點

1. **加密通訊**：所有資料傳輸都經過加密
2. **身份驗證**：支援多種身份驗證方式
3. **埠轉發**：支援本地和遠端埠轉發
4. **檔案傳輸**：可以透過 SCP 和 SFTP 傳輸檔案
5. **跨平台**：支援 Linux、macOS、Windows 等系統

### SSH 的用途

- 遠端登入伺服器
- 遠端執行命令
- 安全檔案傳輸
- 埠轉發和隧道
- 自動化腳本執行

## 安裝 SSH

### 安裝 SSH 客戶端

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install openssh-client

# 檢查版本
ssh -V
```

### 安裝 SSH 伺服器

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install openssh-server

# 啟動 SSH 服務
sudo systemctl start ssh
sudo systemctl enable ssh

# 檢查服務狀態
sudo systemctl status ssh
```

## 基本使用

### 連線到遠端伺服器

```bash
# 基本連線（使用使用者名稱）
ssh username@hostname

# 使用 IP 位址
ssh username@192.168.1.100

# 指定埠號（預設為 22）
ssh -p 2222 username@hostname

# 使用 IPv6
ssh username@[2001:db8::1]
```

### 首次連線

首次連線到新主機時，SSH 會要求確認主機金鑰：

```
The authenticity of host 'hostname (192.168.1.100)' can't be established.
ECDSA key fingerprint is SHA256:xxxxx.
Are you sure you want to continue connecting (yes/no)?
```

輸入 `yes` 後，主機金鑰會被加入 `~/.ssh/known_hosts`。

### 連線選項

```bash
# 詳細模式（顯示連線過程）
ssh -v username@hostname

# 更詳細的模式
ssh -vv username@hostname
ssh -vvv username@hostname

# 執行遠端命令後退出
ssh username@hostname "ls -l"

# 強制使用 IPv4
ssh -4 username@hostname

# 強制使用 IPv6
ssh -6 username@hostname

# 指定身份檔案
ssh -i ~/.ssh/id_rsa username@hostname
```

## SSH 金鑰認證

### 為什麼使用金鑰認證

- **更安全**：不需要傳輸密碼
- **更方便**：不需要每次輸入密碼
- **自動化**：適合腳本和自動化任務

### 產生 SSH 金鑰對

```bash
# 產生 RSA 金鑰（2048 位元）
ssh-keygen -t rsa -b 2048

# 產生 RSA 金鑰（4096 位元，更安全）
ssh-keygen -t rsa -b 4096

# 產生 ECDSA 金鑰
ssh-keygen -t ecdsa -b 521

# 產生 Ed25519 金鑰（推薦，更快更安全）
ssh-keygen -t ed25519

# 指定金鑰檔案名稱
ssh-keygen -t ed25519 -f ~/.ssh/my_key

# 加入註解（通常是電子郵件）
ssh-keygen -t ed25519 -C "your.email@example.com"
```

**金鑰產生過程：**
1. 選擇儲存位置（預設：`~/.ssh/id_rsa`）
2. 設定密碼短語（可選，但建議設定）
3. 確認密碼短語

### 複製公鑰到遠端伺服器

```bash
# 使用 ssh-copy-id（最簡單）
ssh-copy-id username@hostname

# 指定金鑰
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@hostname

# 指定埠
ssh-copy-id -p 2222 username@hostname

# 手動複製
cat ~/.ssh/id_ed25519.pub | ssh username@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### 手動設定 authorized_keys

```bash
# 在遠端伺服器上
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# 將公鑰加入 authorized_keys
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys

# 設定正確的權限
chmod 600 ~/.ssh/authorized_keys
```

### 使用多個金鑰

```bash
# 編輯 ~/.ssh/config
nano ~/.ssh/config
```

**設定範例：**
```
Host server1
    HostName 192.168.1.100
    User username
    IdentityFile ~/.ssh/id_rsa_server1
    Port 22

Host server2
    HostName example.com
    User admin
    IdentityFile ~/.ssh/id_ed25519
    Port 2222
```

**使用方式：**
```bash
# 使用簡短名稱連線
ssh server1
ssh server2
```

## SSH 設定檔

### 客戶端設定檔

位置：`~/.ssh/config`

**常用設定：**
```
Host *
    # 使用特定金鑰
    IdentityFile ~/.ssh/id_ed25519
    
    # 關閉密碼認證（只使用金鑰）
    PasswordAuthentication no
    
    # 關閉主機金鑰檢查（不建議）
    # StrictHostKeyChecking no
    
    # 保持連線
    ServerAliveInterval 60
    ServerAliveCountMax 3

Host myserver
    HostName 192.168.1.100
    User username
    Port 22
    IdentityFile ~/.ssh/id_rsa_myserver
```

### 伺服器設定檔

位置：`/etc/ssh/sshd_config`

**重要設定：**
```bash
# 編輯設定檔
sudo nano /etc/ssh/sshd_config

# 修改後重新載入
sudo systemctl reload ssh
```

**常用設定選項：**
```
# 監聽埠（預設 22）
Port 22

# 允許 root 登入（建議設為 no）
PermitRootLogin no

# 允許密碼認證
PasswordAuthentication yes

# 允許金鑰認證
PubkeyAuthentication yes

# 允許的使用者（空白表示全部）
AllowUsers username1 username2

# 禁止的使用者
DenyUsers baduser

# 最大認證嘗試次數
MaxAuthTries 3

# 登入超時時間（秒）
LoginGraceTime 60

# 保持連線
ClientAliveInterval 60
ClientAliveCountMax 3
```

## 埠轉發

### 本地埠轉發

將本地埠轉發到遠端伺服器。

```bash
# 基本語法
ssh -L [本地埠]:[目標主機]:[目標埠] username@ssh_server

# 範例：將本地 8080 埠轉發到遠端 80 埠
ssh -L 8080:localhost:80 username@remote-server

# 轉發到其他主機
ssh -L 8080:192.168.1.50:80 username@ssh-server

# 背景執行
ssh -f -N -L 8080:localhost:80 username@remote-server
```

**選項說明：**
- `-L`：本地埠轉發
- `-f`：背景執行
- `-N`：不執行遠端命令

### 遠端埠轉發

將遠端埠轉發到本地。

```bash
# 基本語法
ssh -R [遠端埠]:[本地主機]:[本地埠] username@ssh_server

# 範例：將遠端 8080 埠轉發到本地 80 埠
ssh -R 8080:localhost:80 username@remote-server

# 背景執行
ssh -f -N -R 8080:localhost:80 username@remote-server
```

### 動態埠轉發（SOCKS 代理）

建立 SOCKS 代理。

```bash
# 建立 SOCKS5 代理（本地 1080 埠）
ssh -D 1080 username@remote-server

# 背景執行
ssh -f -N -D 1080 username@remote-server
```

**使用方式：**
- 在瀏覽器或應用程式中設定 SOCKS5 代理
- 代理伺服器：`127.0.0.1`
- 埠：`1080`

## 檔案傳輸

### SCP (Secure Copy)

使用 SCP 傳輸檔案。

```bash
# 複製檔案到遠端
scp file.txt username@hostname:/path/to/destination/

# 從遠端複製檔案
scp username@hostname:/path/to/file.txt ./

# 複製目錄（遞迴）
scp -r directory/ username@hostname:/path/to/destination/

# 指定埠
scp -P 2222 file.txt username@hostname:/path/

# 保留檔案屬性
scp -p file.txt username@hostname:/path/

# 顯示進度
scp -v file.txt username@hostname:/path/
```

### SFTP (SSH File Transfer Protocol)

互動式檔案傳輸。

```bash
# 連線到 SFTP 伺服器
sftp username@hostname

# SFTP 命令
sftp> ls                    # 列出遠端檔案
sftp> lls                   # 列出本地檔案
sftp> cd remote_dir         # 切換遠端目錄
sftp> lcd local_dir         # 切換本地目錄
sftp> put file.txt          # 上傳檔案
sftp> get file.txt          # 下載檔案
sftp> mkdir dir             # 建立遠端目錄
sftp> rm file.txt           # 刪除遠端檔案
sftp> exit                  # 退出
```

### rsync over SSH

使用 rsync 透過 SSH 同步檔案。

```bash
# 同步目錄到遠端
rsync -avz -e ssh local_dir/ username@hostname:/remote/dir/

# 從遠端同步
rsync -avz -e ssh username@hostname:/remote/dir/ local_dir/

# 排除特定檔案
rsync -avz -e ssh --exclude '*.log' local_dir/ username@hostname:/remote/dir/

# 顯示進度
rsync -avz -e ssh --progress local_dir/ username@hostname:/remote/dir/
```

## 進階功能

### SSH Agent

管理 SSH 金鑰，避免每次輸入密碼短語。

```bash
# 啟動 SSH Agent
eval "$(ssh-agent -s)"

# 加入金鑰
ssh-add ~/.ssh/id_ed25519

# 列出已加入的金鑰
ssh-add -l

# 刪除所有金鑰
ssh-add -D

# 永久加入（在 ~/.bashrc 中）
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
fi
```

### 執行遠端命令

```bash
# 執行單一命令
ssh username@hostname "ls -l"

# 執行多個命令
ssh username@hostname "cd /var/log && ls -l"

# 執行本地腳本
ssh username@hostname < script.sh

# 傳遞變數
ssh username@hostname "echo $HOME"
```

### SSH 隧道

建立安全的網路隧道。

```bash
# 建立隧道並保持連線
ssh -f -N -L 3306:localhost:3306 username@database-server

# 透過跳板機連線
ssh -J jump-server username@target-server

# 多層跳板
ssh -J jump1,jump2 username@target-server
```

## 安全最佳實踐

### 1. 使用金鑰認證

```bash
# 停用密碼認證（在 /etc/ssh/sshd_config）
PasswordAuthentication no
PubkeyAuthentication yes
```

### 2. 變更預設埠

```bash
# 在 /etc/ssh/sshd_config
Port 2222
```

### 3. 限制使用者

```bash
# 只允許特定使用者
AllowUsers username1 username2

# 禁止特定使用者
DenyUsers baduser
```

### 4. 設定防火牆

```bash
# 只允許特定 IP 連線
# 使用 UFW 或 iptables
```

### 5. 定期更新

```bash
# 更新 SSH 套件
sudo apt update && sudo apt upgrade openssh-server
```

### 6. 監控登入

```bash
# 查看登入記錄
sudo lastlog
sudo last

# 查看失敗的登入嘗試
sudo grep "Failed password" /var/log/auth.log
```

## 故障排除

### 連線問題

```bash
# 使用詳細模式診斷
ssh -vvv username@hostname

# 檢查服務狀態
sudo systemctl status ssh

# 檢查埠是否開啟
sudo netstat -tlnp | grep :22
sudo ss -tlnp | grep :22

# 檢查防火牆
sudo ufw status
```

### 金鑰問題

```bash
# 檢查金鑰權限
ls -l ~/.ssh/

# 修正權限
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys

# 測試金鑰
ssh-keygen -y -f ~/.ssh/id_rsa
```

### 主機金鑰問題

```bash
# 移除舊的主機金鑰
ssh-keygen -R hostname
ssh-keygen -R 192.168.1.100

# 手動編輯 known_hosts
nano ~/.ssh/known_hosts
```

## 常用指令速查

```bash
# 基本連線
ssh username@hostname
ssh -p 2222 username@hostname

# 金鑰管理
ssh-keygen -t ed25519
ssh-copy-id username@hostname
ssh-add ~/.ssh/id_ed25519

# 檔案傳輸
scp file.txt username@hostname:/path/
scp -r dir/ username@hostname:/path/
rsync -avz -e ssh local/ username@hostname:/remote/

# 埠轉發
ssh -L 8080:localhost:80 username@hostname
ssh -R 8080:localhost:80 username@hostname
ssh -D 1080 username@hostname

# 執行命令
ssh username@hostname "command"
```

## 參考資源

- OpenSSH 官方文件：https://www.openssh.com/
- SSH 設定檔說明：`man ssh_config`
- SSH 伺服器設定：`man sshd_config`