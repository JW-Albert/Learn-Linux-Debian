# Linux 網路服務配置

## 簡介

Linux 系統常用於部署各種網路服務，如 Web 伺服器、資料庫伺服器、郵件伺服器等。本教學將介紹如何在 Debian 系統上配置和管理常見的網路服務。

## Web 伺服器

### Nginx

Nginx 是一個高效能的 Web 伺服器和反向代理伺服器。

#### 安裝 Nginx

```bash
# 安裝 Nginx
sudo apt update
sudo apt install nginx

# 啟動服務
sudo systemctl start nginx

# 啟用開機自動啟動
sudo systemctl enable nginx

# 檢查狀態
sudo systemctl status nginx
```

#### 基本配置

```bash
# 主設定檔
sudo nano /etc/nginx/nginx.conf

# 站點配置目錄
ls /etc/nginx/sites-available/
ls /etc/nginx/sites-enabled/

# 建立站點配置
sudo nano /etc/nginx/sites-available/my-site
```

**基本站點配置範例：**
```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    
    root /var/www/html;
    index index.html index.htm;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
```

#### 啟用站點

```bash
# 建立符號連結啟用站點
sudo ln -s /etc/nginx/sites-available/my-site /etc/nginx/sites-enabled/

# 測試配置
sudo nginx -t

# 重新載入配置
sudo systemctl reload nginx
```

#### 常用指令

```bash
# 測試配置
sudo nginx -t

# 重新載入配置（不中斷服務）
sudo systemctl reload nginx

# 重新啟動服務
sudo systemctl restart nginx

# 查看錯誤日誌
sudo tail -f /var/log/nginx/error.log

# 查看存取日誌
sudo tail -f /var/log/nginx/access.log
```

### Apache

Apache 是另一個流行的 Web 伺服器。

#### 安裝 Apache

```bash
# 安裝 Apache
sudo apt update
sudo apt install apache2

# 啟動服務
sudo systemctl start apache2

# 啟用開機自動啟動
sudo systemctl enable apache2
```

#### 基本配置

```bash
# 主設定檔
sudo nano /etc/apache2/apache2.conf

# 站點配置目錄
ls /etc/apache2/sites-available/
ls /etc/apache2/sites-enabled/

# 建立站點配置
sudo nano /etc/apache2/sites-available/my-site.conf
```

**基本站點配置範例：**
```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    
    DocumentRoot /var/www/html
    
    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

#### 啟用站點和模組

```bash
# 啟用站點
sudo a2ensite my-site.conf

# 停用站點
sudo a2dissite my-site.conf

# 啟用模組
sudo a2enmod rewrite
sudo a2enmod ssl

# 測試配置
sudo apache2ctl configtest

# 重新載入配置
sudo systemctl reload apache2
```

## 資料庫伺服器

### MySQL / MariaDB

#### 安裝 MySQL

```bash
# 安裝 MySQL
sudo apt update
sudo apt install mysql-server

# 啟動服務
sudo systemctl start mysql

# 啟用開機自動啟動
sudo systemctl enable mysql

# 安全設定（首次安裝後執行）
sudo mysql_secure_installation
```

#### 基本操作

```bash
# 登入 MySQL
sudo mysql
# 或
mysql -u root -p

# 建立資料庫
CREATE DATABASE mydb;

# 建立使用者
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'password';

# 授予權限
GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;

# 查看資料庫
SHOW DATABASES;

# 退出
EXIT;
```

#### 配置檔案

```bash
# MySQL 設定檔
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

# 常用設定
[mysqld]
bind-address = 127.0.0.1  # 只允許本地連線
# bind-address = 0.0.0.0  # 允許所有 IP 連線
port = 3306
max_connections = 200
```

#### 備份與還原

```bash
# 備份資料庫
mysqldump -u root -p mydb > backup.sql

# 還原資料庫
mysql -u root -p mydb < backup.sql

# 備份所有資料庫
mysqldump -u root -p --all-databases > all_backup.sql
```

### PostgreSQL

#### 安裝 PostgreSQL

```bash
# 安裝 PostgreSQL
sudo apt update
sudo apt install postgresql postgresql-contrib

