---
layout: post
title: Python 学习之路
categories: python
description: Python 学习之路
keywords: python
---

> Python 学习之路，Python 学习笔记


# Python Logging 日志模块

## 屏幕与文件输出

```
import logging
 
# 创建一个 logging 对象, TEST-LOG 为定义这个LOG的 name  [ %(name)s ]
logger = logging.getLogger('TEST-LOG')
# 设置日志 级别为 DEBUG, 全局日志 级别 全局级别 优先级高
logger.setLevel(logging.DEBUG)
 
 
# 创建一个 用于 屏幕输出 的 StreamHandeler
ch = logging.StreamHandler()
# 设置日志 级别为 DEBUG
ch.setLevel(logging.DEBUG)
 
 
# 创建一个 用于 文件输出 的 FileHandler, 并输出到 access.log 文件
fh = logging.FileHandler("access.log")
# 设置日志 级别为 WARNING
fh.setLevel(logging.WARNING)
 
 
# 创建一个 日志格式 Formatter (
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
 
 
# 让 屏幕输出，与 文件输出 都按照 formatter 这个格式生成
ch.setFormatter(formatter)
fh.setFormatter(formatter)
 
 
# 讲 StreamHandeler 与 FileHandler 添加到 logger 这个对象中。
logger.addHandler(ch)
logger.addHandler(fh)
 
 
# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
```


## 日志文件 轮转

```


import logging

 from logging.handlers import RotatingFileHandler


 #定义一个RotatingFileHandler，最多备份5个日志文件，每个日志文件最大10M

 Rthandler = RotatingFileHandler('myapp.log', maxBytes=10*1024*1024,backupCount=5)

 Rthandler.setLevel(logging.INFO)

 formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')

 Rthandler.setFormatter(formatter)

 logging.getLogger('').addHandler(Rthandler)


```


## handle方法

```

logging.StreamHandler: 日志输出到流，可以是sys.stderr、sys.stdout或者文件

logging.FileHandler: 日志输出到文件

日志回滚方式，实际使用时用RotatingFileHandler和TimedRotatingFileHandler

logging.handlers.BaseRotatingHandler

logging.handlers.RotatingFileHandler

logging.handlers.TimedRotatingFileHandler

logging.handlers.SocketHandler: 远程输出日志到TCP/IP sockets

logging.handlers.DatagramHandler:  远程输出日志到UDP sockets

logging.handlers.SMTPHandler:  远程输出日志到邮件地址

logging.handlers.SysLogHandler: 日志输出到syslog

logging.handlers.NTEventLogHandler: 远程输出日志到Windows NT/2000/XP的事件日志

logging.handlers.MemoryHandler: 日志输出到内存中的制定buffer

logging.handlers.HTTPHandler: 通过"GET"或"POST"远程输出到HTTP服务器

```



# Python 面相对象编程


## 类的 实例化

```

# 把一个抽象的类变成一个具体的对象的过程， 叫实例化。

class Role(object):
    def __init__(self,name,role,weapon,life):
        self.name = name
        self.role = role
        self.weapon = weapon
        self.life = life

    def buy_weapon(self,weapon):
        self.weapon = weapon
        print("[%s] buy weapon [%s]" %(self.name,weapon))

    def info(self):
        print('''
        ------个人信息-------
            姓名：%s
            身份：%s
            武器：%s
            生命：%s
        ---------------------
        ''' %(self.name,self.role,self.weapon,self.life))

p1 = Role('jicki','警察','手枪',100)
p2 = Role('luck','匪徒','机关枪',100)
p1.info()
p2.info()
p1.buy_weapon('AK47')
p2.buy_weapon('狙击枪')
p1.info()
p2.info()



打印结果：
------个人信息-------
姓名：jicki
身份：警察
武器：手枪
生命：100
---------------------

------个人信息-------
姓名：luck
身份：匪徒
武器：机关枪
生命：100
---------------------

[jicki] buy weapon [AK47]
[luck] buy weapon [狙击枪]

------个人信息-------
姓名：jicki
身份：警察
武器：AK47
生命：100
---------------------

------个人信息-------
姓名：luck
身份：匪徒
武器：狙击枪
生命：100
---------------------
```


## 类的继承

