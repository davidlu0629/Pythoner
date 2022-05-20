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


## MultiThread
* Reference: https://blog.gtwang.org/programming/python-threading-multithreaded-programming-tutorial/

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
