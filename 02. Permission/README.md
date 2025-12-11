# Permission Control

## 簡介

Linux 是一個多使用者作業系統，權限管理是系統安全的核心機制。透過權限控制，可以限制使用者對檔案和目錄的存取，保護系統資源不被未授權的使用者修改或刪除。

## 使用者與群組

### 使用者 (User)

每個 Linux 系統都有多個使用者帳號，每個使用者都有唯一的識別碼（UID）。系統會為每個使用者建立家目錄，通常位於 `/home/使用者名稱`。

### 群組 (Group)

群組是使用者的集合，用於簡化權限管理。一個使用者可以屬於多個群組，但只有一個主要群組（primary group）。

### 查看目前使用者資訊

```bash
# 查看目前使用者
whoami

# 查看目前使用者的 UID 和 GID
id

# 查看目前使用者所屬的群組
groups
```

## Root 使用者

### 什麼是 Root

Root 是 Linux 系統中的超級使用者（superuser），擁有系統的完全控制權限。Root 使用者的 UID 為 0，可以執行任何操作，包括：
- 修改系統檔案
- 安裝或移除軟體
- 變更任何檔案的權限
- 管理使用者帳號

### Root 的危險性

由於 root 擁有無限權限，直接以 root 身份操作系統是非常危險的。一個錯誤的指令可能導致：
- 系統檔案被刪除
- 系統設定被破壞
- 資料遺失

因此，應該避免直接以 root 身份登入，而是使用 `sudo` 機制來執行需要特權的操作。

## Sudo 機制

### 什麼是 Sudo

Sudo (Super User Do) 允許一般使用者以 root 權限執行特定指令，而不需要知道 root 的密碼。這提供了更安全的權限管理方式。

### Sudo 的優點

1. 不需要分享 root 密碼
2. 可以記錄誰執行了哪些指令
3. 可以限制特定使用者只能執行特定指令
4. 提供更細緻的權限控制

### 使用 Sudo

```bash
# 以 root 權限執行單一指令
sudo <指令>

# 以 root 權限進入互動式 shell（不建議）
sudo -i

# 以 root 權限執行 shell（不建議）
sudo -s
```

範例：
```bash
# 安裝套件需要 root 權限
sudo apt install vim

# 編輯系統設定檔需要 root 權限
sudo nano /etc/hosts
```

### Sudoers 設定檔

Sudo 的設定檔位於 `/etc/sudoers`，定義了哪些使用者可以使用 sudo，以及可以執行哪些指令。

**警告：** 不要直接編輯 `/etc/sudoers`，應使用 `visudo` 指令：

```bash
sudo visudo
```

`visudo` 會檢查語法錯誤，避免設定錯誤導致無法使用 sudo。

### 將使用者加入 Sudo 群組

在 Debian 系統中，屬於 `sudo` 群組的使用者可以使用 sudo：

```bash
# 將使用者加入 sudo 群組（需要 root 權限）
sudo usermod -aG sudo <使用者名稱>

# 查看使用者所屬的群組
groups <使用者名稱>
```

## 檔案權限

### 權限類型

Linux 檔案系統為每個檔案和目錄定義了三種權限：

1. **讀取 (Read, r)**：可以查看檔案內容或列出目錄內容
2. **寫入 (Write, w)**：可以修改檔案內容或在目錄中建立/刪除檔案
3. **執行 (Execute, x)**：可以執行檔案（程式或腳本）或進入目錄

### 權限對象

權限針對三種對象設定：

1. **擁有者 (Owner, u)**：檔案或目錄的擁有者
2. **群組 (Group, g)**：檔案或目錄所屬的群組
3. **其他人 (Others, o)**：既不是擁有者也不屬於該群組的使用者

### 查看檔案權限

使用 `ls -l` 指令查看檔案權限：

```bash
ls -l
```

輸出範例：
```
-rw-r--r-- 1 user group 1024 Jan 1 12:00 file.txt
drwxr-xr-x 2 user group 4096 Jan 1 12:00 directory
```

權限欄位的說明：
- 第 1 個字元：檔案類型（`-` 為一般檔案，`d` 為目錄）
- 第 2-4 個字元：擁有者權限（rwx）
- 第 5-7 個字元：群組權限（rwx）
- 第 8-10 個字元：其他人權限（rwx）

### 符號表示法

權限可以用符號表示：
- `r`：讀取權限（數字 4）
- `w`：寫入權限（數字 2）
- `x`：執行權限（數字 1）
- `-`：沒有該權限（數字 0）

範例：
- `rwxr-xr--`：擁有者可以讀寫執行，群組可以讀取執行，其他人只能讀取
- `rw-------`：只有擁有者可以讀寫，其他人沒有任何權限

### 數字表示法

權限也可以用三位數字表示，每位數字是權限值的總和：
- 讀取 (r) = 4
- 寫入 (w) = 2
- 執行 (x) = 1

計算方式：
- `rwx` = 4 + 2 + 1 = 7
- `rw-` = 4 + 2 + 0 = 6
- `r-x` = 4 + 0 + 1 = 5
- `r--` = 4 + 0 + 0 = 4
- `---` = 0 + 0 + 0 = 0

常見權限組合：
- `755`：`rwxr-xr-x`（擁有者完全權限，群組和其他人可讀取執行）
- `644`：`rw-r--r--`（擁有者可讀寫，群組和其他人只能讀取）
- `600`：`rw-------`（只有擁有者可讀寫）
- `777`：`rwxrwxrwx`（所有人完全權限，不建議使用）