# 啟動服務
sudo systemctl start postgresql

# 啟用開機自動啟動
sudo systemctl enable postgresql
```

#### 基本操作

```bash
# 切換到 postgres 使用者
sudo -u postgres psql

# 建立資料庫
CREATE DATABASE mydb;

# 建立使用者
CREATE USER myuser WITH PASSWORD 'password';

# 授予權限
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;

# 查看資料庫
\l

# 退出
\q
```

#### 配置檔案

```bash
# PostgreSQL 設定檔
sudo nano /etc/postgresql/14/main/postgresql.conf

# 連線設定檔
sudo nano /etc/postgresql/14/main/pg_hba.conf
```

## 郵件伺服器

### Postfix

Postfix 是一個流行的郵件傳輸代理（MTA）。

#### 安裝 Postfix

```bash
# 安裝 Postfix
sudo apt update
sudo apt install postfix

# 安裝過程會提示選擇配置類型
# 選擇 "Internet Site" 用於完整郵件伺服器
# 選擇 "Local only" 僅用於本地郵件
```

#### 基本配置

```bash
# 主設定檔
sudo nano /etc/postfix/main.cf

# 基本設定
myhostname = mail.example.com
mydomain = example.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, $mydomain
```

#### 重新載入配置

```bash
# 測試配置
sudo postfix check

# 重新載入配置
sudo systemctl reload postfix

# 查看郵件佇列
sudo mailq

# 查看日誌
sudo tail -f /var/log/mail.log
```

## 檔案共享服務

### Samba

Samba 提供 Windows 相容的檔案共享服務。

#### 安裝 Samba

```bash
# 安裝 Samba
sudo apt update
sudo apt install samba

# 啟動服務
sudo systemctl start smbd
sudo systemctl start nmbd

# 啟用開機自動啟動
sudo systemctl enable smbd
sudo systemctl enable nmbd
```

#### 配置 Samba

```bash
# 編輯設定檔
sudo nano /etc/samba/smb.conf
```

**基本共享配置範例：**
```ini
[global]
workgroup = WORKGROUP
server string = Samba Server
security = user

[shared]
path = /srv/shared
browsable = yes
writable = yes
valid users = user1, user2
```

#### 建立 Samba 使用者

```bash
# 建立 Samba 使用者（必須先有系統使用者）
sudo smbpasswd -a username

# 啟用使用者
sudo smbpasswd -e username

# 刪除使用者
sudo smbpasswd -x username
```

#### 重新載入配置

```bash
# 測試配置
sudo testparm

# 重新載入配置
sudo systemctl reload smbd
```

### NFS

NFS（Network File System）用於 Unix/Linux 系統間的檔案共享。

#### 安裝 NFS 伺服器

```bash
# 安裝 NFS 伺服器
sudo apt update
sudo apt install nfs-kernel-server

# 啟動服務
sudo systemctl start nfs-kernel-server

# 啟用開機自動啟動
sudo systemctl enable nfs-kernel-server
```

#### 配置 NFS

```bash
# 編輯匯出設定檔
sudo nano /etc/exports
```

**設定範例：**
```
/srv/shared 192.168.1.0/24(rw,sync,no_subtree_check)
/home/user1 192.168.1.100(rw,sync,no_root_squash)
```

**選項說明：**
- `rw`：讀寫權限
- `ro`：唯讀權限
- `sync`：同步寫入
- `no_subtree_check`：不檢查子目錄
- `no_root_squash`：允許 root 存取

#### 套用配置

```bash
# 重新載入匯出表
sudo exportfs -ra

# 查看匯出的目錄
sudo exportfs -v

# 重新啟動服務
sudo systemctl restart nfs-kernel-server
```

#### 在客戶端掛載 NFS

```bash
# 安裝 NFS 客戶端
sudo apt install nfs-common

# 掛載 NFS 共享
sudo mount -t nfs server:/srv/shared /mnt/nfs

# 永久掛載（加入 /etc/fstab）
server:/srv/shared /mnt/nfs nfs defaults 0 0
```

## DNS 伺服器

### BIND9

BIND9 是常用的 DNS 伺服器軟體。

#### 安裝 BIND9

```bash
# 安裝 BIND9
sudo apt update
sudo apt install bind9 bind9utils bind9-doc

