# Python基础（四）-文件读写、异常处理、正则表达式、多进程线程和套接字（Socket）的使用

[TOC]



## 一、文件的读写

文件读写的过程：打开文件  -->  判断大小  -->  分配内存 -->  读取/写入文件 -->  关闭文件

打开文件可以通过```open()```函数来打开

```python
def open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True)
```

| 模式 | 描述                                                         |
| :--: | ------------------------------------------------------------ |
|  r   | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
|  rb  | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。 |
|  r+  | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
|  w   | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
|  wb  | 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
|  w+  | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
|  a   | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
|  ab  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
|  a+  | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

### file 对象方法

```file.read([size])``` size未指定则返回整个文件,如果文件大小>2倍内存则有问题.f.read()读到文件尾时返回""(空字串)

```file.readline()``` 返回一行

```file.readlines([size]) ```返回包含size行的列表,size 未指定则返回全部行

```for line in f: print line  ``` #通过迭代器访问

```f.write("hello\n")``` #如果要写入字符串以外的数据,先将他转换为字符串.

```f.tell()``` 返回一个整数,表示当前文件指针的位置(就是到文件头的比特数).

```f.seek(偏移量,[起始位置])``` 用来移动文件指针.

- 偏移量:单位:比特,可正可负
- 起始位置:0-文件头,默认值;1-当前位置;2-文件尾

```f.close()``` 关闭文件

```python
# 按照一般思路的写法
def main():
    try:
        fs = open('test.txt', 'r', encode='utf-8')  # 打开一个文件流
        content = fs.read()  # 读取文件流
        for line in fs:  # 一行一行的遍历
            print(line, end='')  # 输出一行的内容
        fs.close()  # 关闭文件的输入输出流， 防止IO出错
	except (FileNotFoundError, IOError) as e:
        print(e)
        print('指定文件无法读取')
        
        
if __name__ == '__main__':
    main()
```

```python
# 采用Python上下文管理的语句with，with句柄内上下文内容中的__enter__ 和__exit__会被自动的执行，完成上下文的打开和退出。
def main():
    try:  # 进行异常获取
        with open('test.txt', 'r', encode='utf-8') as fs:  # 以一种安全的方式打开文件流
            my_list = fs.readlines()  # 获取每一行并存在列表中
            for line in my_list:  # 遍历整个列表并进行输出
                print(line, end='')
    except (FileNotFoundError, IOError) as e:  # 抓取到文件未找到和IO错误异常
        print(e)  # 输出错误消息
        print('指定文件无法读取')  # 打印错误提示


if __name__ == '__main__':
    main()
```

### 写入JSON数据格式、数据的解析和封装

将字典类型的数据封装成JSON对象并写入

```python
import json

def main():
    mydict = {
        'name': 'TengTengCai',
        'age': '22',
        'friends': ['BaiCai', 'QingCai', 'LUObO'],
        'car': [{'broad': 'BNW',
                'speed': 320},
                {'broad': 'Kawaisaki',
                'speed': 300},
                {'broad': 'KTM',
                'speed': 290}]
    }
    try:
        with open('data.json', 'w', encoding='utf-8') as fs:  # 打开需要写入的文件
            json.dump(mydict, fs)  # 封装为json数据格式并保存在文件中
    except (FileNotFoundError, IOError) as e:
        print(e)
        print('指定文件写入失败')
        
        
if __name__ == '__main__':
    main()
```

将文件中的JSON字符串读取并解析成字典对象，在控制台输出

```python
import json
def main():
    try:  # 抓取异常
        with open('data.json', 'r', encoding='utf-8') as fs:  # 获取文件输出流
            data = json.load(fs)  # 读取文件并转换为字典对象
            print(data)  # 打印出字典
    except （FileNotFoundError, IOError） as e:
        print(e)
        print('指定文件读取失败')
        
if __name__ in '__main__':
    main()
```

## 二、异常处理

异常处理机制的目的是为了，当程序出现了崩溃后，能够让程序继续运行的，并给以用户一个友好的提示，或者自己主动的引出异常，来限制用户错误的操作，并给出友好的提示。

```python
#  一个完整的异常获取处理过程
try：
	print('可能会出错的代码段')
except Exception as e:
    print('抓取到的错误为' + e)  # 错误处理的代码段
else：
    print('没有错误执行')
finally:
    print('无论错误与否都执行')
```

当然出来获取系统和程序发出的异常外，我们还可以自己引出异常。

```python
def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)  # 引出错误
    return 10 / n

def bar():
    try:
        foo('0')
    except ValueError as e:  # 抓取到错误
        print('ValueError!')  # 输出提示

bar() 
```

## 三、正则表达式

