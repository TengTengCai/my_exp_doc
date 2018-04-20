
# Python基础（一）
[TOC]

## 1. python之禅


```python
import this
```

    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    

The Zen of Python, by Tim Peters	<br />		
<br />
Beautiful is better than ugly.			//漂亮比丑陋更好<br />
Explicit is better than implicit.		//明确比含蓄更好<br />
Simple is better than complex.			//简单比复杂更好<br />
Complex is better than complicated.		//复杂比混乱更好<br />
Flat is better than nested.			//扁平比嵌套更好<br />
Sparse is better than dense.			//稀疏胜于繁密<br />
Readability counts.				//可读性很重要<br />
Special cases aren't special enough to break the rules.
Although practicality beats purity.				//即便假借特例的实用性之名，也不可违背这些规则<br />
Errors should never pass silently.				//不要忽略错误<br />
Unless explicitly silenced.					//除非错误本身需要以忽略对待<br />
In the face of ambiguity, refuse the temptation to guess.	//不确定面前，我们应抵挡妄加猜测的引诱<br />
There should be one-- and preferably only one --obvious way to do it.	//应该有一种，也但愿只有这一种是显而易见的解决之道<br />
Although that way may not be obvious at first unless you're Dutch.	//万事开头难，除非荷兰人<br />
Now is better than never.						<br />
Although never is often better than *right* now.			//做好不过不做，而不假思索就动手还不如不做<br />
If the implementation is hard to explain, it's a bad idea.		//如果某个实现无法很好阐释，那么它肯定是一个糟糕的办法<br />
If the implementation is easy to explain, it may be a good idea.	//如果某个实现很容易说清楚，那么它可能是个不错的方案<br />
Namespaces are one honking great idea -- let's do more of those!	//命名空间是个绝妙的发明 -- 对此我们应多多益善<br />

## 2. 代码注解
`"""`三个双引号为块注释，一般在代码的开始，表明该文件的作用和介绍，一些编写人名称，时间和版本自定义的方法中进行，解释方法作用、传递的参数值和返回值
`#`号表示单行注释<br\>

```python
"""

第一个Python程序 - hello，world

Author:藤藤菜
Version:1.0
Date:2018年2月27日

"""
def hello(strings):
    """
    打印对应的字符串
    :param strings:需要打印的字符串
    :return:null返回为空
    """
    print(strings)
print('hello，world')
# 通过print函数输出一个字符串到屏幕上
print('你好世界')

```

## 3. 变量命名规则
### 硬性规则：
1. 变量名由字母/数字、下划线构成，数字不能开头
2. 大小写敏感
3. 不要关键字与保留字冲突