```

class ShoolMember(object):
    def __init__(self,name,age,sex):
        self.name = name
        self.age = age
        self.sex = sex
        self.enroll()

    def enroll(self):
        print("[%s] 加入了学校" %self.name)

    def tell(self):                            # 所有类 公有方法
        print("大家好，我的名字叫[%s]" %self.name)

class Teacher(ShoolMember):       #继承了 ShoolMember 类
    def __init__(self,name,age,sex,coures,salary):    # 重写 ShoolMember 父类的构造方法__init__
        super(Teacher,self).__init__(name,age,sex)    # 新式类 的继承，继承了父类的__init__
        self.coures = coures
        self.salary = salary

    def teaching(self):                              # Tenacher 类 私有方法
        print("[%s] 是教 [%s] 课程" %(self.name,self.coures))

class Student(ShoolMember):    #继承 ShoolMember 类
    def __init__(self,name,age,sex,coures,tuition):    # 重写 ShoolMember 父类的构造方法__init__
        super(Student,self).__init__(name,age,sex)     # 新式类 的继承，继承了父类的__init__
        self.coures = coures
        self.tuition = tuition

    def pay_tuition(self):                           # Student 类 私有方法
        print("[%s] 学习 [%s] 课程交了 [%s] 学费" %(self.name,self.coures,self.tuition))

# 实例化
t1 = Teacher("alax",22,'F','Python',10000)
t2 = Teacher('wupeiqi',23,'F','Python',10000)
s1 = Student('jicki',22,'F','Python',6000)

t1.teaching()       # 调用 Teacher 私有方法
s1.pay_tuition()    # 调用 Student 私有方法

t1.tell()     # 调用共用 方法
s1.tell()     # 调用共用 方法


输出结果：
[alax] 加入了学校
[wupeiqi] 加入了学校
[jicki] 加入了学校
[alax] 是教 [Python] 课程
[jicki] 学习 [Python] 课程交了 [6000] 学费
大家好，我的名字叫[alax]
大家好，我的名字叫[jicki]

```


## 多态

```
Python 多态：
    多态性 允许子类类型的指针赋值给父类类型的指针。
    多态的作用是什么？  
    我们知道，封装可以隐藏实现细节，使的代码模块化；
    继承可以扩展已存在的代码模块（类）；
    封装与继承的目的都是为了 代码重用。
    而多态则是为了实现 接口重用，多态的作用就是为了类的继承和派生的时候，保证能使用类的成员中任意类的实例的某一属性时的正确调用。
    

Python 是不支持多态的，可以使用如下方法实现：

class Animal(object):
    def __init__(self,name):
        self.name = name
        
    def talk(self):
        raise NotImplementedError("Subclass must implement")
        
class Cat(Animal):
    def talk(self):
        return 'miao, miao !'
        
class Dog(Animal):
    def talk(self):
        return 'wang wang !'
        

def animal_talk(obj):
    print(obj.talk())
    
d = Dog("WangCai")
c = Cat("BoSi")

animal_talk(c)
animal_talk(d)


输出结果：
miao, miao !
wang wang !

```





# Python 反射


```
import sys
class WebServer(object):
    def __init__(self,host,port):
        self.host = host
        self.port = port

    def start(self):
        print('service start.....')

    def stop(self):
        print('service stop......')

    def restart(self):
        self.stop()
        self.start()


if __name__ == '__main__':
    server = WebServer('localhost',9999)

    if hasattr(server,sys.argv[1]):         #判断server这个objcet 中是否包含 sys.argv[1] ，既然用户输入的这个 方法
        fun = getattr(server,sys.argv[1])   #获取server 中这个objcet 的 sys.argv[1] ，既用户输入的这个 方法
        fun()         # 加上() 调用

```



# Python Socket

## 服务端例子

```
import socket

ip_port = ('127.0.0.1', 9999)   # IP 与 端口，在一个 元组里面

sk = socket.socket()          # socket 默认是 tcp 协议
sk.bind(ip_port)              # 绑定 IP 与 端口
sk.listen(5)                  # 服务端最大连接数

while True:
    print('等待连接....')
    conn,addr = sk.accept()                           #accpet 返回两个变量 conn 是客户端连接过来时创建的实例。addr 是客户端的IP地址。
    print('客户段连接IP ', addr)                      #当客户端连接过来时，打印 客户端的IP地址。
    client_data = conn.recv(1024)                     # client_data 这个变量等于 客户端发送的数据 (1024) = 1K 。
    print(str(client_data,'utf8'))                    # 打印客户端发送过来的信息，因为是中文所以python 3.0 中要使用 str 来声明 utf8
    conn.sendall(bytes('服务端发来消息...','utf8'))   # 发送这条信息到客户端,python 3.0 要使用 bytes 来发送, 并声明 utf8
    conn.close()                                 
```