# 啟動服務
sudo systemctl start bind9

# 啟用開機自動啟動
sudo systemctl enable bind9
```

#### 基本配置

```bash
# 主設定檔
sudo nano /etc/bind/named.conf.options

# 區域設定檔
sudo nano /etc/bind/named.conf.local

# 建立區域檔案
sudo nano /etc/bind/db.example.com
```

**基本區域檔案範例：**
```
$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
@       IN      A       192.168.1.100
ns1     IN      A       192.168.1.100
www     IN      A       192.168.1.100
```

#### 測試 DNS

```bash
# 測試配置
sudo named-checkconf
sudo named-checkzone example.com /etc/bind/db.example.com

# 重新載入配置
sudo systemctl reload bind9

# 測試 DNS 查詢
dig @localhost example.com
nslookup example.com localhost
```

## FTP 伺服器

### vsftpd

vsftpd 是一個安全、快速的 FTP 伺服器。

#### 安裝 vsftpd

```bash
# 安裝 vsftpd
sudo apt update
sudo apt install vsftpd

# 啟動服務
sudo systemctl start vsftpd

# 啟用開機自動啟動
sudo systemctl enable vsftpd
```

#### 配置 vsftpd

```bash
# 編輯設定檔
sudo nano /etc/vsftpd.conf
```

**常用設定：**
```ini
# 允許本地使用者登入
local_enable=YES

# 允許寫入
write_enable=YES

# 本地使用者家目錄限制
chroot_local_user=YES

# 允許被動模式
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=50000

# 日誌設定
xferlog_enable=YES
```

#### 重新載入配置

```bash
# 重新載入配置
sudo systemctl restart vsftpd

# 查看日誌
sudo tail -f /var/log/vsftpd.log
```

## 服務管理最佳實踐

### 安全性設定

```bash
# 1. 只開放必要的埠
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp

# 2. 限制服務存取來源
# 在服務設定檔中限制允許的 IP

# 3. 使用強密碼
# 為所有服務設定強密碼

# 4. 定期更新
sudo apt update && sudo apt upgrade
```

### 監控服務

```bash
# 監控服務狀態
watch -n 1 'systemctl status nginx mysql'

# 監控日誌
sudo tail -f /var/log/nginx/error.log
sudo journalctl -u nginx -f

# 監控資源使用
top
htop
```

### 備份配置

```bash
# 備份 Nginx 配置
sudo tar -czf nginx-backup.tar.gz /etc/nginx/

# 備份 MySQL 資料
mysqldump -u root -p --all-databases > mysql-backup.sql

# 備份所有服務配置
sudo tar -czf services-config-backup.tar.gz /etc/nginx/ /etc/apache2/ /etc/mysql/
```

## 常用指令速查

```bash
# 服務管理
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name
sudo systemctl status service-name
sudo systemctl reload service-name

# Nginx
sudo nginx -t
sudo systemctl reload nginx

# Apache
sudo apache2ctl configtest
sudo systemctl reload apache2

# MySQL
sudo mysql
mysql -u user -p

# PostgreSQL
sudo -u postgres psql

# Samba
sudo testparm
sudo systemctl reload smbd

# NFS
sudo exportfs -ra
sudo showmount -e

# BIND9
sudo named-checkconf
sudo systemctl reload bind9
```

## 相關檔案位置

- Nginx：`/etc/nginx/`
- Apache：`/etc/apache2/`
- MySQL：`/etc/mysql/`
- PostgreSQL：`/etc/postgresql/`
- Postfix：`/etc/postfix/`
- Samba：`/etc/samba/`
- NFS：`/etc/exports`
- BIND9：`/etc/bind/`
- vsftpd：`/etc/vsftpd.conf`

## 參考資源

- Nginx 文件：https://nginx.org/en/docs/
- Apache 文件：https://httpd.apache.org/docs/
- MySQL 文件：https://dev.mysql.com/doc/
- PostgreSQL 文件：https://www.postgresql.org/docs/
- Samba 文件：https://www.samba.org/samba/docs/