### 官方建议：
- 用全小写字母，多个单词之间用下划线连接见名知意
```python
print = 1000
# 改变了保留字的地址，print()函数将无法正常运行
print(1200)
```
## 4. 输入函数和数据类型
```python
# input输入 int数据类型转换
x = int(input('x = '))
y = int(input('y = '))
print('%d + %d = %d' % (x, y, x+y))
print('%d - %d = %d' % (x, y, x-y))
print('%d * %d = %d' % (x, y, x*y))
print('%d / %d = %.2f' % (x, y, x/y))
# 整除法
print('%d // %d = %d' % (x, y, x//y))
# 取余
print('%d %% %d = %d' % (x, y, x%y))
# 求幂运算
print('%d ** %d = %d' % (x, y, x**y))
```
## 5.运算符
1. 赋值运算符：`= += -= *= /= //= **=`
2. 算术运算符：`+ - * / // % **`
3. 关系运算符：`> >= < <= == !=`
4. 逻辑运算符：`and or not`
```python
x = 3
y = 5
x += y
print(x) # x = x + y
x *= y + 2
print(x) # x = x * y
x /= y
print(x) # x = x / y
x **= y
print(x) # x = x ** y
```
## 小练习
### 输入圆的半径计算圆的面积
```python
import math
# Python在语言层面没有定义常量的语法
# 当时我们可以通过把变量名用全大写来做一个隐含提示（隐喻）
# 全大写的变量要当做长量来看待在代码中不能修改它的值
# 经验：符号常量总是优于字面常量
r = float(input('请输入圆的半径是多少'))
print(type(r))
s = r ** 2 * math.pi
c = 2 * math.pi * r
print('您输入的圆的半径所计算的周长为：%.2f' % (c))  # 保留后两位小数
print('您输入的圆的半径所计算的面积为：%f' % (s))
print('圆的半径为：',r)
```
### 判断闰年
```python
year = int(input('请输入年份：'))
# condition1 = year % 4 == 0
# condition2 = year % 100 != 0
# condition3 = year % 400 == 0
# is_leap = (condition1 and condition2) or condition3
is_leap = year % 4 == 0 and year % 100 != 0 or year % 400 == 0
print(is_leap)
```
### 摄氏度和华氏度互相转换
```python
temperature = str(input('请输入需要转换的温度。（在数值后面添加，C为摄氏度，F为华氏度）'))
value = float(temperature[0:len(temperature)-1])
if 'C' in temperature:
    print('输入的是摄氏度,转换为华氏度的结果为:%.2f °F' % (32 + value * 1.8))
elif 'F' in temperature:
    print('输入的是华氏度,转换为摄氏度的结果为:%.2f °C' % ((value-32) / 1.8))
else:
    print('输入有误!')
```
### 输入三条边的长度能构成三角形，求三角形的周长面积是多少
```python
import math
arr = [0,0,0]
for i in range(3):
    arr[i] = float(input('请输入第%d条边' % (i+1)))
arr.sort()
print(arr)
if (arr[0] + arr[1]) > arr[2]:
    half = (arr[0] + arr[1] + arr[2])/2;
    s = math.sqrt(half * (half - arr[0]) * (half - arr[1]) * (half - arr[2]))
    print('面积为%f' % (s))
else:
    print('输入的数据不符合要求')
```
## 6.循环结构
```for```循环和 ```while```循环
一般情况下，有限次数的循环使用```for```<br\>
无限次循环或未知循环多少次使用```while```<br\>
### 猜数字小游戏
```python
from random import randint
print('欢迎来到猜数字小游戏！')
value = randint(0,100)
num = 1
game_over = False
while not game_over:
    if num > 7:
        print('真笨%d次了，还猜不中~' % num)
    thy_answer = int(input('请输入你猜的数字'))
    if thy_answer == value:
        print('恭喜你猜对了，随机数为：%d，你的答案为：%d' % (value,thy_answer))
        game_over = True
    elif thy_answer > value:
        print('猜的数字大了，再猜~')
    else:
        print('猜的数字小了，再猜~')
    
    num += 1
```
### 让电脑来陪你猜数字（反转角色）



```python
from random import randint
true_value = int(input('请您输入一个1-100的整数我来猜'))
com_answer = 100 // 2;
game_over = False
left = 0
right = 100
while not game_over:
    print('我猜的数字为：%d' % com_answer)
    if com_answer == true_value:
        print('你让我猜的数字是%d，我猜的是%d' % (true_value, com_answer))
        game_over = True
    else:
        my_answer = str(input('告诉我大了还是小了：'))
        if my_answer == "大":
            right = com_answer
            com_answer = (left + com_answer) // 2
        elif my_answer == "小":
            left = com_answer
            com_answer = (right + com_answer) // 2
```

    请您输入一个1-100的整数我来猜68
    我猜的数字为：50
    告诉我大了还是小了：小
    我猜的数字为：75
    告诉我大了还是小了：大
    我猜的数字为：62
    告诉我大了还是小了：小
    我猜的数字为：68
    你让我猜的数字是68，我猜的是68
    

### 人机猜拳(加赌注，谁先赌完就输)


```python
from random import randint
COMPUTER_WIN = 0 # 0为电脑获胜
PEOPLE_WIN = 1 # 1为人类获胜
com_money = 1000
my_money = 1000

def settlement(bet1, bet2, state):
    """
    该方法是计算金钱的剩余
    :param bet1:玩家赌注
    :param bet1:电脑赌注
    :param state:判断是否胜出
    :return:null
    """
    global my_money
    global com_money
    if state == 1:
        my_money += bet2
        com_money -= bet2
        print('你赢了')
    elif state == 0:
        com_money += bet1
        my_money -= bet1
        print('电脑赢了')
    print('电脑余额：%d  你的余额：%d \n' % (com_money,my_money))

print('你敢挑战我吗? 石头：0   剪刀：1   布：2')
game_over = False
while not game_over:
    if com_money <= 0:
        print('你获胜了')
        game_over = True
    elif my_money <= 0:
        print('电脑获胜了')
        game_over = True
    else:
        my_input = str(input('你出：'))
        if '石头' in my_input:
            my_device = 0
        elif '剪刀' in my_input:
            my_device = 1
        elif '布' in my_input:
            my_device = 2
        else:
            print('错误的输入')
            break
        my_bet = int(input('赌资：'))
        com_device = randint(0,2)
        if com_device == 0:
            print('电脑出: 石头')
        elif com_device == 1:
            print('电脑出: 剪刀')
        elif com_device == 2:
            print('电脑出: 布')
        com_bet = randint(10,151)
        print('电脑本轮的赌资为：%d' % com_bet)
        if my_device == 0 and com_device == 1:
            settlement(my_bet, com_bet, 1)
        elif my_device == 1 and com_device == 2:
            settlement(my_bet, com_bet, 1)
        elif my_device == 2 and com_device == 0:
            settlement(my_bet, com_bet, 1)
        elif my_device == 1 and com_device == 0:
            settlement(my_bet, com_bet, 0)
        elif my_device == 2 and com_device == 1:
            settlement(my_bet, com_bet, 0)
        elif my_device == 0 and com_device == 2:
            settlement(my_bet, com_bet, 0)
        else:
            print('本局打平~\n')

```

    你敢挑战我吗? 石头：0   剪刀：1   布：2
    你出：石头
    赌资：200
    电脑出: 剪刀
    电脑本轮的赌资为：61
    你赢了
    电脑余额：939  你的余额：1061 
    
    你出：石头
    赌资：200
    电脑出: 布
    电脑本轮的赌资为：15
    电脑赢了
    电脑余额：1139  你的余额：861 
    
    你出：900
    错误的输入
    

