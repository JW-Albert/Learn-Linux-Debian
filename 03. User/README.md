# User and Group Manager

## 簡介

Linux 是多使用者作業系統，每個使用者都有獨立的帳號、家目錄和權限設定。群組則是用來組織使用者，簡化權限管理的機制。本教學將說明如何建立、管理和刪除使用者與群組。

## 使用者帳號管理

### 查看使用者資訊

```bash
# 查看目前登入的使用者
whoami

# 查看目前使用者的詳細資訊
id

# 查看所有使用者列表
cat /etc/passwd

# 查看特定使用者的資訊
id <使用者名稱>

# 查看使用者的登入記錄
last <使用者名稱>
```

### 建立新使用者

使用 `useradd` 或 `adduser` 指令建立新使用者。

#### useradd 指令

`useradd` 是較低階的指令，需要手動設定密碼和其他選項。

```bash
# 建立基本使用者
sudo useradd <使用者名稱>

# 建立使用者並指定家目錄
sudo useradd -d /home/<使用者名稱> <使用者名稱>

# 建立使用者並指定主要群組
sudo useradd -g <群組名稱> <使用者名稱>

# 建立使用者並指定額外群組
sudo useradd -G <群組1,群組2> <使用者名稱>

# 建立使用者並指定 UID
sudo useradd -u <UID> <使用者名稱>

# 建立使用者並指定 shell
sudo useradd -s /bin/bash <使用者名稱>

# 建立系統使用者（無登入 shell，通常用於服務）
sudo useradd -r -s /usr/sbin/nologin <使用者名稱>

# 完整範例
sudo useradd -m -d /home/newuser -s /bin/bash -G sudo,users newuser
```

常用選項說明：
- `-m`：自動建立家目錄
- `-d`：指定家目錄路徑
- `-s`：指定登入 shell
- `-g`：指定主要群組（GID）
- `-G`：指定額外群組
- `-u`：指定使用者 ID (UID)
- `-r`：建立系統使用者
- `-c`：添加註解（通常是全名）

#### adduser 指令

`adduser` 是較高階的互動式指令，會自動建立家目錄並提示設定密碼。

```bash
# 互動式建立使用者
sudo adduser <使用者名稱>
```

`adduser` 會自動：
- 建立家目錄
- 設定基本群組
- 提示輸入密碼
- 提示輸入使用者資訊（全名、電話等）

### 設定使用者密碼

```bash
# 為使用者設定密碼
sudo passwd <使用者名稱>

# 變更自己的密碼
passwd

# 鎖定使用者帳號（禁止登入）
sudo passwd -l <使用者名稱>

# 解鎖使用者帳號
sudo passwd -u <使用者名稱>

# 刪除使用者密碼（允許無密碼登入，不安全）
sudo passwd -d <使用者名稱>
```

### 修改使用者資訊

使用 `usermod` 指令修改已存在使用者的設定。

```bash
# 變更使用者名稱
sudo usermod -l <新名稱> <舊名稱>

# 變更使用者 UID
sudo usermod -u <新UID> <使用者名稱>

# 變更主要群組
sudo usermod -g <群組名稱> <使用者名稱>

# 添加額外群組（保留原有群組）
sudo usermod -aG <群組名稱> <使用者名稱>

# 變更額外群組（取代原有群組）
sudo usermod -G <群組1,群組2> <使用者名稱>

# 變更家目錄
sudo usermod -d /home/<新目錄> <使用者名稱>

# 變更家目錄並移動現有檔案
sudo usermod -d /home/<新目錄> -m <使用者名稱>

# 變更登入 shell
sudo usermod -s /bin/bash <使用者名稱>

# 鎖定帳號（在 /etc/passwd 中標記）
sudo usermod -L <使用者名稱>

# 解鎖帳號
sudo usermod -U <使用者名稱>

# 設定帳號過期日期
sudo usermod -e 2024-12-31 <使用者名稱>

# 移除帳號過期日期
sudo usermod -e "" <使用者名稱>
```

### 刪除使用者

```bash
# 刪除使用者（保留家目錄）
sudo userdel <使用者名稱>

# 刪除使用者並移除家目錄
sudo userdel -r <使用者名稱>

# 強制刪除（即使使用者正在使用中）
sudo userdel -f <使用者名稱>
```

### 切換使用者

```bash
# 切換到其他使用者（需要該使用者密碼）
su <使用者名稱>

# 切換到 root
su
# 或
su -

# 切換使用者並執行指令
su - <使用者名稱> -c "<指令>"

# 以其他使用者身份執行指令（使用 sudo）
sudo -u <使用者名稱> <指令>
```

## 群組管理

### 查看群組資訊

```bash
# 查看所有群組
cat /etc/group

# 查看目前使用者所屬的群組
groups

# 查看特定使用者所屬的群組
groups <使用者名稱>

# 查看群組的詳細資訊
getent group <群組名稱>
```

### 建立新群組

```bash
# 建立基本群組
sudo groupadd <群組名稱>

# 建立群組並指定 GID
sudo groupadd -g <GID> <群組名稱>

# 建立系統群組
sudo groupadd -r <群組名稱>
```

### 修改群組資訊

```bash
# 變更群組名稱
sudo groupmod -n <新名稱> <舊名稱>

# 變更群組 GID
sudo groupmod -g <新GID> <群組名稱>
```

### 刪除群組

```bash
# 刪除群組
sudo groupdel <群組名稱>
```

注意：如果群組是某個使用者的主要群組，則無法刪除該群組。

### 管理群組成員

