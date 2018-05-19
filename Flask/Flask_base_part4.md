# Flask 基础 Part4 Flask-SQLAlchemy 的使用

Flask-SQLAlchemy使用起来非常有趣，对于基本应用程序来说非常简单，并且适用于大型应用程序。

安装

```bash
pip install flask-sqlalchemy
```



## 配置

Flask-SQLAlchemy存在以下的配置值，Flask-SQLAlchemy从主Flask配置中加载这些值，可以通过各种方式进行填充。请注意，在创建引擎后其中一些内容无法修改，因此需要确保尽早的进配置，并且不要在运行时对其进行修改。

| 配置名称   						 | 				介绍					|
| -------------------------------- | ------------------------------------------------------------ |
| `SQLALCHEMY_DATABASE_URI`        | 将要被用于数据库链接的URI。 例如:   `mysql://username:password@server/db` `mysql+pymysql://root:123456@localhost:3306/TTC` |
| `SQLALCHEMY_BINDS`               | 一个将会绑定多种数据库的字典。  更多详细信息请看官文 [绑定多种数据库](http://flask-sqlalchemy.pocoo.org/2.3/binds/#binds). |
| `SQLALCHEMY_ECHO`                | 如果设置为True，SQLAlchemy会将记录所有标准错误声明，这对调试非常有用。 |
| `SQLALCHEMY_RECORD_QUERIES`      | 可以用于禁用或启用查询记录的显示 。 查询记录会自动的在调试或测试模式下进行 。 |
| `SQLALCHEMY_NATIVE_UNICODE`      | 可以用来启用或禁用本地对 unicode  的支持。 |
| `SQLALCHEMY_POOL_SIZE`           | 数据库池的大小。  默认与数据库引擎的值相同 (通常为 5) |
| `SQLALCHEMY_POOL_TIMEOUT`        | 指定池的连接超时（以秒为单位）。 |
| `SQLALCHEMY_POOL_RECYCLE`        | 自动循环连接的秒数。这是MySQL所必须的，默认情况下，闲置8小时后会删除连接。如果使用MySQL，SQLAlchemy会自动将其设置为2小时，一些后端可能使用不同的默认超时值。 |
| `SQLALCHEMY_MAX_OVERFLOW`        | 控制连接池达到最大大小后还可以创建的连接数，当这些附加连接返回到连接池时，它们将会被断开并丢弃。 |
| `SQLALCHEMY_TRACK_MODIFICATIONS` | 如果设置为True，Flask-SQLAlchemy将跟踪对对象的修改，并发出信号。默认值为None，他可以启用跟踪功能，但会发出警告，表明它在将来会被默认禁用。这需要额外的内存，如果不需要，应该禁用。 |

下面对SQLAlchemy进行配置(本文程序简单不会进行Blueprints分块)

app.py

```python
from flask import Flask
from flask_script import Manager
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)  # 创建一个Flask app对象
# 数据库链接的配置，此项必须，格式为（数据库+驱动://用户名:密码@数据库主机地址:端口/数据库名称）
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123456@localhost:3306/flask_ttc'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # 跟踪对象的修改，在本例中用不到调高运行效率，所以设置为False
db = SQLAlchemy(app=app)  # 为哪个Flask app对象创建SQLAlchemy对象，赋值为db
manager = Manager(app=app)  # 初始化manager模块

@app.route('/')
def hello_world():
    reutrn 'Hello World!'
    
if __name__ == '__main__':
    manager.run()  # 运行服务器
```

## 声明模型

这里的模型就的我们口中通常说的MTV中Model，显然app.py 中用@app.route()装饰器装饰的就是View了，Templates中存在的html模板文件就的我们的T了。

这里简单的写一个Model

models.py

```python
from app import db  # 导入app文件中的SQLAlchemy对象

class Student(db.Model):  # 继承SQLAlchemy.Model对象，一个对象代表了一张表
    s_id= db.Column(db.Integer, primary_key=True, autoincrement=True, unique=True)  # id 整型，主键，自增，唯一
    s_name = db.Column(db.String(20))  # 名字 字符串长度为20
    s_age = db.Column(db.Integer, default=20)  # 年龄 整型，默认为20
    
    __tablename__ = 'student'  # 该参数可选，不设置会默认的设置表名，如果设置会覆盖默认的表名
    def __init__(self, name, age):  # 初始化方法，可以对对象进行创建
        self.s_name = name
        self.s_age = age
    def __repr__(self):  # 输出方法，与__str__类似，但是能够重现它所代表的对象
        return '<Student %r, %r, %r>' % (self.s_id, self.s_name, self.sage)
```

列的类型是Column的第一个参数。可以直接指定他们，也可以的调用他们以进一步的指定他们的长度。以下类型是最常见的：

| 类型  | 介绍                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Integer | 一个整数                                                |
| String(size) | 一个字符串，并可以设置它的最大长度 |
| Text | 更长的unicode文本                        |
| DateTime | 通过Python的datetime对象来表示时间日期 |
| Float| 存储一个浮点数 |
| Boolean| 存储一个布尔值                                  |
| PickleType | 存储一个序列化（ Pickle ）后的Python对象 |
| LargeBinary | 存储巨大长度的二进制数据          |

我们可以通过交互式的环境来创建数据库中的表

```powershell
>>> from app import db  # 引入SQLAlchemy
>>> from models import *  # 映入加载了model的SQLAlchemy对象，db
>>> db.create_all()  # 创建表
```

这样数据库中就已经存在student表了

## 插入(Insert)删除(Delete)修改(Update)选择(Select)

这些操作都可以通过SQL语句来实现

```python
sql = 'select * from student;'
stus = db.session.execute(sql)
```

### 插入

```python
# 插入一条
stu = Student('TTC', 22)
db.session.add(stu)
db.session.commit()
# 如果添加的对象是列表，使用db.session.add_all(list)
stus = []
db.session.add_all(stus)
db.session.commit()
```

### 删除

```python
stu = Student.query.get(1)  # 需要删除的对象
db.session.delete(stu)  # 删除
db.session.commit()  # 提交事务
```

### 修改

```python
stu = Student.query.get(1)  # 需要修改的对象
stu.s_age = 18  # 修改值
db.session.commit()  # 提交
```

### 查询

过滤查询

```python
stu = Student.query.filter(s_name='TTC').first()
# 如果查询一个不存在的会返回一个None
```

用主键查询

```python
stu = Student.query.get(1)  # 主键查询
```

比较查询

```python
# __lt__ 小于  __le__小于等于   __gt__ 大于  __ge__ 大于等于
Student.query.filter(Student.s_age.__lt__(16))  # 小于16岁 
Student.query.filter(Student.s_age.__le__(16))  # 小于等于16岁
Student.query.filter(Student.s_age.__gt__(16))  # 大于16岁
Student.query.filter(Student.s_age.__ge__(16))  # 大于等于16岁
```

In 查询

```python
# 使用in_方法获取与列表中值相匹配的值
Student.query.filter(Student.s_age.in_([16, 17, 18, 19, 20]))
```

排序

```python
Student.query.order_by('s_age') # 按年龄排序，默认升序，在前面加-号为降序'-s_age'
```

限制条数和从第几条开始

```python
Student.query.filter(s_age=18).offset(2).limit(3)  # 跳过二条开始查询，限制输出3条
```

多条件查询

```python
Student.query.filter(Student.s_age == 18, Student.s_name == 'TTC')
# 也可以通过and_并且  or_或者  not_非 的方法来进行查询
Student.query.filter(and_(Student.s_age == 18, Student.s_name == 'TTC'))
Student.query.filter(or_(Student.s_age == 18, Student.s_name == 'TTC'))
Student.query.filter(not_(Student.s_age == 18, Student.s_name == 'TTC'))
```

## 模型关系

### 一对多(one-to-many)关系

最常见的关系就是一对多的关系。因为关系在它们建立之前就已经声明，可以使用字符串来指代还没有创建的类。

比如班级和学生的关系即为一对多的关系，一个班级对应多名学生。

models.py

```python
from app import db


class Grade(db.Model):
    """班级表"""
    g_id = db.Column(db.Integer, primary_key=True, autoincrement=True)  # 班级id
    g_name = db.Column(db.String(20))  # 班级名称
    g_desc = db.Column(db.String(100))  # 描述
    students = db.relationship('Student', backref='grade', lazy=True)  # 关系

    __tablename__ = 'grade'  # 表名

    def __init__(self, name, desc):  # 初始化方法
        self.g_name = name  # 名称
        self.g_desc = desc  # 描述

    def __repr__(self):  # 输出方法，显示对象内容
        return '<Grade %r, %r, %r>' % (self.g_id, self.g_name, self.g_desc)


class Student(db.Model):
    """学生表"""
    s_id = db.Column(db.Integer, primary_key=True, autoincrement=True)  # 学生ID
    s_name = db.Column(db.String(20))  # 学生姓名
    s_age = db.Column(db.Integer, default=20)  # 学生年龄
    g_id = db.Column(db.Integer, db.ForeignKey('grade.g_id'))  # 外键

    __tablename__ = 'student'  # 表名

    def __init__(self, name, age, g_id):  # 初始化方法
        self.s_name = name  # 姓名
        self.s_age = age  # 年龄
        self.g_id = g_id  # 班级ID

    def __repr__(self):  # 输出方法，显示对象内容
        return '<Student %r, %r, %r>' % (self.s_id, self.s_name, self.s_age)
```

通过学生获取班级

```python
g_id = Student.query.get(12).grade.g_id
```

通过班级获取学生

```python
stus = Grade.query.get(2).students
```

### 一对一（one-to-one）关系

需要使用一对一的关系，和一对多的关系的写法是一样的，只是在relationship中有一个参数不同，需要将`uselist=Flase`，这样两个表之间的关系就变成了一对一的关系。其参数的含义就是不使用列表，两个表之间只有一条对应一条的关联。

### 多对多（many-to-many）关系

如果想使用多对多关系，需要定义一一个用于关系的辅助表，对于这个辅助表，建议不使用模型，而是采用一个实际的表：

我们接着之前的写，加入学生(Student)和课程(Course)的关系

```python
from app import db  # 引入模块
class Student(db.Model):
    """学生表"""
    s_id = db.Column(db.Integer, primary_key=True, autoincrement=True)  # 学生ID
    s_name = db.Column(db.String(20))  # 学生姓名
    s_age = db.Column(db.Integer, default=20)  # 学生年龄
    g_id = db.Column(db.Integer, db.ForeignKey('grade.g_id'))  # 班级ID

    __tablename__ = 'student'  # 学生表

    def __init__(self, name, age, g_id):  # 初始化方法
        self.s_name = name  # 名称
        self.s_age = age  # 年龄
        self.g_id = g_id  # 班级ID

    def __repr__(self):  # 格式化输出显示对象中的值
        return '<Student %r, %r, %r>' % (self.s_id, self.s_name, self.s_age)

# 辅助表 记录学生表和课程表的多对多关系
sc = db.Table('sc',  # 表名
              db.Column('s_id', db.Integer, db.ForeignKey('student.s_id'), primary_key=True),
              db.Column('c_id', db.Integer, db.ForeignKey('course.c_id'), primary_key=True)
              )


class Course(db.Model):
    """课程表"""
    c_id = db.Column(db.Integer, primary_key=True, autoincrement=True)  # 课程的ID
    c_name = db.Column(db.String(20))  # 课程名称
    students = db.relationship('Student', secondary=sc, backref='courses')  # 与学生表的关系
	# secondary指定副表名称
    def __init__(self, name):  # 初始化方法
        self.c_name = name  # 课程名

    def __repr__(self):  # 格式化输出显示对象中的值
        return '<Course %r, %r >' % (self.c_id, self.c_name)
```

多对多关系的数据插入方式

```python
stu = Student.query.get(s_id)  # 获取学生对象
cou = Course.query.get(c_id)  # 获取课程对象
# 方式一
cou.students.append(stu)  # 建立学生和课程的关系
db.session.add(cou)  # 为会话添加事务
# 方式二
stu.courses.append(cou)
db.session.add(stu)
# 方式一和方式二等效

db.session.commit()  # 提交事务
```

多对多关系的数据删除方式

```python
stu = Student.query.get(s_id)
cou = Course.query.get(c_id)
# 方式一
cou.students.remove(stu)
db.session.add(stu)
# 方式二
stu.students.remove(cou)
db.session.add(stu)
# 方式一和方式二等效

db.session.commit()
```

多对多关系互相查询

通过学生查课程

```python
cous = Student.query.get(s_id).courses
```

通过课程查学生

```python
stus = Course.query.get(c_id).students
```





