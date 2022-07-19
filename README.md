# Pythoner
Python相關知識
## 小知識集
### sleep, 暫停幾秒
```python
import time
time.sleep(t)
```
### 連續數字組成list
```python
list(range(1, 6)) # [1,2,3,4,5]
```
### VSCode
#### 新增jupyter編輯檔案
* Reference: https://code.visualstudio.com/docs/datascience/jupyter-notebooks
* Command Palette (Ctrl+Shift+P) input Jupyter: `Create New Jupyter Notebook`

### 紀錄log
#### module
logging
#### code
```python
import logging
import time
from datetime import datetime, timedelta
import os

####### function ####################################################
def handle_exception(exc_type, exc_value, exc_traceback):
    logger.error("Uncaught exception", exc_info=(exc_type, exc_value, exc_traceback))
    
def GetLogNum(logFolder, logtime):
    #files = glob.glob(logFolder+time.strftime("%Y-%m-%d_*.log"))
    maxnum = 0
    #folderNameinLOG = os.listdir(logFolder)
    dayFolderName = time.strftime("%Y-%m-%d", logtime)
    if os.path.isdir(logFolder+dayFolderName):
        files = glob.glob(logFolder+dayFolderName)

        for f in files:
            fullpath, fname = os.path.split(f)
            fsplitdot = fname.split('.')[0]
            fsplitdash = fsplitdot.split('_')
            if fsplitdash[0] == dayFolderName:
                try:
                    if int(fsplitdash[1]) > maxnum:
                        maxnum = int(fsplitdash[1])

                except:
                    print('Create log path: {}'.format(f))

    return maxnum
####### function ####################################################

####### log setting #################################################
# log format setting
if not os.path.isdir('LOG'):
    os.mkdir('LOG')
logFolder = "LOG/"
# Log format
FORMAT = '%(asctime)s: %(message)s' # asctime: create time, message: 後面的logging.info中的內容

sys.excepthook = handle_exception
logger = logging.getLogger(__name__)
handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)

for handler in logging.root.handlers[:]:
    logging.root.removeHandler(handler)
    handler.close()

logtime = time.localtime()
lognum = []
lognum.append(GetLogNum(logFolder, logtime)+1)
dayLogFolderName = time.strftime('%Y-%m-%d', logtime)
if not os.path.isdir(logFolder+dayLogFolderName):
    os.mkdir(logFolder+dayLogFolderName)
lastlogfile = []
lastlogfile.append(logFolder+dayLogFolderName+'/' +
                   dayLogFolderName+time.strftime("-%H%M%S.log", logtime))
logging.basicConfig(level=logging.INFO,
                    filename=lastlogfile[0], filemode='a', format=FORMAT)
####### log setting #################################################

####### logging #####################################################
logging.info("folder: {}, first replay file, get command = False".format(listReplayFolder[indReplayFolder]))
####### logging #####################################################
```

### 刪除list裡的多個元素
```python
del listItem[a:b] # 刪除listItem中index a到b-1的元素
```

### 取得修改檔案時間、建立檔案時間
```python
import datetime
import os

# Path to the file
path = r"E:\demos\files_demos\sample.txt"

# file modification timestamp of a file
m_time = os.path.getmtime(path)
# convert timestamp into DateTime object
dt_m = datetime.datetime.fromtimestamp(m_time)
print('Modified on:', dt_m)

# file creation timestamp in float
c_time = os.path.getctime(path)
# convert creation timestamp into DateTime object
dt_c = datetime.datetime.fromtimestamp(c_time)
print('Created on:', dt_c)
```

### 取代字串內容
```python
txt = "I like bananas"

x = txt.replace("bananas", "apples")

print(x) # I like apples
```

## MultiThread
* Reference: 
  * https://blog.gtwang.org/programming/python-threading-multithreaded-programming-tutorial/
  * https://www.maxlist.xyz/2020/03/15/python-threading/
  * https://www.wongwonggoods.com/python/python-threading/