### 输入三个数，按照从小到大进行一个输出
```python
a = float(input('第一个数：'))
b = float(input('第二个数：'))
c = float(input('第三个数：'))

# 传统方法
# temp = 0
# if a > b:
#   temp = a
#   a = b
#   b = temp
# if b > c:
#   temp = b
#   b = c
#   c = temp
# if a > b:
#   temp = a
#   a = b
#   b = temp
    
# python方法，元组
# if a > b:
#     a, b = b, a
# if b > c:
#     b, c = c, b
# if a > b:
#     a, b = b, a

# python方法，元组和三元运算符
(a, b) = a > b and (b,a) or (a,b)
(b, c) = b > c and (c,b) or (b,c)
(a, b) = a > b and (b,a) or (a,b)

print(a,b,c)

```


```python
"""
完成以下输出
*
**
***
****
*****
1
12
123
1234
12345
A
AB
ABC
ABCD
ABCDE
    *
   **
  ***
 ****
*****
     *
    ***
   *****
  *******
 *********
A
BB
CCC
DDDD
EEEEE
"""
value = int(input('你要打印多少行： '))
for i in range(1,value + 1):
    for j in range(1,i + 1):
        print('*',end='')
    print()

for i in range(1,value + 1):
    for j in range(1,i + 1):
        print(j,end='')
    print()

for i in range(1,value + 1):
    for j in range(1,i + 1):
        print(chr(64 + j),end='')
    print()
    
for i in range(1,value + 1):
#     print(' ' * (value+1 - i),end='')
    for k in range(value + 1 - i, 1, -1):
        print(' ',end='')
    for j in range(1,i + 1):
        print('*',end='')
    print()
    
for i in range(0,value):
    for k in range(value + 1 - i, 1, -1):
        print(' ',end='')
    for j in range(0,2*i + 1):
        print('*',end='')
    print()
    
for i in range(1,value + 1):
    for j in range(1,i + 1):
        print(chr(64 + i),end='')
    print()   
```

### 猴子吃桃问题
- 有一群猴子，去摘了一堆桃子，商量之后决定每天吃剩余桃子的一半，当每天大家吃完桃子之后，有个贪心的小猴都会偷偷再吃一个桃子，按照这样的方式猴子们每天都快乐的吃着桃子，直到第十天，当大家再想吃桃子时，发现只剩下一个桃子了。问：猴子们一共摘了多少桃子


```python
my_sum = 1
for _ in range(9):
    my_sum = (my_sum + 1) * 2
print(my_sum)
```

    1534
    

### 五人捕鱼问题
- 有ABCDE五人夜间到河边捕鱼,捕完鱼后五人在河边睡着.天亮后A先醒来,将所捕鱼平均分伟五份,结果余一条,将余的一条扔掉,带走自己的一堆.B醒来将余下的四堆又分为五份,也余一条,同样仍掉,也带走自己的一堆.C、D、E醒来后也如此,问他们这天晚上至少捕到多少条鱼?


```python
is_over = False
num = 0
while not is_over:
    temp = num
    for i in range(5):
#         print(i)
        if temp % 5 == 1:
            temp = (temp - 1) * 0.8
            if i == 4:
                print('最少捕了%d条鱼' % num)
                is_over = True
    num += 1
    
```

    最少捕了3121条鱼
    


