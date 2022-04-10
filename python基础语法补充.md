### python基础语法补充

#### 一、循环

##### 1.while循环（python不支持do while语法）

**符合条件就循环**

```python
while 条件:
  循环体
  
  
i = 0
while i < 10:
  print(i)
else: #条件不成立时，默认执行这里（但是循环是break掉的，就不会执行这里）
  print("ok!")
  

```

#### 二、格式化方案

##### 1.老的格式化	

- **%s** ：字符串的占位符，稍后会添加字符串（任何内容）
- **%d**：表示字符串中占位整数，不是整数会报错(digit)
- **%f**  : 表示字符串中占位小数，具体精确到哪一位小数，% . 2f

```python
s = """------------------info of %s -------------------
    Name: %s
    age : %s
    job : %s
    hobby : %s
  """ %(name,name,age,job,hobby)

```

##### 2.新的方案（要求python版本在3.5以上）

```python
name = "salar"
addr = "水坑"
job = "打飞机"

print("我叫%s,我喜欢在%s%s"%(name,addr,job))

# 新的方案
print(f"我叫{name},我喜欢在{addr}做{job}")

```

##### 3.五种格式化的方法

```python
name = "salar"
age = 123
s1 = "我叫%s,我今年%s岁了" % (name,age)
s2 = f"我叫{name},我今年{age}岁了"
s3 = "我叫{}，我今年{}岁了".format(name,age)
s4 = "我叫{0},我今年{}碎了".format(name,age)
s5 = "我叫{aname},我今年{aage}岁了".format(aname = name,aage = age)
```

#### 三、类

##### 1.类的定义

- **普通定义**

```python
class 类名:
  属性1 = value1
  属性2 = value2
  def func(self):  #self代表的是实例对象
    pass
```

- **构造器定义**

```python
class 类名：
  def __init__(self,arg):
    self.arg = arg
    
  def func(self):
    pass
```

- 区别
  - 普通类的属性定义都是全局的（通用的），构造体定义的都是实例化的（个体的）
  - 类名.属性1，这是全局的，self.属性1，这是实例对象的（各自的）

##### 2.类的私有

- 只需要在前边加上 __(两个下划线)，属性是这样，函数也可以
- 类外是无法访问到类内的，并且继承也不继承私有

##### 3.类的继承

**一定要用 super(Cat, self).__init__(name,age) 去初始化父类，否则，继承自 Animal的 Cat子类将没有 name和age两个属性。**

```python
class Animal(object):  #  python3中所有类都可以继承于object基类
   def __init__(self, name, age):
       self.name = name
       self.age = age

   def call(self):
       print(self.name, '会叫')

######
# 现在我们需要定义一个Cat 猫类继承于Animal，猫类比动物类多一个sex属性。 
######
class Cat(Animal):
   def __init__(self,name,age,sex):
       super(Cat, self).__init__(name,age)  # 不要忘记从Animal类引入属性
       self.sex=sex

if __name__ == '__main__':  # 单模块被引用时下面代码不会受影响，用于调试
   c = Cat('喵喵', 2, '男')  #  Cat继承了父类Animal的属性
   c.call()  # 输出 喵喵 会叫 ，Cat继承了父类Animal的方法 
```

##### 4.类外的访问

- 方法一：使用一个函数来调用

```python
class Person():
  def __init__(self,name):
    self.__name = name
    
  def name(self):
    return self.__name

```

- 方法二：使用装饰器

```python
class Person():
  def __init__(self,name):
    self.__name = name
    
  @property   # 通过这样写，类外调用就可以使用Person().name 
  def name(self):
    return self.__name
  
  @name.setter            # 通过这样写，类外就可以修改name的值了，Person().name = "张三"   
  def name(self,value):   # 需要注意的是，@name.setter的name和上边name(self)的name必须一致
    self.__name = value
  
```

#### 四、修饰器(@)

##### 1.普通装饰器

作用：可以在改变原来代码的基础上，给函数添加新的功能，

		  可以在原有的操作前边或者后边随意添加新的功能

```python
#-----------------------代码原理-----------------------
def wrapper(fn): #传入参数是函数
  def inner():
    print("函数执行之前")
    fn()  #执行被修饰的函数
    print(" 被修饰的函数执行之后")
  return inner # 将内层函数返回
  
def add():
  print("我是新增函数")
  
add = wrapper(add) #此时add就编程了inner
add()    # 此时执行的就是inner函数

#-------------------------------------------------------
# add = wrapper(add) 这句代码可以被替换成如下：
#使用@方法
@wrapper  #add = wrapper(add)
def add():
  print("这是新增函数")
  
add()
```

##### 2.通用装饰器

```python
def wrapper(fn):
  def inner(*args,**kwargs):
    "在执行目标函数之前"
    rst = fn(*args,**kwargs)  #处理目标函数的返回值
    "在执行目标函数之后"
    return rst
  return inner
  
@wrapper
def func(参数):
  pass

target = func() #千万注意这里执行的是inner函数
```

