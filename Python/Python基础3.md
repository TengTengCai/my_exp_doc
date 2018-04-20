# Python基础(三) - 面向对象思想

[TOC]

## 一、面向对象概念

### 1.  对象

​	对象是人们要机械能研究的事物从最简单的整数到复杂的飞机等均可以看作是对象，它不仅能表示具体的事物，还能表示抽象的规则、计划和事件

### 2. 对象的状态和行为

​	对象具有状态，一个对象用数据值来描述它的状态。

​	对象还有操作，用于改变对象的状态，对象及其操作就是对象的行为。

​	对象实现了数据和操作的结合，使数据和操作封装于对象的统一体中

### 3. 类

​	具有相同特征（数据元素）和行为（功能）的对象的抽象就是类，因此，对象的抽象是类，类的具体化就是对象，也可以说类的实例是对象，类实际上就是一种数据类型。

​	类具有属性，它是对象的状态的抽象，用数据结构来描述类的属性。

​	类具有的操作，它是对象的行为的抽象，用操作名和实现改操作的方法来描述。

### 4. 类的结构

​	在客观世界中有若干类，这些类之间有一定的结构关系。通常有两种主要的结构关系，即一般--具体结构关系，整体--部分结构关系。

 1.  一般--具体结构称为分类结构，也可以说是‘或’关系，或者是‘is a’关系。

 2.  整体--部分结构称为组装结构，它们之间的关系是一种‘与’关系，或者是‘has a’关系。

### 5.消息和方法

​	对象之间进行通信的结构叫做消息。在对象的操作中，当一个消息发送给某一个对象时，消息包含接收对象去执行某种操作的消息。发送一条消息至少要包括说明接受消息的对象名、发送给改对象的消息名（即对象名、方法名）。一般还要对参数加以说明，参数可以是认识该消息的对象所知道的变量名，或者是所有对象都知道的全局变量名。

​	类中的实现过程叫做方法，一个方法有方法名、返回值、函数、方法体。

## 二、面向对象三大基本特征

面向对象有三大特征：封装、继承、多态

### 封装

封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只可让可信的类或者对象操作，对不可信的进行信息隐藏。封装是面向对象的特征之一，是对象和类概念的主要特征。简单是说，一个类就是一个封装了数据一级操作这些数据的代码的逻辑实体。在一个对象内部，某些代码或某些数据可以是私有的，不能被外界访问。通过这种方式，对象对内部数据提供了不同级别的保护，以防止程序中无关的部分意外的改变或错误的使用了对象的私有部分。

### 继承

继承， 是指可以让某个类型的对象获得另一个类型的对象的属性的方法。它支持按级分类的概念。基础是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对象这些功能进行扩展。通过基础创建的新类成为“子类”或“派生类”， 被继承的类称为“父类”或“超类”。继承的过程，就是从一般到特殊的过程。

### 多态

多态，是指一个类实例的相同方法在不同情形有不同表现形式。多态机制使具有不同内部结构的对象可以共享相同的外部接口，这意味着，虽然针对不同对象的具体操作不同，单通过一个公共的类，他们可以通过相同的方式予以调用。

## 三、面向对象七大基本原则

### 单一职责原则(SRP,Single Responsibility Principle)

​	一个类，只有一个引起它变化的原因。应该只有一个职责。每一个职责都是变化的一个轴线，如果一个类有一个以上的职责，这些职责就耦合在了一起。这会导致脆弱的设计。当一个职责反生变化时，可能会影响其它的职责。另外，多个职责耦合在一起，会影响复用性。

### 开发封闭原则(OCP, Open-Close Principle)

核心思想：软件实体应该是可拓展，而不可修改的。也就是说，对扩展是开发的，而对修改是封闭的。

所以开放封闭原则主要体现在两个方面：

1. 对扩展开放，意味着有新的需求或变化时，可以对现有代码进行扩展，以适应新的情况。
2. 对修改封闭，意味着类一旦设计完成，就可以独立完成其工作，而不要对类进行任何修改。

### 依赖倒转原则(DIP, Dependence Inversion Principle)