## 客户端例子

```

import socket

ip_port = ('127.0.0.1', 9999)         # IP 与 端口，在一个 元组里面
sk = socket.socket()                  # socket 默认是 tcp 协议
sk.connect(ip_port)                   # 连接服务端 IP 与 端口

sk.sendall(bytes('客户端发来消息....','utf8'))            # 发送这条信息到服务端,python 3.0 要使用 bytes 来发送, 并声明 utf8
server_reply = sk.recv(1024)                            # server_reply 这个变量等于 客户端发送的数据 (1024) = 1K 。
print(str(server_reply,'utf8'))                         # 打印服务端发送过来的信息，因为是中文所以python 3.0 中要使用 str 来声明 utf8
sk.close()                                              # 关闭连接

```



# Python 异常处理


```
try:
    pass
except Exception as e:
    pass
```


## 常用的异常列表

```
AttributeError   试图访问一个对象没有的树形，比如foo.x，但是foo没有属性x

IOError    输入/输出异常；基本上是无法打开文件

ImportError    无法引入模块或包；基本上是路径问题或名称错误

IndentationError    语法错误（的子类） ；代码没有正确对齐

IndexError   下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]

KeyError   试图访问字典里不存在的键

KeyboardInterrupt   Ctrl+C被按下

NameError   使用一个还未被赋予对象的变量

SyntaxError   Python代码非法，代码不能编译(个人认为这是语法错误，写错了）

TypeError   传入对象类型与要求的不符合

UnboundLocalError   试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局变量，
导致你以为正在访问它

ValueError   传入一个调用者不期望的值，即使值的类型是正确的

```


## 抓住特定错误

```

s1 = input('>>>')
try:
    int(s1)
except KeyError as e:
    print('键错误')
except ValueError as e:
    print('Value 错误')
except Exception as e:
    print('错误: ', e )

```


## 异常在程序中的应用

```


try:
    # 主代码块
    pass
except KeyError as e:
    #出现KeyError时，执行如下程序
    pass
else:
    # 主代码块执行完，执行该块
    pass
finally:
    # 无论异常与否，都会执行该块
    pass
```


## 主动触发异常

```

try:
    raise Exception('抛出异常')
except Exception as e:
    print(e)
```


## 自定义异常

```

class MyException(Exception):    #创建一个类，继承 Exception

    def __init__(self, msg):
        self.message = msg

    def __str__(self):
        return self.message
 
try:
    raise MyException('我的异常')   #主动触发MyException
except MyException as e:
    print(e)
```


## 异常中的断言

```

class MyException(Exception):    #创建一个类，继承 Exception

    def __init__(self, msg):
        self.message = msg

    def __str__(self):
        return self.message

a = 1        #定义一个变量 

try:
    assert a == 2        # 断言 a 是否 等于 2 ,不等于就抛出 AssertionError 这个错误
except MyException as e:
    print(e)
```




# Python 线程的调用

> Python 启动线程有2种方法，分别为 直接调用 与 继承调用。


## 直接调用

```
import threading
import time

def sayhi(num):
    print('运行线程号为： %s' %num)
    time.sleep(3)

if __name__ == '__main__':
    t1 = threading.Thread(target=sayhi,args=(1,)) #创建一个线程 t1
    t2 = threading.Thread(target=sayhi,args=(2,)) #创建一个线程 t2

    t1.start()        #运行线程t1
    t2.start()        #运行线程t2

    print(t1.getName())    #打印线程t1名称
    print(t2.getName())    #打印线程t2名称

    t1.join()       #join 等待线程t1运行完毕再往下走
    t2.join()       #join 等待线程t2运行完毕再往下走
    
    print("运行完毕")
	
```

	

## 直接调用，多线程