##### 3.高级装饰器

**一个函数被多个装饰器修饰**

```python
def wrapper1(fn):
  def inner(*args,**kwargs):
    "wrapper1-before"
    rst = fn(*args,**kwargs)  #处理目标函数的返回值
     "wrapper1-after"
    return rst
  return inner
  
def wrapper2(fn):
  def inner(*args,**kwargs):
    "wrapper2-before"
    rst = fn(*args,**kwargs)  #处理目标函数的返回值
     "wrapper2-after"
    return rst
  return inner
  
def wrapper3(fn):
  def inner(*args,**kwargs):
    "wrapper3-before"
    rst = fn(*args,**kwargs)  #处理目标函数的返回值
    "wrapper3-after"
    return rst
  return inner

#就近原则装饰
@wrapper3
@wrapper2
@wrapper1
def target(参数):
  print("我是target")
  
#执行顺序,一层包一层
"""
wrapper3-before
wrapper2-before
wrapper1-before
我是target
wrapper1-after
wrapper2-after
wrapper3-after
"""
```

##### 4.带参数的装饰器

**针对不同的函数调用不同的装饰器**

```python
def gua_out(gua_name):
  def gua(fn):
    def inner(*args,**kwargs):
      print("开启{gua_name}外挂")
      rst = fn(*args,**kwargs)
      print("关闭外挂")
      return rst
    return fn
  return gua

@gua_out("气球") #先执行函数调用，返回一个装饰器和，和@组成语法糖，@gua
def dnf():
  print("我要打卢克西")
  
def lol():
  print("我要杀边全场")
```

#### 五、函数补充

##### 1.实参

- 位置参数：按照位置，给性参数传递参数
- 关键字参数：按照形参的名字给进行传递数据
- 混合参数（位置参数+关键字参数），但有顺序，先位置参数，后关键字参数

##### 2.形参

- 位置参数
- 默认值参数：在形参申明的时候，直接给出一个默认值
  - 后续在访问到这个函数的时候，该参数就可以不给出具体数据，直接使用默认值
  - 在写实参的时候，可以不写这个参数，需要给值，就用给出的值
  - **坑**：如果你的默认值参数是一个可变的数据类型，会被共享
  ```python
  def func(value,lst = []):
    lst.append(value)
    print(lst)
    
  func(10086) # [10086]
  func(7788)  # [10086,7788]
  ```
- 动态传参
  - \*参数名：动态接收位置参数，位置参数会被打包成一个**元组类型**
  - \*\*参数名：动态接收关键字参数，关键字参数会被打包成一个**字典类型**
  - 混合使用，要注意顺序，**正确顺序：**位置参数，\*args，默认值参数，**kwargs
    - def func(*arg,\*\*kwargs)，可以接受任意参数，但注意他们的数据类型分别是元组和字典
- 动态传参补充
  - 如果要把列表元素的每一项当成参数传递给func函数，可以利用\*完成**打散操作**，可以借助\*把可迭代对象转换成**位置参数**进行传参
  - 对于关键字参数，可以将字典转转换成关键字参数，通过\*\*在实参位置来传递（**打散操作**），在形参使用\*\*来接收，把**关键字参数**聚合成字典
  ```python
  def func(*args):
    print(args)
  lst = ["张无忌","张翠山","张三丰"]
  
  #把列表中的每一项当成参数传递给func
  func(lst[0],lst[1],lst[2])
  
  # 可以借助*来完成打散操作
  func(*lst)  #（"张无忌","张翠山","张三丰"）
  ```

```python
def func(**kwargs):# 把关键字聚合成字典
  print(kwargs)

dic = {"name":"赵敏","age":18}

#下边两种方法结果一样
func(name = dic["name"],age = dic["age"])
func(*dic)
```

#### 六、协程（Coroutine）

**协程函数里面不能有同步的模块，要不然不起作用**

**首先，协程是非真实存在的**，但线程和进程是真实存在的

**协程：**协程被称为微线程，是一种用户动态内的上下文切换技术，简单来说，就是通过一个线程实现代码块相互切换的技术。（来回跳着执行）

python 3.4版本以后提供了一种asyncio模块 + Python3.5推出的async、async语法内部基于协程并且遇到io请求自动化切换

##### 1.**事件循环：**理解成一个死循环，去检测某些代码

```python
任务列表 = [任务一，任务二，任务三...]
while True:
  可执行任务列表，已完成任务列表 = 去任务列表中检查所有的任务，将"可执行"和"已完成"的任务返回
  
  for 就绪任务 in 可执行的任务列表:
    执行已就绪的任务
    
  for 已完成的任务 in 已完成任务列表:
    在任务列表里移除已完成的任务
    
  如果任务列表的任务都完成，终止循环
  
  
#python代码
import asyncio

#生成一个事件循环
loop = asyncio.get_event_loop()

#将任务添加到"任务列表"
loop.run_until_complete(任务)
  
```