```python
'''
百钱百鸡，公鸡5快，母鸡3块，小鸡1块钱三只，问各有多少只
x+y+z==100
x*5+y*3+z//3 = 100
z%3 == 0
穷举法
'''
for x in range(21):
    for y in range(34):
        for z in range(0,100,3):
            if x + y + z == 100 and 5 * x + 3 * y + z // 3 == 100:
                print(x,y,z)

# 算法优化之后,循环次数减少
print()
for x in range(21):
    for y in range(34):
        z = 100 - x - y
        if 5 * x + 3 * y + z // 3 == 100 and z % 3 == 0:
                print(x,y,z)
```

    0 25 75
    4 18 78
    8 11 81
    12 4 84
    
    0 25 75
    4 18 78
    8 11 81
    12 4 84
    

### 水仙花数100~999 
```153 = 1 ** 3 + 5 ** 3 + 3 ** 3```


```python
for i in range(100,1000):
    str_num = str(i)
    num0 = int(str_num[0])
    num1 = int(str_num[1])
    num2 = int(str_num[2])
    my_sum = (num0 ** 3) + (num1 ** 3) + (num2 ** 3)
#     print(num0,num1,num2,my_sum)
    if my_sum == i:
        print('水仙花数：%d' % i)
```

    水仙花数：153
    水仙花数：370
    水仙花数：371
    水仙花数：407
    

### 找出10000以内的完美数
```
6 = 1 + 2 + 3
28 = 1 + 2 + 4 + 7 + 14
```


```python
from math import sqrt
from time import time
start = time()
for num in range(2,10000):
    my_sum = 1
    for i in range(2, int(sqrt(num)+1)):
        if num % i == 0:
            my_sum += i
            if i != num // i:
                my_sum += num // i
    if my_sum == num:
        print('完美数为%d' % num)
end = time()
print(end-start)
        
```

    完美数为6
    完美数为28
    完美数为496
    完美数为8128
    0.13359308242797852
    

### Craps 赌博游戏
- 规则：玩家掷两个骰子，每个骰子点数为1-6，如果第一次点数和为7或11，则玩家胜；如果点数和为2、3或12，则玩家输庄家胜。若和为其他点数，则记录第一次的点数和，玩家继续掷骰子，直至点数和等于第一次掷出的点数和则玩家胜；若掷出的点数和为7则庄家胜。


```python
from random import randint

"""
Craps 赌博游戏

Author：藤藤菜
Date：2018-03-02
version: 0.1

"""
PLAYER_WIN = 1  # 玩家获胜
COM_WIN = 0  # 电脑获胜
com_money = 1000  # 电脑初始金额
player_money = 1000  # 玩家初始金额


def calculation(debt, state):
    """
    该方法计算玩家和电脑的余额
    :param debt:赌注
    :param state:判断是否胜出
    :return:null
    """
    global com_money
    global player_money
    if state == PLAYER_WIN:
        com_money -= debt
        player_money += debt
    elif state == COM_WIN:
        com_money += debt
        player_money -= debt
    print('电脑余额: %d  玩家余额: %d' % (com_money, player_money), end='\n\n')


def recharge(charge_money):
    """
    对用户进行充值
    :param charge_money: 充值金额
    :return:null
    """
    global player_money
    player_money += charge_money


print('欢迎加入Craps游戏！目前你的余额为：%d  电脑的余额为：%d' % (player_money, com_money), end='\n\n')
game_over = False
while not game_over:
    while True:
        player_debt = int(input('玩家押注：'))
        if player_debt > player_money:
            print('押注失败，余额不足')
        else:
            break
    com_debt = randint(1, com_money)
    print('电脑押注：%d' % com_debt)
    face1 = randint(1, 6)
    face2 = randint(1, 6)
    my_point = face1 + face2
    print('第一次的点数为%d' % my_point)
    if my_point == 7 or my_point == 11:
        print('玩家胜')
        calculation(com_debt, PLAYER_WIN)
    elif my_point == 2 or my_point == 3 or my_point == 12:
        print('电脑胜')
        calculation(player_debt, COM_WIN)
    else:
        go_on = True
        while go_on:
            face1 = randint(1, 6)
            face2 = randint(1, 6)
            current_point = face1 + face2
            print('游戏继续的点数为%d' % current_point)
            if current_point == 7:
                print('电脑胜')
                calculation(player_debt, COM_WIN)
                go_on = False
            elif current_point == my_point:
                print('玩家胜')
                calculation(com_debt, PLAYER_WIN)
                go_on = False
    if com_money <= 0:
        print('最终==玩家==获胜')
        game_over = True
    elif player_money <= 0:
        is_recharge = input('哈哈哈~你已经没有余额了是否需要充值???（Y/N）')
        if is_recharge == 'Y' or is_recharge == 'y':
            money = int(input('输入你的充值金额：'))
            recharge(money)
        else:
            print('最终==电脑==获胜')
            game_over = True

```