## 變更檔案權限

### chmod 指令

`chmod` (change mode) 用於變更檔案或目錄的權限。

#### 使用數字表示法

```bash
# 設定檔案權限為 644
chmod 644 file.txt

# 設定目錄權限為 755
chmod 755 directory

# 遞迴變更目錄及其內容的權限
chmod -R 755 directory
```

#### 使用符號表示法

```bash
# 為擁有者添加執行權限
chmod u+x file.txt

# 為群組添加寫入權限
chmod g+w file.txt

# 移除其他人的讀取權限
chmod o-r file.txt

# 設定擁有者為讀寫，群組為讀取，其他人無權限
chmod u=rw,g=r,o= file.txt

# 為所有人添加執行權限
chmod a+x file.txt

# 遞迴變更
chmod -R u+x directory
```

### chown 指令

`chown` (change owner) 用於變更檔案或目錄的擁有者和群組。

```bash
# 變更檔案擁有者
sudo chown user file.txt

# 變更檔案擁有者和群組
sudo chown user:group file.txt

# 只變更群組
sudo chown :group file.txt

# 遞迴變更目錄及其內容
sudo chown -R user:group directory
```

### chgrp 指令

`chgrp` (change group) 用於變更檔案或目錄的群組。

```bash
# 變更檔案群組
sudo chgrp group file.txt

# 遞迴變更目錄及其內容的群組
sudo chgrp -R group directory
```

## 預設權限與 Umask

### 預設權限

當建立新檔案或目錄時，系統會根據 umask 值來設定預設權限。

- 新檔案的預設權限：`666` (rw-rw-rw-)
- 新目錄的預設權限：`777` (rwxrwxrwx)

### Umask

Umask 是一個遮罩值，用於從預設權限中移除某些權限。

```bash
# 查看目前的 umask
umask

# 設定 umask（例如 022）
umask 022
```

Umask 的計算方式：
- 檔案：預設權限 (666) - umask = 實際權限
- 目錄：預設權限 (777) - umask = 實際權限

範例（umask = 022）：
- 新檔案：666 - 022 = 644 (rw-r--r--)
- 新目錄：777 - 022 = 755 (rwxr-xr-x)

常見的 umask 值：
- `000`：所有人完全權限（不安全）
- `022`：群組和其他人無寫入權限（常用）
- `027`：群組無寫入，其他人無任何權限
- `077`：只有擁有者有權限

## 特殊權限

### SUID (Set User ID)

當檔案設定 SUID 權限時，執行該檔案的使用者會暫時獲得檔案擁有者的權限。

```bash
# 設定 SUID（在執行權限位置顯示為 s）
chmod u+s file
chmod 4755 file  # 4 表示 SUID

# 移除 SUID
chmod u-s file
```

範例：`/usr/bin/passwd` 通常設定 SUID，讓一般使用者可以修改自己的密碼。

### SGID (Set Group ID)

當檔案或目錄設定 SGID 權限時：
- 檔案：執行時會以檔案群組的權限執行
- 目錄：在該目錄中建立的新檔案會繼承目錄的群組

```bash
# 設定 SGID
chmod g+s file
chmod 2755 file  # 2 表示 SGID

# 移除 SGID
chmod g-s file
```

### Sticky Bit

Sticky bit 主要用於目錄，設定後只有檔案擁有者、目錄擁有者或 root 可以刪除該目錄中的檔案。

```bash
# 設定 Sticky bit
chmod +t directory
chmod 1755 directory  # 1 表示 Sticky bit

# 移除 Sticky bit
chmod -t directory
```

範例：`/tmp` 目錄通常設定 Sticky bit，讓所有使用者可以建立檔案，但只能刪除自己的檔案。

## 實用範例

### 保護敏感檔案

```bash
# 設定只有擁有者可讀寫
chmod 600 secret.txt

# 變更擁有者為 root
sudo chown root:root secret.txt
```

### 共享目錄設定

```bash
# 建立共享目錄，群組成員可以讀寫
mkdir shared
chmod 775 shared
chgrp developers shared

# 設定 SGID，讓新檔案繼承群組
chmod g+s shared
```

### 腳本檔案權限

```bash
# 建立腳本並設定執行權限
nano script.sh
chmod +x script.sh
# 或
chmod 755 script.sh
```

### 備份重要檔案

```bash
# 備份並設定適當權限
cp important.txt important.txt.backup
chmod 644 important.txt.backup
```

## 注意事項

1. 使用 `sudo` 時要特別小心，確認指令正確後再執行
2. 不要將敏感檔案的權限設為 `777`，這會讓所有人都有完全控制權
3. 定期檢查重要檔案的權限設定
4. 使用 `chmod -R` 時要確認路徑正確，避免誤改整個系統的權限
5. 理解權限的繼承關係，特別是目錄權限對其內容的影響

## 相關指令速查

```bash
# 查看目前使用者
whoami
id

# 查看檔案權限
ls -l
ls -la  # 包含隱藏檔案

# 變更權限
chmod <權限> <檔案>
chown <使用者>:<群組> <檔案>
chgrp <群組> <檔案>

# 查看和設定 umask
umask
umask <值>

# 以其他使用者身份執行指令
sudo -u <使用者> <指令>
```