正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。

re 模块使 Python 语言拥有全部的正则表达式功能。 

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。 

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

详细的正则表达式语法请见：

[正则表达式30分]:http://deerchao.net/tutorials/regex/regex.htm

在Python中常用的正则表达式函数有```compile()  match() search() findall() finditer() split() sub() ```

```python
# 使用正则表达式将数字从字符串中提取出来
# re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；
# 而re.search匹配整个字符串，直到找到一个匹配。
import re

def main():
    string1 = '12443asd14aqw5fd68qwe7fa4s56'
    m = re.match(r'\d+', string1)  # 从头开始匹配
    print(m.group())  # 输出12443
    string2 = 'asd14aqw5fd68qwe7fa4s56'
    ms = re.search(r'\d+', string2)  # 从任意一处开始匹配
    print(ms.group()) # 输出14
    pattern = re.compile(r'\d+')
    m = pattern.match(string1)
    ms = pattern.search(string2)
    m_list = pattern.findall(string1)  # 匹配所有符合正则表达式的字符串，并返回一个列表
    for m in m_list:
        print(m, end=',')  # 输出所有的数字
    print()
    iter = pattern.finditer(string1)  # 用迭代器的方式，找到所有符合要求的字符串，节省空间费时间
    for temp in iter:  # 在使用的时候才会在内存中申请空间
        print(temp.group(), end='')  # 输出所有的数字
        
    sentence = 'You go your way, I will go mine'
    pure = re.sub(' ', '*', sentence)  # 对匹配的字符进行字符串替换
    print(pure)
    my_list = re.split(r'[ ,!]', sentence)  # 对匹配的字符进行字符串切片
    print(my_list)
    pass

if __name__ in '__main__':
    main()

```

## 四、多进程和多线程

进程（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础。在早期面向进程设计的计算机结构中，进程是程序的基本执行实体；在当代面向线程设计的计算机结构中，进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。

在Python中是能够使用多线程，来调度多核CPU来同时工作。

```python
import time
from multiprocessing import Process

# process  进程 启用多进程在控制台中一共输出Boom XiaKaLaKa，10次
count = 0
def output(string):
    global count
    inner_count = 0
    while count < 10:
        print(string, end='', flush=True)
        count += 1
        inner_count += 1
        time.sleep(0.5)
    print('\n%s打印了%d次\n' % (string, inner_count))


def main():
    Process(target=output, args=('Boom',)).start()  # 开启多进程调用start()方法
    Process(target=output, args=('XiaKaLaKa',)).start()


if __name__ == '__main__':
    main()

```

在Python中也能使用多线程来实现异步任务处理， 但是只能调用一个CPU来全力执行线程中的任务

```python
from threading import Thread
from time import sleep


def output():
    while True:
        print('Boom', end='', flush=True)
        sleep(0.001)


def main():
    Thread(target=output).start()  # 启用多线程调用output函数
    while True:
        print('XiaKaLaKa', end='', flush=True)
        sleep(0.001)


if __name__ == '__main__':
    main()

```

当然了，这样写，参数太长了，一般情况下，都是将线程写成一个类，这样，能对线程进行更加丰富的封装和定制

```python
from threading import Thread
from time import sleep
from random import randint


# 创建线程的两种方式
# 1. 直接创建Thread对象并通过target参数指定线程启动后要执行的任务
# 2. 继承Thread自定义线程 通过重写run方法指定线程启动后执行的任务

class PrintThread(Thread):  # 创建类， 继承线程

    def __init__(self, string, count):  # 初始化参数
        super().__init__()
        self._string = string  # 需要打印的字符串
        self._count = count  # 计数

    def run(self):
        for _ in range(self._count):
            print(self._string, end='', flush=True)
            sleep(randint(0,2))  # 随机休眠，让每个线程都有占用CPU资源的机会


def main():
    PrintThread('Boom', 100).start()  # 开启自定义的线程
    PrintThread('XiaKaLaKa', 100).start()


if __name__ == '__main__':
    main()

```

下面我们来模拟一个下载文件的行为

```python
import time
import random
from threading import Thread
from multiprocessing import process


# 如果多个任务之间没有任何的关联（独立子任务）而且希望利用cpu的多核特性
# 那么我们推荐使用多进程


class DownloadTask(Thread):
    def __init__(self, filename):  # 初始化线程构造方法
        super().__init__()
        self.filename = filename

    def run(self):
        print('开始下载%s...' % self.filename)
        delay = random.randint(5, 15)  # 随机数模拟下载时间
        time.sleep(delay)  # 线程休眠
        print('%s下载完成，用时%d秒.' % (self.filename, delay))


def main():
    start = time.time()  # 保存开始时间
    t1 = DownloadTask('Python从入门到住院.pdf')
    t2 = DownloadTask('Android从入门到放弃.pdf')

    t1.start()  # 开启线程
    t2.start()
    t1.join()  # 等待子线程执行完成
    t2.join()
    end = time.time()  # 保存结束时间
    print('总共耗费了%f秒' % (end - start))  # 计算耗费的时间


if __name__ == '__main__':
    main()
```