```
import threading
import time

def sayhi(num):
    print('运行线程号为： %s' %num)
    time.sleep(3)

if __name__ == '__main__':
    t_list = []
    for i in range(10):       # 循环创建 10个 线程。
        t = threading.Thread(target=sayhi,args=[i,])
        t.start()
        t_list.append(t)      # 将创建的进程 写入 t_list 这个列表中。

    for i in t_list:         # 循环列表，让所有线程，join ,执行完毕。
        i.join()

    print("运行完毕")
	
```

## 继承的方式

```

import threading
import time

class MyThread(threading.Thread):    #定义一个类，继承 threading.Thread 这个父类。
    def __init__(self,num):
        threading.Thread.__init__(self)       # 重写父类的构造函数。
        self.num = num

    def run(self):  #定义每个线程要运行的函数
        print("线程运行号:%s" %self.num)
        time.sleep(3)

if __name__ == '__main__':

    t1 = MyThread(1)
    t2 = MyThread(2)
    t1.start()
    t2.start()
	
```


	
## 继承方式，增加join

```
import threading
import time

class MyThread(threading.Thread):    #定义一个类，继承 threading.Thread 这个父类。
    def __init__(self,num):
        threading.Thread.__init__(self)       # 重写父类的构造函数。
        self.num = num

    def run(self):  #定义每个线程要运行的函数
        print("线程运行号:%s" %self.num)
        time.sleep(3)
        print('线程 %s 运行完毕' %self.num)

if __name__ == '__main__':

    t1 = MyThread(1)
    t2 = MyThread(2)
    t1.start()
    t2.start()
    t1.join()
    t2.join()

```


## 多线程 Events

```
# Event 对象用于线程间通信，它是由线程设置的信号标志，如果信号标志位真，则其他线程等待直到信号接触。
# event = threading.Event() 生成实例
# event.wait() 等待设定，既 线程信号标签为 假, 阻塞。
# event.set()  设置标签，既 线程信号标签为 真，通行。
# event.clear() 清除设定。
# event.isSet() 判断是否有设定。


import threading
import time

def light():
    if not event.isSet():
        event.set()
    count = 0
    while True:
        if count < 10:
            print('\033[42;1m --绿灯-- \033[0m')
        elif count < 13:
            print('\033[43;1m --黄灯-- \033[0m')
        elif count < 20:
            if event.is_set():
                event.clear()
            print('\033[41;1m --红灯-- \033[0m')
        else:
            count = 0
            event.set()
            print('\033[42;1m --绿灯-- \033[0m')
        time.sleep(1)
        count += 1

def car(n):
    while 1:
        time.sleep(1)
        if event.isSet():
            print("汽车 [%s] 通过红绿灯 " %n)
        else:
            print("汽车 [%s] 等待红绿灯 " %n)
            event.wait()

if __name__ == '__main__':
    event = threading.Event()
    Light = threading.Thread(target=light)
    Light.start()
    for i in range(3):
        t = threading.Thread(target=car,args=(i,))
        t.start()
		
```


## 多进程(multiprocessing) 

```
# 并发 执行 一个例子:

from multiprocessing import Process
import time

def f(name):
    time.sleep(2)
    print("hello", name)

def f2(name):
    time.sleep(2)
    print("hello", name)

if __name__ == "__main__":
    p1 = Process(target=f, args=('bob1',))
    p2 = Process(target=f, args=('bob2',))
    p3 = Process(target=f2, args=('pop1',))
    p4 = Process(target=f2, args=('pop2',))
    p5 = Process(target=f2, args=('pop3',))
    p1.start()
    p2.start()
    p3.start()
    p4.start()
    p5.start()
    p5.join()
	
``` 



## 多进程间通讯 Queues

```
# 不同进程间内存是不共享的,要想实现进程间数据交换，可使用 Queues


from multiprocessing import Process,Queue

def f(q):
    q.put([1,None, 'hello'])
    q.put([2,None, 'word'])
	
if __name__ == '__main__':
    q = Queue()
    p = Process(target=f, args=(q,))
    p2 = Process(target=f, args=(q,))
    p.start()
    p2.start()
    print('p-1  get :' , q.get())
    print('p-2  get :' , q.get())
    print('p2-1 get :' , q.get())
    print('p2-3 get :' , q.get())
    p.join()

```


## 