1. 高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。

2. 抽象不应该依赖于具体实现，具体实现应该依赖于抽象。

程序要依赖于抽象接口，不要依赖于具体实现。简单的说就是要求对抽象进行编程，不要对实现进行编程，这样就降低了客户与实现模块间的耦合。

### 里氏替换原则(LSP, Liskov Substitution Principle)

​	任何基类可以实现的地方，子类一定可以出现。LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受影响时，基类才能真正被复用，而衍生类也能够在基类的基本上增加新的行为。

​	如此，问题产生了：“我们该如何去度量继承关系的质量？”

​	Liskov于1987年提出了一个关于继承的原则“Inheritance should ensure that any property proved about supertype objects also holds for subtype objects.”——"继承必须确保超类所拥有的性质在子类中仍然成立。"也就是说，当一个子类的实例应该能够替换任何超类的实例时，他们之间才具有is-a关系。

​	简单的说就是“老鼠的儿子会打洞。”

### 接口隔离原则(ISP, Interface Segregation Principle)

客户端不应该依赖它不需要的接口，一个类对另一个类的依赖应该建立在最小的接口上。

​	使用多个专门的接口比使用单一的接口要号。

​	一个类对另外一个类的依赖性应当是建立在最小的接口上单

​	一个接口代表一个角色，不应当将不同的角色都交给一个接口。没有关系的接口合并在一起，形成一个臃肿的大接口，这是对教师和接口的污染。

​	“不应该强迫客户依赖于它们不用的方法。接口属于客户，不属于它所在的类层次结构。”通俗的说，不要强迫客户使用他们不用的方法如果强迫用户使用它们不使用的方法，那么这些客户就会面临由于这些不使用的方法的改变所带来的改变。

### 组合/聚合复用原则(CARP, Composite/Aggregate)

​	组合和聚合都是对象建模中关联关系的一种，表示整体与部分的关系，表示“含有”，整体由部分组合而成，部分可以脱离整体作为一个独立的个体存在。组合则是一种更强的聚合，部分组成整体，而且不可分割，部分不能脱离整体而单独存在。在合成关系中，部分和整体的生命周期一样，组合的新的对象完全支配其组成部分，包括他们的创建和销毁。一个合成关系中成分对象是不能与另外一个合成关系共享。

​	组合/聚合和继承是实现复用的两个基本途径。合成复用原则是指尽量使用合成/聚合，而不是使用继承。

### 迪米特法则(LKP, Least Knowledge Principle)

​	迪米特法则可以简单说成：talk only to your immediate friends。一个软件试题应当金肯呢个少的与其他实体发生相互作用。每个软件单位对其他的单位都只是最少的知识，而且局限于那写与本单位密切相关的软件单位。

​	迪米特法则的初衷在于降低类之间的耦合。由于每个类尽量减少对其他类的依赖，因此，很容易使得系统的功能模块功能独立，相互指尖不存在（或很少有）依赖关系。

​	迪米特法则不希望类之间建立直接的联系。如果真的需要建立练习，也希望能通过它的友员类来传达。因此，应用迪米特发展有可能造成的后果就是：系统中存在大量的中介类，这些类之所以存在完全是为了传递类指尖的相互调用关系——这在一定程度上增加了系统的复杂度。

## 四、面向对象的使用

### 定义类

在python中可以使用```class```关键字来定义类，然后通过函数的知识来定义方法，这样就可以将对象的动态特征描述出来。

```python
class Person(object):
    # __init__初始化构造方法，通过这个方法可以绑定name和age两个属性
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def eat(self, food_name):
        print('%s正在吃%s' % (self.name, food_name))
        
    def run(self):
        if self.age < 12:
            print('%s年纪太小了，跑不快' % self.name)
        elif self.age < 18:
            print('%s年纪小，跑得很快' % self.name)
        elif self.age < 30:
            print('%s年纪还好，跑得比较快' % self.name)
        else:
            print('%s太老了，跑不动了' % self.name)
        
```

### 创建和使用对象

