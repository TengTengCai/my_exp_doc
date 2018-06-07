[TOC]

# Python生成器

我之前是一个做Android的Java程序员，当我一年前开始学习Python时，for循环的差异吓了我一跳。这是因为在java中的语法是这样的：

```java
for(i=0; i < N; i++){
    do_something(i);
}
```

而在Python中我遇到了这两个函数一个叫`range`和一个叫`xrange`的新函数：

> **注意** 在Python2的版本中存在`range`和`xrange`,而在Python3中已经将`xrange`的实现形式替换了`range`所以在Python3中只存在`range`，而且是以生成器形式实现的。

```python
for i in range(N):
    do_something(i)
```

最开始我只会使用`range`，但是随着学习的深入我对`xrange`这个函数有了新的认识。下面就详细的介绍什么是生成器。

## 一、生成器的实现

在实现生成器之前，先尝试着简单的实现Python2中的`range`和`xrange`。这会让我们更加清晰列表和生成器的区别。

`range()`

```python
def range(start, stop, step=1):
    numbers = []
    while start < stop:
        numbers.append(start)
        start += step
    return numbers

for i in range(1, 10000):
    pass
```

`xrange()`

```python
def xrange(start, stop, step=1):
    while start < stop:
        yield start
        start += step

for i in xrange(1, 10000):
    pass
```

通过`yield`这个关键字我们很容易的实现了一个生成器。`yield`的中文意思就是生产，因此`xrange()`这个函数会生产很多值而不是只返回一个。这就让一个看上去很普通的函数转变成一个生成器，一种可以被重复轮询下一个可用值的函数。

## 二、列表和生成器的区别

在前面的实现中，要注意的是`range`的实现必须预先创建一个列表保存范围内的所有数字。所以范围从1到10000，这个函数会对numbers列表进行10000次的append,这会产生非常多的额外开销（这点在前面一篇[列表和元组的详细区别](./Python_ListAndTuple.md)中有讲)，然后返回整个列表。

而另一方面，生成器则能够“返回”很多值。每次代码运行到yield，该函数都会发射出一个值，然后当外不代码需要一个值时，该函数才会继续运行（之前的状态保持不变）并发射一个新的值。当函数结束运行时，一个`StopIteration`异常（for循环会自动的处理这个异常）会抛出表明该生成器已经没有更多的值了。

结果就是虽然两个函数最终运行了同样的计算次数，使用range版本的循环多消耗了10000倍的内存。（如果范围从1到N，则是N倍）

## 三、列表表达式和生成器表达式

Python为了能够更加容易的生成列表和生成器，有列表表达式和生成器表达式。

列表表达式公式`[<value> for <item> in <sequence> if <condition>]`

生成器表达式公式`(<value> for <item> in <sequence> if <condition>)`

其实他们之间就只有`[]`和`()`的区别

下面我们来举一个例子

如，我有一个数字长列表，我想知道其中有多少个数字可以被3整除。

列表表达式

```python
divisible_by_three = len([n for n in list_of_numbers if n % 3 == 0])
```

生成表达式

由于生成器没有一个length属性，所以我们可以通过生成器每遇到一个能被3整除的数字时就发射一个1，sum来求和，就能达到和列表表达式一样的结果。

```python
divisibale_by_three = sum((1 for n in list_of_numbers if n % 3 == 0))
```



