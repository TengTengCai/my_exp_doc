[TOC]

# Python装饰器详解

说到装饰器，它是个什么东西有什么作用，如何编写一个装饰器，具体的应用场景又有哪些呢？下面一一进行讲解。

## 一、什么是装饰器？

顾名思义，装饰器就是用来修饰某个函数，在不改变原来方法代码的前提下，额外的附加其他的功能和属性。可能这样说得比较抽象，下面我们通过代码具体的演示如何编写装饰器，装饰器如何执行的。

## 二、编写装饰器

装饰器也可以分为两种，无参装饰器和有参装饰器

### 无参装饰器

```python
"""
无参装饰器示例
AUTH：TTC
DATE：2018年6月4日 23:58:12
VERSION:v0.01
"""
from functools import wraps


# 装饰器函数
def decorator(fn):
    # 这里加装饰器的意思是，当查看调用方法时，依然显示目标函数的方法名，而不是wrapper
    @wraps(fn)
    def wrapper(*args, **kwargs):  # 给目标函数装饰的方法
        print('foo')  # 在执行目标函数之前运行
        fn(*args, **kwargs)  # 执行目标函数
        print('bar')  # 在目标函数之后运行

    return wrapper  # 返回装饰后的函数


@decorator
def hello_decorator(name):
    """
    需要装饰的函数
    """
    print('hello_decorator, My name is %s' % name)


def main():
    hello_decorator('TTC')
    print(hello_decorator.__name__)


if __name__ == '__main__':
    main()

```

代码执行的顺序是这样的，先进入`mian()`方法，执行`hello_decorator()`发现被`decorator(fn)`方法装饰`hello_decorator()` 该方法被当做参数传递给装饰器函数，而`hello_decorator()`的`name`参数会被`wrapper()`接收(`*args`, `*kwargs`, 为可变参数和关键字参数， 可接受任意形式的参数),将wrapper函数会返回给主函数,从而对`hello_decorator()`函数添加了额外的功能，先输出foo，再执行目标函数，最后输出bar。（在这儿的重点是将函数也作为了一个参数进行传递）

```bash
[tianjun@localhost test]$ python3 example06.py 
foo
hello_decorator, My name is TTC
bar
hello_decorator
```

这样`hello_decorator()`就被赋予了额外的功能，打印foo然后执行函数，执行完后再打印bar。

### 有参装饰器

有参数的装饰器和没参数的构造器的区别在于，多了一层函数的嵌套。

```python
"""
有参装饰器
AUTH: TTC
DATE: 2018年6月5日 17:27:35
"""
from functools import wraps


# 装饰器函数
def decorator(times):
    # 外部装饰器
    def out_wrapper(fn):
        # 内部装饰器
        @wraps(fn)
        def wrapper(*args, **kwargs):
            print('foo')  # 目标函数之前执行
            for _ in range(times):  # 执行次数
                fn(*args, **kwargs)  # 执行目标函数
            print('bar')  # 目标函数之后执行

        return wrapper  # 返回内部装饰器

    return out_wrapper  # 返回外部装饰器


@decorator(times=3)  # 带参数的装饰器糖果语法
def hello_decorator(name):  # 被装饰函数
    """
    需要修饰的函数
    """
    print('hello_decorator, My name is %s' % name)


def main():
    hello_decorator('TTC')
    print(hello_decorator.__name__)


if __name__ == '__main__':
    main()

```

OK,写完了，你以为就结束了吗，NO，NO，NO，如果你知道Python之禅，扁平的比嵌套的好，就知道，这样写的带参装饰器太丑了，我们需要减少嵌套，使语句扁平化。我们可以通过类来实现带参数的装饰器。