## Database相關
### MySQL
```
使用module: pymysql
> pip install PyMySQL
```
#### 簡單功能使用
```python
import pymysql
db_settings = {
  "host" : "10.110.140.224",
  "port" : 3306,
  "user" : "root",
  "password" : "hnsd123",
  "db" : "alarmsystem",
  "charset" : "utf8"
}

# 上傳資料(單筆)
try:
  conn = pymysql.connect(**db_settings)
  with conn.cursor() as cursor: # cursor is like handle in c++
    command = "INSERT INTO hypercto_client(ClientIP, RecordTime)VALUES(%s, %s)" # sql command
    cursor.execute(command, ("100.109.44.123","2021-04-19 15:03:27"))
    conn.commit()
    conn.close()
except Exception as ex:
  print(ex)
  
# 上傳一群資料
try:
  conn = pymysql.connect(**db_settings)
  with conn.cursor() as cursor: # cursor is like handle in c++
    command = "INSERT INTO hypercto_client(ClientIP, RecordTime)VALUES(%s, %s)" # sql command
    tuples = [("100.109.44.123","2021-04-19 15:03:27")...] # 上傳一群資料的資料儲存方式
    cursor.executemany(command, tuples) # executemany，上傳一群檔案
    conn.commit()
    conn.close()
except Exception as ex:
  print(ex)
  
# 查詢資料
try:
  conn = pymysql.connect(**db_settings)
  with conn.cursor() as cursor: # cursor is like handle in c++
    command = "SELECT * from hypercto_client" # sql command
    cursor.execute(command)
    data = cursor.fetchall()
    print(data)
    conn.close()

except Exception as ex:
  print(ex)
```

## DateTime相關
### module
pytz  

### 調整時區設定
```
可以用"Etc/GMT-0"的方式調整時區
```

## C++ code to dll，之後套用到python
* Reference: https://lkm543.medium.com/dll%E6%AA%94%E6%98%AF%E7%94%9A%E9%BA%BC-%E8%AE%93dll%E6%8A%8Apython%E5%8A%A0%E9%80%9F75%E5%80%8D-994e35efa38d
  * 網頁描述: 費波納西數列加快75倍

## matplotlib
* line chart
  ```python
  import matplotlib.pyplot as plt
  import numpy as np

  plt.style.use('_mpl-gallery')

  # make data
  x = np.linspace(0, 10, 100)
  y = 4 + 2 * np.sin(2 * x)

  # plot
  fig, ax = plt.subplots()

  ax.plot(x, y, linewidth=2.0)

  ax.set(xlim=(0, 8), xticks=np.arange(1, 8), ylim=(0, 8), yticks=np.arange(1, 8))

  plt.show()
  ```
  
## pandas
### read_csv
```python
import pandas as pd
df = pd.read_csv(csv_file_name, names=['name_of_column'])
```
### 刪除DataFrame中特定資料的方法
```python
matchFileTotalSize = df["File Total Size (KB)"].isin(['--'])
df[matchFileTotalSize] = np.nan
matchFileDLSize = df["File DL Size (KB)"].isin(['--'])
df[matchFileDLSize] = np.nan
matchProgress = df["Progress"].isin(['--'])
df[matchProgress] = np.nan
matchStatus = df["Status"].isin(['RUNNING'])
df[~matchStatus] = np.nan      
dfutr = df.dropna()
```

## memory
* python有記憶體問題，process取用記憶體使用後，不使用並不會將記憶體返還給系統，而是留著等process建立其他物件時進行分配，如果不希望記憶體使用這麼大，可使用multithread的方式，用新的thread來處理需要使用大量記憶體的部分，在處理完後thread結束會將使用的大量記憶體返還給系統

## console程式，關閉quick edit，或console模式調整
```python
import msvcrt
import ctypes

def disable_quickedit():
    '''
    Disable quickedit mode on Windows terminal. quickedit prevents script to
    run without user pressing keys..'''
    if not os.name == 'posix':
        try:

            kernel32 = ctypes.WinDLL('kernel32', use_last_error=True)
            device = r'\\.\CONIN$'
            with open(device, 'r') as con:
                hCon = msvcrt.get_osfhandle(con.fileno())
                #kernel32.SetConsoleMode(hCon, 0x0080)
                kernel32.SetConsoleMode(hCon, 0x0081)

        except Exception as e:
            print('Cannot disable QuickEdit mode! ' + str(e))
            logging.info('Cannot disable QuickEdit mode! ' + str(e))
            print('.. As a consequence the script might be automatically\
            paused on Windows terminal')
            logging.info('.. As a consequence the script might be automatically\
            paused on Windows terminal')
```
## console程式，取消console視窗上的選項
* 右上角關閉的x按鈕
```python
import win32console
import win32gui
import win32con

# disable close button
hwnd = win32console.GetConsoleWindow()
if hwnd:
    hMenu = win32gui.GetSystemMenu(hwnd, 0)
    if hMenu:
        logging.info('Disable close icon on right-top of the console')
        win32gui.EnableMenuItem(hMenu, win32con.SC_CLOSE,
                                win32con.MF_DISABLED)
```