当定义号一个类之后，可以创建一个对象来，给对象发消息。

```python
def main():
    # 创建人对象并指定姓名和年龄
    person1 = Person('小明'， 22)
    person2 = Person('小红'， 16)
    # 给对象发送吃东西的消息
    person1.eat('藤藤菜')
    # 给对象发送跑的消息
    person1.run()
    
    person2.eat('大白菜')
    person2.run()

if __name__  == '__main__':
    main()
```

### 访问可见性

在常见的面向对象编程语言中，方法和属性都存在访问权限。通常情况下，我们会将对象的属性设置为私有的（private）或受保护的（protected），简单的说就是不允许外界访问，二对象的方法通常都是公开的（public），因为公开的方法就是对象能够接受的消息，在Python中，属性和方法的访问权限只有两种，也就是公开和私有的，如果希望属性是私有的，就可以在属性命名时在前面添加一个‘_’下划线。

```python
class Person(object):
    def __init__(self, name, age)
        self._name = name
        self._age = age
    @property    
    def name(self):
        return self._name
    
    @name.setter
    def name(self, name)
    	self._name = name
    
class Student(Person):
    # 限定Student对象只能百年的_name,_age和_gender属性
    __slots__ = ('_name', '_age', '_gender')
    
    def __init__(self, name, age, gender, ):
        super().__init__(name, age)
        self._gender = gender
        
    def study(self):
        print('今年%d岁的%s，正在学习。。。' % (self._age, self._name))
    
    def play_conputer(self):
        print('今年%d岁的%s，正在玩电脑。。。' % (self._age, self._name))
        
def main():
    stu1 = Student('藤藤菜', 22, '男')
    stu1.study()
    print(stu1.name)  # 输出当前名字
    stu1.name = '这是一个新的名字'  # 赋予一个新的名字
    stu1.play_computer()

if __name__ == '__main__':
    main()
```

### 静态方法和类方法

当行为是属于类而不是属于对象的时候，可以采用静态方法和类方法。发送消息的时候直接类名加对应的方法名称。

```python
from math import sqrt


class Triangle(object):

    def __init__(self, a, b, c):
        self._a = a
        self._b = b
        self._c = c

    @staticmethod
    def is_valid(a, b, c):
        return a + b > c and b + c > a and a + c > b
    
    def perimeter(self):
        return self._a + self._b + self._c

    def area(self):
        half = self.perimeter() / 2
        return sqrt(half * (half - self._a) *
                    (half - self._b) * (half - self._c))


def main():
    a, b, c = 3, 4, 5
    # 静态方法和类方法都是通过给类发消息来调用的
    if Triangle.is_valid(a, b, c):
        t = Triangle(a, b, c)
        print(t.perimeter())
        # 也可以通过给类发消息来调用对象方法但是要传入接收消息的对象作为参数
        # print(Triangle.perimeter(t))
        print(t.area())
        # print(Triangle.area(t))
    else:
        print('无法构成三角形.')


if __name__ == '__main__':
    main()
```

通过类方法创建对象（初始化参数）并获取对象

```python
from time import time, localtime, sleep


class Clock(object):
    """数字时钟"""

    def __init__(self, hour=0, minute=0, second=0):
        self._hour = hour
        self._minute = minute
        self._second = second

    @classmethod
    def now(cls):
        ctime = localtime(time())
        return cls(ctime.tm_hour, ctime.tm_min, ctime.tm_sec)

    def run(self):
        """走字"""
        self._second += 1
        if self._second == 60:
            self._second = 0
            self._minute += 1
            if self._minute == 60:
                self._minute = 0
                self._hour += 1
                if self._hour == 24:
                    self._hour = 0

    def show(self):
        """显示时间"""
        return '%02d:%02d:%02d' % \
               (self._hour, self._minute, self._second)


def main():
    # 通过类方法创建对象并获取系统时间
    clock = Clock.now()
    while True:
        print(clock.show())
        sleep(1)
        clock.run()


if __name__ == '__main__':
    main()
```

###  继承和多态