```bash
# 將使用者加入群組
sudo usermod -aG <群組名稱> <使用者名稱>

# 將使用者從群組中移除（需要編輯 /etc/group 或使用 gpasswd）
sudo gpasswd -d <使用者名稱> <群組名稱>

# 設定群組管理員
sudo gpasswd -A <使用者名稱> <群組名稱>

# 查看群組成員
getent group <群組名稱>
```

## 相關系統檔案

### /etc/passwd

儲存使用者帳號資訊，每行格式為：
```
使用者名稱:密碼(x表示加密):UID:GID:註解:家目錄:登入shell
```

範例：
```
albert:x:1000:1000:Albert User:/home/albert:/bin/bash
```

### /etc/shadow

儲存加密的使用者密碼，只有 root 可以讀取。

每行格式為：
```
使用者名稱:加密密碼:最後修改日期:最小天數:最大天數:警告天數:失效天數:過期日期:保留欄位
```

### /etc/group

儲存群組資訊，每行格式為：
```
群組名稱:密碼(x或!):GID:成員列表
```

範例：
```
sudo:x:27:albert,user1
developers:x:1001:albert,user2,user3
```

### /etc/gshadow

儲存群組密碼資訊（較少使用）。

## 使用者與群組 ID

### UID (User ID)

- 0：root 使用者
- 1-999：系統使用者（通常用於服務）
- 1000+：一般使用者

### GID (Group ID)

- 0：root 群組
- 1-999：系統群組
- 1000+：一般群組

### 查看 ID 範圍

```bash
# 查看 UID 範圍設定
grep UID /etc/login.defs

# 查看 GID 範圍設定
grep GID /etc/login.defs
```

## 實用範例

### 建立開發者使用者

```bash
# 建立開發者群組
sudo groupadd developers

# 建立使用者並加入開發者群組
sudo useradd -m -G developers -s /bin/bash developer1

# 設定密碼
sudo passwd developer1

# 驗證設定
id developer1
groups developer1
```

### 建立系統服務使用者

```bash
# 建立系統使用者（無登入 shell）
sudo useradd -r -s /usr/sbin/nologin -d /var/lib/myservice myservice

# 建立服務目錄
sudo mkdir -p /var/lib/myservice
sudo chown myservice:myservice /var/lib/myservice
```

### 管理群組成員

```bash
# 將多個使用者加入群組
sudo usermod -aG developers user1
sudo usermod -aG developers user2
sudo usermod -aG developers user3

# 查看群組成員
getent group developers

# 移除群組成員
sudo gpasswd -d user3 developers
```

### 變更使用者家目錄

```bash
# 備份舊家目錄
sudo cp -r /home/olduser /home/olduser.backup

# 變更家目錄並移動檔案
sudo usermod -d /home/newuser -m olduser

# 驗證
ls -la /home/newuser
```

### 批量建立使用者

```bash
# 建立多個使用者
for user in user1 user2 user3; do
    sudo useradd -m -s /bin/bash $user
    echo "$user:password123" | sudo chpasswd
done
```

### 查看使用者活動

```bash
# 查看目前登入的使用者
who
w

# 查看使用者最後登入時間
lastlog

# 查看特定使用者的登入記錄
last <使用者名稱>

# 查看失敗的登入嘗試
sudo lastb
```

## 安全最佳實踐

### 密碼政策

```bash
# 安裝密碼品質檢查工具
sudo apt install libpam-pwquality

# 設定密碼政策（編輯 /etc/pam.d/common-password）
# 設定最小長度、複雜度要求等
```

### 帳號鎖定

```bash
# 鎖定帳號（禁止登入）
sudo usermod -L <使用者名稱>
sudo passwd -l <使用者名稱>

# 解鎖帳號
sudo usermod -U <使用者名稱>
sudo passwd -u <使用者名稱>
```

### 定期檢查

```bash
# 檢查沒有密碼的使用者
sudo awk -F: '($2 == "") {print $1}' /etc/shadow

# 檢查 UID 為 0 的使用者（除了 root）
sudo awk -F: '($3 == 0) {print $1}' /etc/passwd

# 檢查長期未使用的帳號
sudo lastlog | grep "Never logged in"
```

## 常用指令速查

### 使用者管理

```bash
# 建立使用者
sudo useradd -m <使用者名稱>
sudo adduser <使用者名稱>

# 刪除使用者
sudo userdel -r <使用者名稱>

# 修改使用者
sudo usermod <選項> <使用者名稱>

# 設定密碼
sudo passwd <使用者名稱>

# 查看使用者資訊
id <使用者名稱>
whoami
```

### 群組管理

```bash
# 建立群組
sudo groupadd <群組名稱>

# 刪除群組
sudo groupdel <群組名稱>

# 修改群組
sudo groupmod <選項> <群組名稱>

# 管理群組成員
sudo usermod -aG <群組> <使用者>
sudo gpasswd -d <使用者> <群組>
```

### 查看資訊

```bash
# 查看使用者
cat /etc/passwd
id <使用者名稱>
whoami
who
w

# 查看群組
cat /etc/group
groups
getent group <群組名稱>
```

## 注意事項

1. 建立使用者時，建議使用 `-m` 選項自動建立家目錄
2. 刪除使用者前，確認已備份重要資料
3. 系統使用者（UID < 1000）通常不應有登入 shell
4. 定期檢查並清理未使用的帳號
5. 使用強密碼政策保護系統安全
6. 避免直接編輯 `/etc/passwd` 和 `/etc/group`，使用專用指令更安全
7. 變更使用者 UID 或 GID 時，需要同時變更相關檔案的擁有者
8. 將使用者加入 sudo 群組時要謹慎，確保使用者值得信任