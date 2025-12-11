# C/C++/Python 編譯環境

## 簡介

在 Linux 系統上開發程式需要安裝相應的編譯器和開發工具。本教學將介紹如何在 Debian 系統上安裝和配置 C、C++ 和 Python 的開發環境。

## C/C++ 編譯環境

### GCC 編譯器

GCC（GNU Compiler Collection）是 Linux 系統上最常用的 C/C++ 編譯器。

#### 安裝 GCC

```bash
# 安裝基本編譯工具
sudo apt update
sudo apt install build-essential

# build-essential 包含：
# - gcc (C 編譯器)
# - g++ (C++ 編譯器)
# - make (建置工具)
# - libc6-dev (C 標準函式庫)

# 檢查版本
gcc --version
g++ --version
make --version
```

#### 安裝特定版本的 GCC

```bash
# 查看可用的 GCC 版本
apt-cache search gcc- | grep "^gcc-"

# 安裝特定版本（例如 GCC 11）
sudo apt install gcc-11 g++-11

# 設定為預設版本
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 100

# 選擇預設版本
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```

### 基本編譯

#### C 程式編譯

```bash
# 建立簡單的 C 程式
nano hello.c
```

**hello.c：**
```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

```bash
# 編譯 C 程式
gcc hello.c -o hello

# 執行
./hello

# 編譯並指定輸出檔名
gcc hello.c -o myprogram

# 編譯時顯示警告
gcc -Wall hello.c -o hello

# 編譯時加入除錯資訊
gcc -g hello.c -o hello

# 最佳化編譯
gcc -O2 hello.c -o hello
```

#### C++ 程式編譯

```bash
# 建立簡單的 C++ 程式
nano hello.cpp
```

**hello.cpp：**
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

```bash
# 編譯 C++ 程式
g++ hello.cpp -o hello

# 執行
./hello

# 指定 C++ 標準版本
g++ -std=c++17 hello.cpp -o hello
g++ -std=c++20 hello.cpp -o hello
```

### 編譯選項

#### 常用編譯選項

```bash
# 基本選項
gcc -o output input.c          # 指定輸出檔名
gcc -c file.c                  # 只編譯不連結
gcc -S file.c                  # 產生組合語言檔

# 警告選項
gcc -Wall file.c               # 啟用所有警告
gcc -Wextra file.c             # 啟用額外警告
gcc -Werror file.c             # 將警告視為錯誤

# 除錯選項
gcc -g file.c                  # 加入除錯資訊
gcc -g3 file.c                 # 加入詳細除錯資訊

# 最佳化選項
gcc -O0 file.c                 # 不最佳化（預設）
gcc -O1 file.c                 # 基本最佳化
gcc -O2 file.c                 # 標準最佳化（推薦）
gcc -O3 file.c                 # 高度最佳化
gcc -Os file.c                 # 最佳化程式大小

# 標準版本
gcc -std=c11 file.c            # C11 標準
gcc -std=c99 file.c            # C99 標準
g++ -std=c++17 file.cpp        # C++17 標準
g++ -std=c++20 file.cpp        # C++20 標準

# 連結選項
gcc file.c -lm                 # 連結數學函式庫
gcc file.c -lpthread           # 連結執行緒函式庫
gcc file.c -L/path/to/lib -lmylib  # 指定函式庫路徑
```

### Makefile

#### 基本 Makefile

```bash
# 建立 Makefile
nano Makefile
```

**基本 Makefile 範例：**
```makefile
CC = gcc
CFLAGS = -Wall -g -O2
TARGET = myprogram
SOURCES = main.c utils.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJECTS) $(TARGET)

.PHONY: clean
```

#### 使用 Makefile

```bash
# 編譯
make

# 清理編譯產物
make clean

# 重新編譯
make clean && make

# 指定目標
make myprogram

# 顯示詳細資訊
make -n  # 顯示會執行的命令但不執行
```

### CMake

CMake 是跨平台的建置系統產生器。

#### 安裝 CMake

```bash
# 安裝 CMake
sudo apt update
sudo apt install cmake