资源的保护，当我们同时开启多个线程同时去访问一个资源的时候，那个资源就是一个临界资源，临界资源，是无法正确的被多线程给读写的，这时候就资源添加一个锁，当有线程在使用的时候就将资源锁起来，使用完后又释放出去，这样就能保证数据的有效读写。

```python
import time
from threading import Thread, Lock


class Account(object):  # 账户类
    def __init__(self):  # 初始化账户
        self._balance = 0
        self._lock = Lock()

    @property
    def balance(self):  # 获取余额
        return self._balance

    def deposit(self, money):  # 存钱
        # 当多个线程同时访问一个资源的时候，就有可能因为竞争，导致资源的状态错误
        # 被多个线程访问的资源，我们通常称之为临界资源，对临界资源的访问需要加上保护
        if money > 0:
            self._lock.acquire()  # 开启资源的锁保护
            try:
                new_balance = self._balance + money
                time.sleep(0.01)
                self._balance = new_balance
            finally:
                self._lock.release()  # 释放资源的锁保护


class AddMoneyThread(Thread):  #  存钱线程
    def __init__(self, account):  # 初始化传入账户对象
        super().__init__()
        self._account = account

    def run(self):
        self._account.deposit(1)  #  线程执行时存入一块钱


def main():
    account = Account()  # 新建账户对象
    tlist = []  # 线程列表
    for _ in range(100):  # 循环100次，进行存钱操作
        t = AddMoneyThread(account)  # 新建一个线程，传入账户对象
        tlist.append(t)  # 添加 到线程列表中
        t.start()  # 开起线程
    for t in tlist:
        t.join()  #  等待线程执行结束
    print('账户余额%d元' % account.balance)  # 打印出当前账户的余额


if __name__ == '__main__':
    main()

```

## 五、套接字（Socket）编程

网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个socket。

建立网络通信连接至少要一对端口号(socket)。socket本质是编程接口(API)，对TCP/IP的封装，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口；HTTP是轿车，提供了封装或者显示数据的具体形式；Socket是发动机，提供了网络通信的能力。



```python
# 服务端，等待客户端的接入

from socket import socket, AF_INET, SOCK_STREAM  # 需要导入socket包
from time import sleep, localtime, time
from datetime import datetime


def main():
    # 创建基于TCP协议的套接字对象
    # 因为我们做的是应用级产品或服务所以可以利用现有的传输服务来实现数据传输
    server = socket(AF_INET, SOCK_STREAM)  # 这两个是默认参数，默认的TCP连接，也可以直接socket()来创建对象
    # 绑定IP地址（网络上主机的身份标识）和端口（用来区分不同服务的IP地址扩展）
    server.bind(('10.7.189.55', 6310))
    # 开始监听客户端的连接，队列数量为512
    server.listen(512)
    print('服务器已经启动正在监听...')
    client, addr = server.accept()  # 开始等待接入
    while True:
        # 通过accept方法接收客户端的连接
        # accept方法是一个阻塞式的方法 如果没有客户端连接上老
        # 那么accept方法就好让代码阻塞 直到有客户端连接成功才返回对象
        # accept方法返回的一个原则，元组中的第一个值是代表客户端的对象
        # 元组中的第二值又是一个元组 其中有客户端的IP地址和客户端的地址
        # client, addr = server.accept()
        print(addr, '连接')
        date_time = datetime
        print(client.recv(512).decode('utf-8'))
        message = input("需要的发的消息")
        client.send(str(message).encode('utf-8'))
        # client.close()


if __name__ == '__main__':
    main()

```

```python
# 客户端的连接
from socket import socket


def main():
    client = socket()  # 创建socket对象
    client.connect(('10.7.189.55', 6310))  # 连接服务器地址
    data = client.send('你1234e44好'.encode('utf-8'))  # 发送消息
    print(type(data))
    print(data)
    client.close()  # 关闭连接


if __name__ == '__main__':
    main()

```

服务器的输出如下：

服务器已经启动正在监听...
('10.7.189.55', 50984) 连接
你1234e44好



这样就完成了一个简单的Socket连接

更全面的P2PSocket聊天代码如下

[代码地址]：https://github.com/TengTengCai/PythonP2PSocket