```python
"""
类有参装饰器
AUTH: TTC
DATE: 2018年6月5日 17:27:35
"""
from functools import wraps


# 装饰器类
class Decorator(object):
    def __init__(self, times):  # 初始化方法，传递参数
        self._times = times  # 参数赋值

    def __call__(self, fn):  # 魔法方法，当类被调用时执行
        @wraps(fn)  # 保留原方法函数名
        def wrapper(*args, **kwargs):  # 装饰器方法
            print('foo')  # 在目标函数之前运行
            for _ in range(self._times):  # 执行多少次
                fn(*args, **kwargs)  # 执行目标函数
            print('bar')  # 在目标函数之后运行
        return wrapper  # 返回装饰猴年的方法


@Decorator(times=3)  # 带参数的装饰器糖果语法
def hello_decorator(name):  # 被装饰函数
    """
    需要修饰的函数
    """
    print('hello_decorator, My name is %s' % name)


def main():
    hello_decorator('TTC')
    print(hello_decorator.__name__)


if __name__ == '__main__':
    main()

```

### 装饰有返回值的函数

如果被装饰的函数有返回值，则需要对函数进行小小的调整。将返回的数值在变量中进行存储，然后再通过装饰器函数返回。

```python
# 装饰器函数
def decorator(fn):
    # 这里加装饰器的意思是，当查看调用方法时，依然显示目标函数的方法名，而不是wrapper
    @wraps(fn)
    def wrapper(*args, **kwargs):  # 给目标函数装饰的方法
        print('foo')  # 在执行目标函数之前运行
        result = fn(*args, **kwargs)  # 执行目标函数, 将执行结果赋值给变量
        print('bar')  # 在目标函数之后运行
        return result  # 返回执行的结果
    return wrapper  # 返回装饰后的函数
```

带参数的装饰器也是同理的。

```python
# 装饰器类
class Decorator(object):
    def __init__(self, times):  # 初始化方法，传递参数
        self._times = times  # 参数赋值

    def __call__(self, fn):  # 魔法方法，当类被调用时执行
        @wraps(fn)  # 保留原方法函数名
        def wrapper(*args, **kwargs):  # 装饰器方法
            print('foo')  # 在目标函数之前运行
            value = None  # 声明变量
            for _ in range(self._times):  # 执行多少次
                value += fn(*args, **kwargs)  # 执行目标函数，将执行结果进行累加
            print('bar')  # 在目标函数之后运行
            return value  # 返回累加的值

        return wrapper  # 返回装饰猴年的方法
```

## 三、装饰器的使用场景

装饰器的使用场景太多了，只要是你不想改变原来的代码，又想添加额外的功能，都可以使用装饰器来完成。

比如，我们在测试一个方法的执行时间上，就可以使用装饰器来实现。

```python
import time
from functools import wraps 


def spend_time(fn):
    @wraps(fn)
    def wrapper(*args, **kwargs):
        start_time = time.time()  # 开始时间
        result = fn(*args, **kwargs)  # 执行函数
        end_time = time.time()  # 结束时间
        print('执行时间：%s' % str(end_time-start_time))  # 输出花费时间
        return result
    return wrapper
```

又比如，当我们在进行数据库的操作时，又时会因为网络原因，或其他原因，导致我们的数据库操作出错，为了不让数据丢失，程序崩溃，一般会写一个装饰器来处理这样的问题。在装饰器中 对错误进行抓取，然后休息几秒再次的提交尝试，知道多吃尝试依然无法完成操作再丢弃。

在这儿用一个Redis的常见错误来解释一下

```python
def db_retry_conn(fn):
    """
    数据库链接重新尝试
    :param fn: 函数
    :return: 执行装饰后的函数
    """
    def wrapper(*args, **kwargs):
        # 尝试3次
        for i in range(3):
            try:
                # 无异常正常返回函数
                return fn(*args, **kwargs)
            except (ConnectionError, BusyLoadingError, DataError) as e:
                # 抓取Redis的连接错误
                logging.error(e)  # 打印错误日志
                print(f'{current_thread().getName()},Redis操作出现错误3秒后尝试重新操作， 第{i+1}次尝试')
                time.sleep(3)

    return wrapper
```

装饰器的应用场景太多了，在这就不一一说了，总之装饰器是个很好的设计，它不仅减少了重复的代码，使代码复用性提高，而且使得代码变得更加的结构化，更容易阅读。







