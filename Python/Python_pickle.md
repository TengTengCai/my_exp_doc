[TOC]

# Python常见数据对象的序列化

​	序列化 (Serialization)将对象的状态信息转换为可以存储或传输的形式的过程。在序列化期间，对象将其当前状态写入到临时或持久性存储区。以后，可以通过从存储区中读取或反序列化对象的状态，重新创建该对象。 

## 一、Python中序列化对象的方式

在Python中对对象进行序列化有三种方式，pickle/shelve/JSON

**pickle**模块为Python对象结构的序列化和反序列化实现了一个基本但强大的算法。 “Pickling”是将Python对象层次结构转换为字节流的过程，“unpickling”是相反的操作，即字节流转换回对象层次结构。 

**shelve**是一个持久的，类似字典的对象。与“dbm”数据库的不同之处在于，shelve上的值（不是键！）本质上可以是任意Python对象 -  pickle模块可以处理的任何东西。这包括大多数类实例，递归数据类型和包含大量共享子对象的对象。键是普通的字符串。 

**JSON**(JavaScript Object Notation) 是一种轻量级的数据交换格式。 易于人阅读和编写。同时也易于机器解析和生成。在原生Python中也有相应的库json。

## 二、使用pickle进行序列化

pickle进行序列化，能对Python中所有的对象进行序列化，但是一次只能序列化一个对象。

```python
import pickle


class Person(object):
    def __init__(self, name, age, sex):
        self._name = name
        self._age = age
        self._sex = sex

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

    @property
    def sex(self):
        return self._sex

    @sex.setter
    def sex(self, sex):
        self._sex = sex


p1 = Person('TTC', 22, '男')  # person对象
my_str = 'TTC is a good boy.'  # 字符串对象
my_list = ['item1', 'item2', 'item3', 'item4', 'item5']  # 列表对象
my_tuple = ('tuple1', 'tuple2', 'tuple3')  # 元组对象
my_set = set((f'set{i}' for i in range(5)))  # 集合对象
my_dict = {'name': 'TTC', 'age': 22, 'sex': '男'}  # 字典对象


def main():
    # 通过文件的方式进行序列化和反序列化
    with open('example.pkl', 'wb') as f:
        pickle.dump(p1, f)  # 进行序列化
        # pickle.dump(my_str, f)
        # pickle.dump(my_list, f)
        # pickle.dump(my_tuple, f)
        # pickle.dump(my_set, f)
        # pickle.dump(my_dict, f)

    with open('example.pkl', 'rb') as f:
        temp = pickle.load(f)  # 进行反序列化
        print(temp)

    # 通过字符（二进制）进行序列化和反序列化
    pickle_p1 = pickle.dumps(p1)
    pickle_str = pickle.dumps(my_str)
    pickle_list = pickle.dumps(my_list)
    pickle_tuple = pickle.dumps(my_tuple)
    pickle_set = pickle.dumps(my_set)
    pickle_dict = pickle.dumps(my_dict)

    new_p1 = pickle.loads(pickle_p1)
    new_str = pickle.loads(pickle_str)
    new_list = pickle.loads(pickle_list)
    new_tuple = pickle.loads(pickle_tuple)
    new_set = pickle.loads(pickle_set)
    new_dict = pickle.loads(pickle_dict)
    print(new_p1, new_str, new_list, new_tuple, new_set, new_dict)


if __name__ == '__main__':
    main()

```

运行结果：

```bash
[tianjun@192 test]$ python3 example03.py 
<__main__.Person object at 0x7f09b08e2be0>
<__main__.Person object at 0x7f09b08e29e8> TTC is a good boy. ['item1', 'item2', 'item3', 'item4', 'item5'] ('tuple1', 'tuple2', 'tuple3') {'set4', 'set0', 'set2', 'set3', 'set1'} {'name': 'TTC', 'age': 22, 'sex': '男'}
```



## 三、使用shelve进行序列化

shelve能对Python中的所有对象进行序列化，而且能同时序列化多个对象，但是序列化的数据只能存储在文件中。

重写下main方法

```python
def main():

    # 将对象序列化到文件中
    with shelve.open('data') as db:
        db['p1'] = p1
        db['my_str'] = my_str
        db['my_list'] = my_list
        db['my_tuple'] = my_tuple
        db['my_set'] = my_set
        db['my_dict'] = my_dict

    # 将对象从序列化文件中获取持久化后的新对象
    with shelve.open('data') as db:
        new_p1 = db['p1']
        new_str = db['my_str']
        new_list = db['my_list']
        new_tuple = db['my_tuple']
        new_set = db['my_set']
        new_dict = db['my_dict']
        print(new_p1, new_str, new_list, new_tuple, new_set, new_dict)

```

运行结果：

```bash
[tianjun@192 test]$ python3 example04.py 
<__main__.Person object at 0x7f6b1fef5550> TTC is a good boy. ['item1', 'item2', 'item3', 'item4', 'item5'] ('tuple1', 'tuple2', 'tuple3') {'set4', 'set0', 'set3', 'set1', 'set2'} {'name': 'TTC', 'age': 22, 'sex': '男'}
```

## 四、使用json进行序列化

json只能对基本的数据类型进行序列化，序列化后的数据能够进行跨平台解析。

```python
def main():
    # 序列化到文件中
    with open('data.json', 'w') as f:
        # json.dump(p1, f)  Person对象不能进行json的序列化
        json.dump(my_str, f)  # 将对象序列化到文件中

    # 从文件中反序列化
    with open('data.json', 'r') as f:
        print(json.load(f))  # 从文件中将对象实例化出来

    # 将对象序列化为字符串
    # json_p1 = json.dumps(p1)  Person对象不能进行json的序列化
    json_str = json.dumps(my_str)
    json_list = json.dumps(my_list)
    json_tuple = json.dumps(my_tuple)
    # json_set = json.dumps(my_set)  set 集合对象无法序列化为json字符串
    json_dict = json.dumps(my_dict)

    # 将字符串反序列化为对象
    # new_p1 = json.loads(json_p1)
    new_str = json.loads(json_str)
    new_list = json.loads(json_list)
    new_tuple = json.loads(json_tuple)
    # new_set = json.loads(json_set)
    new_dict = json.loads(json_dict)
    print(new_str, new_list, new_tuple, new_dict)

```

运行结果：

```python
[tianjun@192 test]$ python3 example05.py 
TTC is a good boy.
TTC is a good boy. ['item1', 'item2', 'item3', 'item4', 'item5'] ['tuple1', 'tuple2', 'tuple3'] {'name': 'TTC', 'age': 22, 'sex': '男'}
```

## 五、总结

1、JSON只能处理基本数据类型。pickle和shelve能处理所有Python的数据类型。

2、JSON用于各种语言之间的字符转换。pickle和shelve用于Python程序对象的持久化或者Python程序间对象网络传输，但不同版本的Python序列化可能还有差异。