```python
# 继承 - 从已经有的父类创建新的过程
# 提供基础信息的称为父类（超类/基类）
# 得到继承信息的称为子类（派生类/衍生类）
# 通过基础我们可以将子类中的重复的代码抽取到父类中
# 子类通过基础并复用这些代码来减少重复代码的编写
# 将来如果要维护子类的公共代码只需要在父类中进行操作即可


class Person(object):
    def __init__(self, name, age):
        self._name = name
        self._age = age

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        if 2 < len(name) < 4:
            self._name = name
        else:
            self._name = '无名氏'

    # 属性访问器
    @property
    def age(self):
        return self._age

    # 属性的修改器
    @age.setter
    def age(self, age):
        if 15 <= age <= 25:
            self._age = age
        else:
            raise ValueError('无效年龄')

    def watch_av(self):
        if self._age > 18:
            return '%s正在看动作电影' % self._name
        else:
            return '推荐看熊出没'


class Student(Person):

    def __init__(self, name, age, grade):
        super().__init__(name, age)
        self._grade = grade

    def study(self, course):
        return '%s%s正在学习%s' % (self._grade, self._name, course)

    # 方法的重写 （override） - 覆盖 / 置换 /覆写
    # 子类在继承父类方法后  对方法进行了重新实现
    # 当我们给子类对象发送watch_av消息时执行的是子类重写过的方法
    def watch_av(self):
        print('正在和成龙学习')

    def __str__(self):
        return ''


class Teacher(Person):
    def __init__(self, name, age, title):
        super().__init__(name, age)
        self._title = title

    def teach(self, course):
        return '%s正在讲%s' % (self._name, course)

    @property
    def title(self):
        return self._title

    @title.setter
    def title(self, title):
        self._title = title


def main():
    stu = Student('王大锤', 22, '初三')
    print(stu.study('语文'))
    print(stu.watch_av())
    teacher = Teacher('asdk',16, 'asd')
    print(teacher.teach('asdas'))
    print(teacher.watch_av())
    pass


if __name__ == '__main__':
    main()


```

子类在继承了父类的方法后，可以对父类已有的方法给出新的实现版本，这个动作称之为方法重写（override）。通过方法重写我们可以让父类的同一个行为在子类中拥有不同的实现版本，当我们调用这个经过子类重写的方法时，不同的子类对象会表现出不同的行为，这个就是多态（poly-morphism）。

```python
from abc import ABCMeta, abstractmethod


class Pet(object, metaclass=ABCMeta):
    """宠物"""

    def __init__(self, nickname):
        self._nickname = nickname

    @abstractmethod
    def make_voice(self):
        """发出声音"""
        pass


class Dog(Pet):
    """狗"""

    def make_voice(self):
        print('%s: 汪汪汪...' % self._nickname)


class Cat(Pet):
    """猫"""

    def make_voice(self):
        print('%s: 喵...喵...' % self._nickname)


def main():
    pets = [Dog('旺财'), Cat('凯蒂'), Dog('大黄')]
    for pet in pets:
        pet.make_voice()


if __name__ == '__main__':
    main()
```

[代码引用]：http://blog.csdn.net/jackfrued/article/details/79545466



在Python中是支持多重继承的，但是尽量要避免多重继承的出现

```python
class A(object):
    def __init__(self):
        pass

    def foo(self):
        print("A's foo")


class B(A):
    def __init__(self):
        super().__init__()


class C(A):
    def __init__(self):
        super().__init__()

    def foo(self):
        print("C's foo")


# 如果一个类有多个父类，而多个父类又有公共的父类（菱形继承、钻石继承）
# 在python3 中采用的是一种C3算法，实现的是广度优先算法
# 在python2 中采用的DFS算法，实现的是深度优先算法
class D(B, C):
    def __init__(self):
        B.__init__(self)
        C.__init__(self)


def main():
    d = D()
    d.foo()
    pass


if __name__ == '__main__':
    main()

```



## 贪吃蛇小游戏

编写了完整的代码注释，能很好的理解面向对象。


https://github.com/TengTengCai/Snake