##### 2.协程函数：定义函数的时候`async def 函数名`

##### 3.协程对象：执行 协程函数() 得到的协程对象

注意：创建协程函数和创建协程对象，**函数内部代码不执行**

		   如果想要运行协程函数内部代码，必须要将协程对象交给**	事件循环**来处理

```python
#定义协程函数

async def func():
  pass

#得到协程对象
result = func()

#这是老版本的写法
loop = asyncio.get_event_loop()
loop.run_until_complete(result)

# python 3.7以上
asyncio.run(result)
```

##### 4.**await关键字**

**await + 可等待的对象（协程对象、Future对象、Task对象->io等待）**

await就是等待对象得到结果之后才继续往下走

##### 5.Task对象

- 往事件循环中添加多个任务，用于并发调度协程
- 通过**asyncio.create_task(协程对象)**的方式创建Task对象，让协程加入事件循环中等待被调度执行（3.7之后才引进）
- 除了上边的，还有低层级的`loop.create_task()`或` ensure_future()`函数。不建议手动实例化Task对象

示例一：（用的较少）

```python
import asyncio

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return "返回值"

async def main():
    print('main 函数开始')

    # 创建一个task对象,将当前执行的task对象添加到事件循环中
    task1 = asyncio.create_task(func())
    
    # 创建一个task对象,将当前执行的task对象添加到事件循环中
    task2 = asyncio.create_task(func())

    print("main函数结束")

    # 当执行的时候遇到io操作，会自动切换执行其他任务
    # 此处的await是等待相应的协程全部都执行完毕并获取结果

    ret1 = await task1
    ret2 = await task2

    print(ret1,ret2)

asyncio.run(main())
```

示例二（换种写法），常用写法

```python
import asyncio
from unicodedata import name

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return "返回值"

async def main():
    print('main 函数开始')

    # 一般方法是创建一个task对象列表
    task_list = [
        asyncio.create_task(func()),
        asyncio.create_task(func())
    ] 
    print("main函数结束")

    # 当执行的时候遇到io操作，会自动切换执行其他任务
    # 此处的await是等待相应的协程全部都执行完毕并获取结果
    # pending 会返回一个set（）集合，没啥意义
    done,pending = await asyncio.wait(task_list，timeout = none)

    print(done,pedding)

asyncio.run(main())
```

示例三  

```python
import asyncio
from unicodedata import name

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return "返回值"

# 一般方法是创建一个task对象列表
task_list = [
    func(),
    func()
] 

# 当执行的时候遇到io操作，会自动切换执行其他任务
# 此处的await是等待相应的协程全部都执行完毕并获取结果
# pending 会返回一个set（）集合，没啥意义
done,pending = asyncio.run(asyncio.wait(task_list，timeout = none))
```

##### 6.Future对象（task的基类）

Task继承Future，Task对象内部await结果的处理基于Future对象而来的

示例一：下边的代码是个死循环

```python
import asyncio

async def main():
    # 获取当前的事件循环
    loop = asyncio.get_event_loop()

    # 创建一个任务（Future对象），这个任务啥也不干
    fut = loop.create_future()

    # 等待最终结果（Future对象）,没有结果会一直等待下去 
    await fut

asyncio.run(main())
```

示例二

```python
import asyncio


async def set_after(fut):
    await asyncio.sleep(2)
    fut.set_result("666")

async def main():
    # 获取当前的事件循环
    loop = asyncio.get_event_loop()

    # 创建一个任务（Future对象），没绑定任何行为，则这个任务永远不知道什么时候结束
    fut = loop.create_future()

    # 创建一个任务（task对象）绑定了set_after函数，函数内部在2s之后，会给fut赋值
    # 即手动设置future任务的最终结果，那么fut就可以结束了

    await loop.create_task(set_after(fut))

    # 等待最终对象获取，最终结果，否则就会一直等待下去
    data = await fut
    print(data)

asyncio.run(main())
```

##### 7.concurrent.futures.Future对象（太难了）

使用进程池和线程池实现异步操作得时用到的对象

##### 8.异步迭代器（以后再补充）

##### 9.aiohttp

```python
import asyncio
import aiohttp



async def get_page(url):
    async with aiohttp.ClientSession() as session:
        # get/post 
        # headers params/data proxy = "https://ip/port"
        async with await session.get(url) as response:
            # text() 字符串的文本信息
            # read() 二进制的信息
            # json() json对象
            # 获取响应数据操作之前一定要使用await进行手动挂起
            page_text = await response.text()
        
tasks = []
urls = ["https://www.baidu.com/",
        "https://www.jd.com/",
        "https://www.taobao.com/"
        ]
for url in urls:
    c = get_page(url)  # 返回一个协程对象
    task = asyncio.ensure_future(c) # 将协程对象封装成任务对象
    tasks.append(task)

# 创建事件循环
loop = asyncio.get_event_loop()

# 运行任务列表
loop.run_until_complete(asyncio.wait(tasks))

```