# 檢查版本
cmake --version
```

#### 基本 CMake 使用

```bash
# 建立 CMakeLists.txt
nano CMakeLists.txt
```

**基本 CMakeLists.txt 範例：**
```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

add_executable(myprogram main.c utils.c)

# 連結函式庫
target_link_libraries(myprogram m pthread)
```

#### 使用 CMake

```bash
# 建立建置目錄
mkdir build
cd build

# 產生建置檔案
cmake ..

# 編譯
make

# 執行
./myprogram

# 清理
make clean
# 或
rm -rf build
```

### 開發工具

#### 安裝開發工具

```bash
# 安裝除錯工具
sudo apt install gdb          # GNU 除錯器
sudo apt install valgrind     # 記憶體檢查工具

# 安裝程式碼分析工具
sudo apt install cppcheck     # C/C++ 靜態分析工具

# 安裝文件產生工具
sudo apt install doxygen      # 文件產生器
```

#### 使用 GDB 除錯

```bash
# 編譯時加入除錯資訊
gcc -g program.c -o program

# 啟動 GDB
gdb ./program

# GDB 常用指令
(gdb) run                    # 執行程式
(gdb) break main             # 在 main 函式設中斷點
(gdb) break file.c:10        # 在檔案第 10 行設中斷點
(gdb) continue               # 繼續執行
(gdb) next                   # 執行下一行
(gdb) step                   # 進入函式
(gdb) print variable         # 顯示變數值
(gdb) backtrace              # 顯示呼叫堆疊
(gdb) quit                   # 退出
```

## Python 開發環境

### Python 安裝

#### 安裝 Python

```bash
# 檢查 Python 版本
python3 --version

# 安裝 Python（通常已預裝）
sudo apt update
sudo apt install python3

# 安裝 Python 開發工具
sudo apt install python3-dev python3-pip

# 安裝特定版本的 Python
sudo apt install python3.11 python3.11-dev
```

#### 安裝 pip

```bash
# pip 通常隨 Python 一起安裝
pip3 --version

# 如果未安裝，使用以下指令
sudo apt install python3-pip

# 升級 pip
pip3 install --upgrade pip
```

### 虛擬環境

#### 使用 venv（推薦）

```bash
# 建立虛擬環境
python3 -m venv myenv

# 啟動虛擬環境
source myenv/bin/activate

# 停用虛擬環境
deactivate

# 在虛擬環境中安裝套件
pip install package-name

# 列出已安裝套件
pip list

# 匯出套件清單
pip freeze > requirements.txt

# 從清單安裝套件
pip install -r requirements.txt
```

#### 使用 virtualenv

```bash
# 安裝 virtualenv
sudo apt install python3-virtualenv

# 建立虛擬環境
virtualenv myenv

# 啟動虛擬環境
source myenv/bin/activate
```

### Python 套件管理

#### 使用 pip

```bash
# 安裝套件
pip install package-name
pip install package-name==1.0.0  # 指定版本
pip install package-name>=1.0.0  # 最低版本

# 升級套件
pip install --upgrade package-name

# 解除安裝套件
pip uninstall package-name

# 搜尋套件
pip search package-name

# 顯示套件資訊
pip show package-name

# 安裝本地套件
pip install /path/to/package
pip install .  # 安裝目前目錄的套件
```

#### 常用 Python 套件

```bash
# 科學計算
pip install numpy scipy matplotlib pandas

# Web 開發
pip install flask django fastapi

# 資料庫
pip install sqlalchemy pymysql psycopg2

# 測試
pip install pytest unittest

# 開發工具
pip install ipython jupyter black flake8
```

### Python 編譯擴充

#### 編譯 Python C 擴充

```bash
# 建立 setup.py
nano setup.py
```

**setup.py 範例：**
```python
from setuptools import setup, Extension

module = Extension('mymodule',
                   sources=['mymodule.c'])

