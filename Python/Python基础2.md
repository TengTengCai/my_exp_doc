# Python 基础（二）

[TOC]




## 一、字符串

字符串是 Python 中最常用的数据类型。我们可以使用引号('或")来创建字符串。

创建字符串很简单，只要为变量分配一个值即可。例如：

```python
str1 = 'hello, world! '
    print(len(str1))			# 获取字符串的长度
    print(str1.capitalize())	# 字符串首字母大写
    print(str1.upper())			# 字符串全部大写
    print(str1.lower())			# 字符串全部小写
    print(str1)
    print(str1.find(''))		# 查找字符的坐标，没找到会返回-1
    print(str1.index('lo'))		# 也是查找字符的坐标，没找到会报错
    print(str1.startswith('he'))	# 判断开始的字符是否是‘he’
    print(str1.startswith('HEl'))
    print(str1.endswith('o'))		# 判断结束的字符是否是‘o’
    print(str1.center(50, '*'))		# 生成50个字符，str1居中，其余的用*号填充
    print(str1.rjust(50, '-'))		# 生成50个字符，str1居左，其余的用-号填充
    print(str1.ljust(50, '_'))		# 生成50个字符，str1局右，其余的用_来填充
    str2 = 'abc123456'
    print(str2[2: 5])			# 对字符串进行切片，结果为c12
    print(str2[-1: -3: -1])		# 对字符串进行切片从后往前切，结果为65
    print(str2[-3: -1])			# 对字符串进行切片从倒数第三个开始切，结果为45
    print(str2[2:])			# 对字符串进行切片，2以后的所有字符切下来，结果为c12345
    print(str2[2::2])		# 对字符串进行切片，从2开始，每隔两个字符切一次 结果为c246
    print(str2[::2])	# 对字符串进行切片，从头开始，每隔两个字符切一次 结果为ac246
    print(str2[::-1])	# 对字符串进行切片，从末尾开始倒序输出
    print(str2.isdigit())  	# 判断字符串是否是数字
    print(str2.isalpha())	# 判断字符串是否是字母
    print(str2.isalnum())	# 判断字符串是否是字母数字
    str3 = '  1021766585@qq.com '
    print(str3)
    print(str3.strip())		# 清除开头和末尾的空格
```

例子

实现跑马灯效果

```python
import os
import time
def trotting():
    str1 = '欢迎来到藤藤菜的技术博客'
    str2 = str1.center(50, '*')
    while True:
        str2 = str2[1:] + str2[0]  # 这里是最核心的步骤将字符串的第一个字符添加到最后
        print(str2)
        time.sleep(0.5)
        os.system('cls')
def main():
    trotting()
if __name__ in '__main__':
    main()
```

生成指定长度的验证码

```python
def verification(code_len=4):
    """
    生成指定长度的验证码

    :param code_len: 验证码长度，正整数
    :return: 验证码字符串
    """
    code = ''
    for _ in range(code_len):
        my_type = randint(0, 2)		# 随机大小写和数字
        if my_type == 0:
            code += chr(randint(48, 57)) # 随机数字
        elif my_type == 1:
            code += chr(randint(97, 122)) # 随机小写
        else:
            code += chr(randint(65, 90)) # 随机大写
    return code
```

获取文件名后缀

```python
def get_suffix2(file_name, has_dot=False):
    """
    获取文件名后缀

    :param file_name: 文件的名称
    :param has_dot: 是否带文件名后缀的点，需要就传True，默认为False
    :return: 返回文件名后缀
    """
    index = str(file_name).rfind('.')  		# 从右开始查找点
    if 0 < index < len(file_name) - 1:		# 点不在开始和结尾的地方
        index = index if has_dot else index + 1 # 判断是否需呀加点后缀
        return file_name[index:]		# 返回切片的字符串
    else:
        return ''		#返回空字符
```

## 二、列表

序列是Python中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置或索引，第一个索引的0， 第二个索引的1，依次类推。

Python有6个序列的内置类型，但最常见的是列表和元组。

序列都可以进行的操作包括索引、切片、加、乘、检查成员。

此外，Python已经内置确定序列长度以及确定最大和最小的元素的方法。

列表是最常用的Python数据类型，它可以作为一个方括号内的逗号分隔值出现。

列表的数据项不需要具有相同的类型，创建一个列表，只需要把逗号分隔的不同的数据项使用方括号括起来即可。例如

### 1. 基本操作

```python
list1 = ['hello', 'world', 1996, 2018]
list2 = [1, 2, 3, 4, 5]
list3 = ['a', 'b', 'c', 'd', 'e']
```

```python
def test_list():
    """列表的相关使用"""
    list1 = [100, 200, 500]
    # 三种遍历列表的方式
    # 第一种通过索引来遍历
    for index in range(len(list1)):
        print(list1[index])
    # 第二种通过In从列表中取出元素
    for val in list1:
        print(val)
    # 第三种通过枚举的方式
    for index, val in enumerate(list1):
        print('索引为%d的元素为%d' % (index, val))
    list1.append(123) # 在列表的最后追加一个值为123的元素
    list1.insert(1, 300) # 在列表索引为1的位置插入一个值为300 的元素，其后的元素依次向后移动
    if 500 in list1:
        list1.remove(500) # 移除某个元素需要进行IF判断是否存在于列表中，如果存在就删除不存在就不删除，如果不进行判断强行删除不存在的元素，会导致程序运行错误
    del list1[3] # 移除某个索引位置的元素也可以直接使用 del 关键字
    if 100 in list1:
        list1.index(100, 0, 2) # 查找某个元素的索引需要判断其是否存在于列表中，如果存在就返回对应索引，不存在就跳过，如果强行查找不存在元素的索引会导致程序运行错误，第二个参数是从哪开始查找，第三个参数是从哪开始结束。
    list1.pop() # 默认从list1中取出最后一个元素,及参数为-1，如果为2就取出list1中索引为2的元素。取出的意思是从列表中拿出来，pop()方法的返回值就是取出的元素。
    print(list1)
```

接下来我们写个小程序，输出斐波那契数列 

```python
def fibonacci(num):
    """斐波那契数列"""
    list1 = [1, 1]
    for _ in range(2, num):
        length = len(list1)
        lsit1.append(list1[length] + list1[length -1])
     return list1
def main():
    print(fibonacci(20))
if __name__ == '__main__':
    main()
```

### 2. 列表的排序与最大最小

```python
def string_list():
    """
    Python内置的排序方法默认都是排升序（从小到大）
    如果希望排列成降序（从大到小）可以通过reverse参数来指定
    python中的函数几乎都是没有副作用的函数
    调用函数后不会影响传入的参数
    """
    l1 = ['orange', 'apple', 'banana', 'red', 'blue', 'yellow']
    l2 = [12, 59, 56, 46, 35, 34, 38, 14, 413, 4565, 65, 12, 47]
    l3 = sorted(l1, reverse=True) # python中自带的排序函数，第二个参数为是否为倒序,会返回一个新的列表对象
    print(l3)
    print(l1)
    l1.sort(reverse=True) # list对象中的方法，对原有的对象进行排序
    print(l1)
   	l4 = list(reversed(l1)) # 传递一个l1的倒序生成器，生成一个新的列表对象
    print(l4)
    print(max(l2)) # 输出l2中最大的元素
    print(min(l2)) # 输出l2中最小的元素
    
```

### 3. 列表的创建方式

列表的创建方式分为三中

1. 使用list对象，传递进一个生成器，或使用方括号直接写入个数
2. 使用生成表达式
3. 使用生成器

```python
import sys
def main():
    list1 = list(range(1, 10))			# 使用list对象生成列表
    print(list1)
    list2 = [1, 2, 3, 4, 5, 6, 7, 8, 9]	# 直接声明
    print(list2)
    list3 = [x for x in range(1, 10)]	# 使用生成表达式，生成表达式语法，创建列表容器，使用这种语法创建列表后，元素已经准备就绪，需要小号过多的内存空间
    print(list3)
    print(sys.getsizeof(list3))
    list4 = [x ** 2 for x in range(1, 10)]
    print(list4)
    list5 = (x for x in range(1, 10))	# 使用生成器，这里得到的不是一个列表，而是一个生成器对象，通过生成器可以获取到数据，它不占用额外的空间存储数据，每次需要数据的时候就通过内部的运算得到数据，当然这需要花费更多的时间
    print(list5)
    print(sys.getsizeof(list5))
    list6 = [x + y for x in 'ABCDE' for y in '1234567']
    print(list6)
if __name__ == '__mian__':
    main()
```

来写个小程序，输入年月日，求当天是当年的第几天

```python
def is_leap_year(year):
    """判断是否为闰年"""
    return year % 4 == 0 and year % 100 != 0 or year % 400 == 0

def which_day(year, month, date):
    """
     计算传入的日期是这一年的第几天

     :param year: 年
     :param month: 月
     :param date: 日
     :return:
     """
    days_of_mouth = [[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
                     [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]][is_leap_year(year)]
    total = 0
    for index in range(month - 1):
        total += days_of_mouth[index]
    return total + date

def main():
    print(which_day(2018, 2, 23))
    pass

if __name__ == '__main__':
    main()
```



## 三、元组

Python的元组与列表类似，不同之处在于元组的元素不能修改。

元组使用小括号，列表使用方括号。

元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可。例如

```python
tup1 = ('hello', 'world', 1996, 2018)
tup2 = (1, 2, 3, 4, 5)
tup3 = 'a', 'b', 'c', 'd'
```

元组的基本操作

```python
def mian():
    tup1 = (1, 2, 2, 3, 3, 3)
    print(len(tup1))     # 元组的长度
    print(tup1[0])		# 元组的第一个元素
    print(tup1.count(3))	# 值为3的元素个数
    if 2 in tup1:
        tup1.index(2, 0, 5) 	# index需要进行if判断在元组中是否存在，如果不存在就输出else或跳过，存在就返回对应的索引

if __name__ == '__main__':
    main()
```

## 四、集合

​	集合（简称集）是数学中一个基本概念，它是集合论的研究对象，集合论的基本理论直到19世纪才被创立。最简单的说法，即是在最原始的集合论——朴素集合论中的定义，集合就是“确定的一堆东西”。集合里的“东西”，叫作元素。

​	由一个或多个确定的元素所构成的整体叫做集合。集合中的元素有三个特征：1.确定性（集合中的元素必须是确定的）。 2.互异性（集合中的元素互不相同）。例如：集合A={1，a}，则a不能等于1）。  3.无序性（集合中的元素没有先后之分），如集合{3,4,5}和{3,5,4}算作同一个集合。

集合的基本操作

```python
def main():
    set1 = {1, 3, 3, 5, 7, 9} # 定义集合
    set1.add(4)				# 添加元素
    set1.update([11, 13])	 # 批量添加
    if 3 in set1:			
        set1.remove(3)		# 判断是否存在再进行删除对应元素
    print(set1)
    set2 = {2, 4, 6, 8, 10, 12}
    print(set2)
    set3 = set1 & set2		# 运算符求交集
    set3 = set1.intersection(set2) # 使用函数求交集
    print(set3)
    set4 = set1 | set2		# 运算符求并集
    set3 = set1.union(set2)	# 使用函数求并集
    print(set4)
    set5 = set1 - set2		# 运算符求差集
    set3 = set1.difference(set2) # 函数求差集
    print(set5)
    set6 = set1 ^ set2		# 与非运算
    set3 = set1.symmetric_difference(set2)
    print(set6)
    print(set2.issubset(set1))		# 是否是子集
    print(set1.issuperset(set2))	# 是否是超集
    list1 = list(set1)		# 集合转列表
    tup1 = tuple(set1)		# 集合转元组
    set1 = set(list1)		# 列表转集合
    set1 = set(tup1)		# 元组转集合
    pass
if __name__ == '__main__':
    main()
```

## 五、字典

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值(key=>value)对用冒号(**:**)分割，每个对之间用逗号(**,**)分割，整个字典包括在花括号(**{})**中 ,格式如下所示：

```python
d = {key1 : value1, key2 : value2 }
```

键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例：

```python
dict = {'刘备': 90, '张飞': 84, '关羽'： 99, '曹操'： 95}
```

字典的基本操作

```python
def main():
    dict1 = {'刘备': 90, '张飞': 84, '关羽'： 99, '曹操'： 95}
    print(dict1['刘备'])				# 获取值
    dict1['张飞'] = 60				# 修改值
    print(dict1)			
    dict1.update({'典韦': 85, '徐晃': 70})	# 批量添加
    print(dict1.pop('曹操'))				# 取出一个键值对，返回对应值
    del dict1['典韦']						# 删除对应键值对
    for x in dict1:
        print(x, '------->', dict1[x])		#遍历字典
    if '许褚' in dict1:					# 安全正确的值的获取方法。需判断是否存在对应的键值对
        print(dict1['许褚'])
    pass
if __name__ == '__main__':
    main()
```

## 六、模块的导入

模块的导入有两种方法：

1. ```import abc```  这里导入了一个叫abc的模块，当你需要使用的时候需要通过```abc.foo()```,来调用函数或方法。

2. ```from abc import foo```在这里导入了一个叫abc模块的foo函数或方法，在调用的时候可以直接使用```foo()```直接的调用，当然如果想使用模块中的所有函数和方法需要在import后加上*号```from abc import *```

   > 注意：导入模块的顺序是有规范的，当然每个公司的规范不一样，一般情况下会将Python内置的库按照字母顺序排在最前，然后空一行将第三方库按照字母顺序排在其后，最后空一行将自己写的库按照字母顺序排在最后。这样写的目的是为了他人在检查和修改你代码时能更好的检查你的库文件是否引入完整。
   >
   > 例如：
   > ```python
   > from math import sqrt # Python内置库
   >
   > from selenium import webdrive # Python第三方库
   >
   > from clock import show_time # Python自定义模块
   > ```
   >  如果发生函数方法名冲突，会默认调用后声明的方法，可以只有import的方式来解决，在模块后面添加一个```as```来另命名为其他字符，该字符的使用当然只在当前文件有效。
   >  ```python
   >  import combination as com
   >  ```

小游戏

二十一根火柴

```python
def match_game():
    total = 21
    while total > 0:
        print('总共还有%d根火柴' % total)
        while True:
            num = int(input('拿几根火柴：'))
            if 1 <= num <= 4 and num <= total:
                break
        total -= num
        if total > 0:
            com = randint(1, min(4, total))
            print('计算机拿走了%d根火柴' % com)
            total -= com
            if total == 0:
                print('计算机拿到最后一根火柴，你赢了')
        else:
            print('你拿到最后一根火柴，你输了！')

```
## 七、函数的使用

我们可以把程序中相对独立的功能和模块抽取出来，这样我们就不必重复的编写代码，而且将来需要使用相应功能的函数直接调用就可以了。

```python
# y = f(x) f是函数名 x是自变量 y是因变量
def f(x):
    """
    计算阶乘
    """
    y = 1
    for z in range(1, x + 1):
        y *= z
    return y
```

这样我们就成功的定义了一个函数。

那么如果我的函数有时需要一个参数有时需要两个参数，或者未知个参数的时候怎么办呢。

在其他的编程语言中，例如JAVA 、C++这类面向对象编程语言中可以通过函数的重载来实现，就是可以有相同名字的函数而传递不相同个数或类型的参数。而在Python中只需要定义一个方法就能实现传递不同个数的参数。

```python
from math import sqrt
def is_prime(num=100):
    """判断是否是素数"""
    for i in range(2,int(sqrt(num))+1):
        if num % i == 0:
            return False
    return True if num != 1 else False
def main():
    is_prime(15) # 传递参数
    is_prime() # 不传递参数，使用默认的参数
if __name__ == '__main__':
    main()
```

### 函数中的可变参数

那么如果我们不知道需要传递多少个参数时可以通过可变参数来实现。

```python
# 在参数前使用*表示args是可变参数
# 那么调用add函数时传入的参数个数是可变的
# 函数的参数可以是0个或任意多个
def add(*args):
    total = 0
    for value in args:
    	total += value
    return total
add()
add(1)
add(1, 2)
add(1, 2, 3)
add(1, 2, 3, 4)
```

```python
#可变参数 - 元组 - 可通过佛如In来获取元组中的值
#关键字参数 - 字典 - 根据参数名来决定如何执行
def say_hello(*args, **kwargs):  # 传递一个可变参数和一个关键字参数，这样能接受所有类型任意数量的参数
    print(args)
    print(kwargs)
    for key in kwargs:  # for循环遍历字典的键
        print(key, '--->', kwargs[key])  # 通过键来获取到对应的值
    if 'name' in kwargs:  # 如果只取某一个值，需要先进行判断是否存在这个键，如果不存在这个键强行读取会导致取值错误
        print('你好， %s！' % kwargs['name'])
    elif 'age' in kwargs:
        age = kewargs['age']
        if age < 18:
            print('未成年')
        else:
            print('成年')
     else:
        print('请输入个人信息')

# 命名关键字参数， 在‘*’后的参数必须添加关键字传递
def foo(a, b, c, *, name, age):
    print(a + b + c)
    print(name, '--->', age)
    
def main():
    info = {'name': '藤藤菜', 'age': 18, 'tel': '123456789'}
    my_list = [1, 2, 3, 4, 5]
    say_hello(*my_list, **info)  # 在字典变量名的前面添加一个‘*’将列表变量当作可变参数传递给函数，添加两个‘*’表示将字典当作关键字参数传递给函数
    foo(1, 2, 3, name='teng', age=22)
    foo(1, b=2, c=3, name='teng', age=22)
    foo(a=1, b=2, c=3, name='teng', age=22)
    pass

if __name__ == '__main__':
    main()
```




 判断是否是回文数

```python
# 一个数判断是否是回文数
def is_palindrome(num):
    """
    判断是否是一个回文数

    :param num: 需要判断是数字
    :return: 返回boolean类型，是就返回True 不是就是False
    """
    total = 0
    temp = num
    while temp > 0:
        total = total * 10 + temp % 10
        temp //= 10
    return num == total
```

判断是否是回文素数

```python
number = int(input('请输入一个非负整数'))
    if is_palindrome(number) and is_prime(number):
        print('%d 是回文素数' % number)
    else:
        print('%d 不是回文素数' % number)
```

> ```
> 注意：and和or都是带短路功能的运算符
> 如果and左边的表达式为False，那么右边的表达式会被短路 
> 如果or左边的表达式为True，那么有变动表达式会被短路
> 索引左右两边的表达式放置的顺序可能会被执行效率产生明显的影响
> ```

### 将函数做为参数传递

通过向函数中传入函数，可以写出更通用的代码

```python
# 例如对列表进行累加，累减，累乘，累除时，可以传递一个加减乘除的函数进去进行操作
def add(x, y):  # 加
    return x + y

def sub(x, y):  # 减
    return x - y

def mul(x, y):  # 乘
    return x * y

def divs(x, y):  # 除
    return x / y
# calc函数中的第二参数是另一个函数，它代表了一个二元运算
# 这样calc函数就不需要跟某一种特定的二元运算耦合在一起
# 所以calc函数变得通用性更强，可以由传入的第二个参数来决定到底做什么
def calc(my_list, op):
    total = my_list[0]
    for index in range(1, len(my_list)):
        total = op(total, my_list[index])
    return total

def main():
    my_list = [1, 2, 3, 4, 5]
    print(calc(my_list, add))  # 加
    print(calc(my_list, sub))  # 减
    print(calc(my_list, mul))  # 乘
    print(calc(my_list, divs))  # 除
    pass

if __name__ == '__main__':
    main()

```

### 将函数作为返回值

```python
# 定义一个函数
def bar():
    def foo(a, b, c, *, name, age):  # 函数中的方法
        print(a + b + c)
        print(name, '-->', age)
    return foo  # 返回该函数中的方法

def main():
    x = bar()  # 将返回的方法赋值给x
    x(1, 2, 3, name='teng', age=18)  # 这是调用方法就需要使用x变量来传递相应的参数
    pass

if __name__ == '__main__':
    main()
```

### 装饰器

装饰器本质上是一个Python函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就能抽离出大量与函数功能本身无关的雷同代码并继续重用。概括讲，装饰器的作用就是为已经存在的对象添加额外的功能。

```python
def record(fn)：
	def wrapper(*args, **kwargs):
        print('准备执行%s函数' % fn.__name__)
        print(args)
        print(kwargs)
        val = fn(*args, **kwargs)  # 执行被修饰的函数，在这段代码的前后，可以添加一些额外的代码，而这些额外的代码可以让我们在执行函数时做一些额外的工作
        print('%s函数执行完成' % fn.__name__)
        return val
    return wrapper

# 通过装饰器装饰foo函数，让foo函数在执行过程中可以做更多额外的事情
@record
def foo(n):
    if n == 0 or n == 1:
        return 1
    return n * f(n - 1)

def main():
    print(foo(5))
    pass

if __name__ == '__main__':
    main()
```





### 递归的使用

和其他编程语言一样，python也有递归函数,比如之前的求和是通过for循环来实现的，现在我们来通过递归来实现。

```python
def get_sum(x):
    if x== 0 or x == 1:
        return 1
    return x + get_sum(x - 1)
get_sum(100)
```

函数的递归原则

1. 收敛条件，让递归在有限次数内就能完成回溯（如果递归无法在有限次数内收敛就有可能导致RecursionError）
2. 递归公式

