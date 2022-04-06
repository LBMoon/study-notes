### python知识点补充

#### 一、并发编程

**线程和进程**

**类比**：

- 一个**工厂**，至少有一个**车间**，一个车间里面至少有一个**工人**，最终是**工人**在工作
- 一个**程序**，至少有一个**进程**，一个进程里面至少有一个**线程**，最终是**线程**在工作

python的一个程序在运行的时候，内部就会创建一个进程（主进程），在进程中就会创建一个线程（主线程），由线程逐行运行代码。

- **线程：**是计算机中可以被cpu调度的最小的单元（真正在工作（相当于工人））
- **进程：**是计算机资源分配的最小单元（进程为线程提供资源）

一个进程中够可以包含多个线程，同一个进程中的线程可以**共享**此进程中的资源。

##### 1.多线程（threading）和多进程（multiprocessing）

```python

from multiprocessing import Process
from threading import Thread

def func(a1,a2,a3):
    passs
 
#实例化一个线程
t = Thread(target=func,args=(11,22,33))
# 开始执行线程
t.start()

# 实例化一个进程
p = Process(target = func,args=(11,22,33))
# 开始执行进程
p.start()

```

举例：下载抖音的短视频

- 正常执行：

```python

# 正常执行

import time
import requests

ul_list = [
    {'aaa.mp4','https://123.anfis.com/ajifn'},
    {'bbb.mp4','https://123.anfis.com/ajifn'},
    {'ccc.mp4','https://123.anfis.com/ajifn'}
]
print(time.time())

def task(fileName,videoUrl):
    res = requests.get(videoUrl)
    with open(fileName,"wb") as f:
        f.write(res.content)
        
print(time.time())

```

- **使用多线程**

```python
from threading import Thread
import time
import requests

ul_list = [
    {'aaa.mp4','https://123.anfis.com/ajifn'},
    {'bbb.mp4','https://123.anfis.com/ajifn'},
    {'ccc.mp4','https://123.anfis.com/ajifn'}
]

print(time.time())

def task(fileName,videoUrl):
    res = requests.get(videoUrl)
    with open(fileName,"wb") as f:
        f.write(res.content)
        
# 为什么将打印结束的时间函数写在了这里，而不是最下边
# 是因为主线程会直接串行执行程序，不管其他线程是否执行完
# 主线程会直接执行到最后，然后等其他线程执行完毕
    print(time.time())
  
for name,url in url_list:
    
    # 创建线程，让每一个线程去执行task函数（参数不同）
    t = Thread(task,args=(name,url))
    t.start()
    

```

- **使用多进程**（代码和多线程基本一样）

```python
from multiprocessing import Process
import time
import requests

ul_list = [
    {'aaa.mp4','https://123.anfis.com/ajifn'},
    {'bbb.mp4','https://123.anfis.com/ajifn'},
    {'ccc.mp4','https://123.anfis.com/ajifn'}
]

print(time.time())

def task(fileName,videoUrl):
    res = requests.get(videoUrl)
    with open(fileName,"wb") as f:
        f.write(res.content)
        
    print(time.time())
  
# 必须写在这里面
if __name__ == "__main__":
    for name,url in url_list:

        # 创建进程程，让每一个线程去执行task函数（参数不同）
        t = Process(task,args=(name,url))
        t.start()


```

**总结**：**多进程** 比 **多线程** 的开销大

##### 2.GIL锁（全局解释器锁 Global Interpreter Lock）

**GIL锁**是Cpython解释器特有的，让一个进程中同一时刻只能有一个线程可以被CPU调用

- 如果需要使用到计算机的多核，建议使用多进程，突破GIL锁机制，实现并发
- 如果不打算使用多核优势，那就使用多线程开发。

**使用场景**：计算操作需需要使用CPU的多核优势，IO操作不需要

- 计算密集型，用**多进程**，例如：大量的数据计算【累加计算示例】
- IO密集型，用**多线程**，例如：文件读写、网络数据传输【下载抖音短视频】

**多进程和多线程可以配合使用**

##### 3.线程的常用方法

- **t.start( )**，当前线程准备就绪（等待CPU调度，具体时间是由CPU来决定）

- **t.join( )**，等待当前线程的任务执行完毕后再向下继续执行。

- **t.setDaemon(布尔值)**，守护线程，必须放在start之前。

  - 如果是t.setDaemon(True)，设置为守护线程，主线程执行完后，子线程也会自动关闭。
  - 如果是t.setDaemon(False)，设置为非守护线程，主线程等待子线程，子线程执行完毕后，主线程才结束。（默认值是False）

- **线程名称的设置和获取**

  - 设置：**t.setName('xxxx')**，在start( )之前设置
  - 获取：**name = threading.current_thread().getName()**

- **自定义线程类**，直接将线程需要做的事情写到run的方法里面

  ```python
  import threading
  class MyThread(threading.Thread):
      def run(self):
          print('执行次线程'，self._args)
          
  t = MyThread(args = (100,))
  t.start()
  ```

  ```python
  import requests
  import threading
  
  class DouYinThread(threading.Thread):
      def run(self):
          file_name,video_url = self._args
          res = requests.get(videoUrl)
      	with open(fileName,"wb") as f:
          	f.write(res.content)
             
  ul_list = [
      {'aaa.mp4','https://123.anfis.com/ajifn'},
      {'bbb.mp4','https://123.anfis.com/ajifn'},
      {'ccc.mp4','https://123.anfis.com/ajifn'}
  ]
  
  for item in url_list:
      t = DouYinThread(args=(itme[0],item[1]))
      t.start()
              
  ```

- **线程锁(线程安全)：**

  - 列表 L.append()
  - 列表 L.extend(L2)
  - 列表 x = L[i],x.pop()
  - 列表 L[ i : j]
  - 列表 L.sort()
  - x = y
  - x.field = y
  - D[x] = y
  - D1.upadta(D2)
  - D.keys()

**示例一：**

```python
import threading

# 创建一个锁,两个线程必须用同一把锁
lock = threading.Rlock()

loop = 10000000
number = 0

def _add(count):
    lock_object.acquire()  # 加锁
    global number
    for i in range(count):
        number += 1
    lock_object.releasee() # 解锁
    
def _sub(count):
    lock_object.acquire()  # 申请锁(等待)
    global number
    for i in range(count):
        number -= 1
    lock_object.release()
    
t1 = threading.Thread(target = _add,args = (loop,))
t2 = threading.Thrad(target = _sub,args = (loop,))

t1.start()
t2.start()

t1.join() # t1 线程执行完，才继续走
t2.join() # t2 线程执行完，才继续走
```

**示例二：(求和)**

```python
import threading
num = 0
lock_object = threading.Rlock()

def task():
    print("开始")
    lock_object.acquire()  # 第一个抵达的线程进入并上锁，其他线程就需要在此等待
    global = num
    for i in range(100000000):
        num += 1
    lock_object.release()  # 线程出去，并解开锁，其他线程就可以进入并且执行
    print(num)
    
for i in range(2):
    t = threading.Thread(target = task)
    t.start()
    
    
    
# 写法二
def task():
    print("开始")
	with lock_object: # 上下文管理，内部自动执行 acquire和release
        global num
        for i in range(1000000):
            num += 1
    print(num)

for i in range(2):
    t = threading.Thread(target = task)
    t.start()
```

 