setup(name='mymodule',
      version='1.0',
      description='My C extension',
      ext_modules=[module])
```

```bash
# 編譯擴充
python3 setup.py build_ext --inplace

# 安裝擴充
python3 setup.py install
```

### 開發工具

#### 安裝開發工具

```bash
# 程式碼格式化
pip install black

# 程式碼檢查
pip install flake8 pylint

# 型別檢查
pip install mypy

# 測試框架
pip install pytest unittest

# 互動式環境
pip install ipython jupyter
```

#### 使用工具

```bash
# 格式化程式碼
black myfile.py

# 檢查程式碼
flake8 myfile.py
pylint myfile.py

# 型別檢查
mypy myfile.py

# 執行測試
pytest
python -m unittest
```

## 整合開發環境（IDE）

### Visual Studio Code

```bash
# 安裝 VS Code（從官網下載 .deb 檔）
# 或使用 snap
sudo snap install code --classic

# 安裝擴充套件
# - C/C++ (Microsoft)
# - Python (Microsoft)
# - CMake Tools
```

### Code::Blocks

```bash
# 安裝 Code::Blocks
sudo apt install codeblocks
```

### Eclipse

```bash
# 安裝 Eclipse
sudo apt install eclipse
```

## 實用範例

### 編譯多檔案 C 專案

```bash
# 專案結構
project/
├── main.c
├── utils.c
├── utils.h
└── Makefile

# Makefile
CC = gcc
CFLAGS = -Wall -g -O2
TARGET = myprogram
SOURCES = main.c utils.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJECTS) $(TARGET)
```

### Python 專案結構

```bash
# 專案結構
myproject/
├── myproject/
│   ├── __init__.py
│   ├── main.py
│   └── utils.py
├── tests/
│   └── test_main.py
├── requirements.txt
├── setup.py
└── README.md
```

## 常見問題

### 問題：找不到標頭檔

```bash
# 安裝開發標頭檔
sudo apt install libssl-dev
sudo apt install libxml2-dev
sudo apt install libcurl4-openssl-dev

# 指定標頭檔路徑
gcc -I/usr/include/custom file.c
```

### 問題：找不到函式庫

```bash
# 安裝函式庫開發檔
sudo apt install libmylib-dev

# 指定函式庫路徑
gcc -L/usr/lib/custom file.c -lmylib

# 查看函式庫位置
ldconfig -p | grep mylib
```

### 問題：Python 套件安裝失敗

```bash
# 使用系統套件管理
sudo apt install python3-package-name

# 或使用 pip 安裝（可能需要編譯工具）
sudo apt install build-essential python3-dev
pip install package-name
```

## 最佳實踐

1. **使用虛擬環境**：Python 專案使用虛擬環境隔離依賴
2. **版本控制**：使用 Git 管理程式碼
3. **編譯選項**：開發時使用 `-Wall -g`，發布時使用 `-O2`
4. **文件化**：為程式碼加入註解和文件
5. **測試**：撰寫單元測試確保程式正確性
6. **程式碼檢查**：使用工具檢查程式碼品質

## 常用指令速查

```bash
# C/C++ 編譯
gcc file.c -o program
g++ file.cpp -o program
gcc -Wall -g -O2 file.c -o program

# Make
make
make clean

# CMake
cmake ..
make

# Python
python3 script.py
pip install package
python3 -m venv venv
source venv/bin/activate

# 除錯
gdb ./program
python3 -m pdb script.py
```

## 相關檔案位置

- GCC：`/usr/bin/gcc`
- Python：`/usr/bin/python3`
- pip：`~/.local/bin/pip` 或 `/usr/bin/pip3`
- 虛擬環境：通常在專案目錄下
- 系統函式庫：`/usr/lib/`
- 標頭檔：`/usr/include/`

## 參考資源

- GCC 文件：https://gcc.gnu.org/onlinedocs/
- CMake 文件：https://cmake.org/documentation/
- Python 文件：https://docs.python.org/3/
- Make 手冊：`man make`
- GDB 手冊：`man gdb`