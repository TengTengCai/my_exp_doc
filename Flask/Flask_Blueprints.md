# Flask 进阶蓝图（Blueprints）项目模块化



## 蓝图（Blueprints）

​	Flask使用蓝图的概念来制作应用程序组件，并支持应用程序或应用程序中的通用模式。蓝图可以大大简化大型应用程序的工作方式，并为Flask扩展程序在应用程序上注册操作提供了一个重要手段。Blueprint对象与Flask应用程序对象的工作方式类似，但实际上并不是应用程序，相反，它的构建或扩展应用程序的蓝图。

**为什么会使用蓝图呢**

Flask中的蓝图优势：

- 将应用程序分解为一组蓝图，这对于更大的应用是更好的。一个项目可以实例化一个应用程序对象，初始化多个扩展，并注册一组蓝图。
- 在URL前缀的应用程序上注册蓝图，URL前缀的参数将成为蓝图中所有视图函数的常见视图参数。
- 在具有不同URL规则的应用程序中多次注册蓝图。
- 通过蓝图提供模版过滤器，静态文件，模版和其他实用程序。蓝图不必实现应用程序或查看功能。
- 在初始化Flask扩展时，在应用程序中注册这些蓝图。

Flask中的蓝图不是可插拔的应用程序，因为它实际上并不是一个应用程序 - 它是一组可以在应用程序中注册的操作集，甚至可以多次执行。 

Blueprints仅在Flask级别提供分离，共享应用程序配置，并可根据需要在注册时更改应用程序对象。缺点是一旦创建应用程序就无法取消注册蓝图，而需要取消就只有销毁整个应用程序对象。

简单的说，蓝图记录了应用程序执行注册后的操作。Flask在调度请求时，将一个端点到另一个端点的URL视图函数与蓝图进行关联起来。



## 将项目模块化

首先我们先将项目的骨架搭建起来。

```bash
[tianjun@localhost Flask_TTC]$ tree
.
├── App    # App应用程序 Flask对象
│   ├── Admin  # 分块的Admin模块， 后台模块
│   │   ├── __init__.py  # Admin模块初始化文件，创建admin蓝图对象
│   │   ├── models.py  # 数据库模型文件
│   │   └── views.py  # 逻辑控制文件
│   ├── Index  # 分块的Index模块， 前台模块
│   │   ├── __init__py  # Index模块初始文件，创建index蓝图对象
│   │   ├── models.py  # 数据库模型文件
│   │   └── views.py  # 逻辑控制文件
│   └── __init__.py  # App模块初始化文件，主要对db和app创建的方法
├── config.py  # 项目配置文件
├── manager.py  # 项目启动文件
├── static  # 静态文件
├── templates  # 模板文件
├── test  # 测试文件放置位置
└── utils  # 工具文件
    ├── functions.py
    └── __init__.py

5 directories, 11 files
```

先对配置文件进编写

config.py

```python
import os

BASE_DIR = os.getcwd()  # 项目的绝对路径 

TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')  # 模板文件的路径

STATICFILES_DIR = os.path.join(BASE_DIR, 'static')  # 静态文件的路径

SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://root:123456@localhost:3306/ttc'  # 数据库URI

SQLALCHEMY_TRACK_MODIFICATIONS = False  # 查询跟踪，不太需要，False，不占用额外的内存

```

然后在App模块中的`__init__.py`文件中编写对Flask对象进行创建的方法，初始化SQLAlchemy对象

`App/__init__.py`

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

from config import SQLALCHEMY_DATABASE_URI， SQLALCHEMY_TRACK_MODIFICATIONS

db = SQLAlchemy()  # 初始化SQLAlchemy

def create_app():
    """ 创建app的方法 """
    app = Flask(__name__)  # 生成Flask对象
    app.config['SQLALCHEMY_DATABASE_URI'] = SQLALCHEMY_DATABASE_URI  # 配置app的URI
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = SQLALCHEMY_TRACK_MODIFICATIONS
    db.init_app(app=app)  # SQLAlchemy初始化App
    # 在这还可以设置好配置后， 初始化其他的模块
    
    return app  # 返回Flask对象app 

```

接着在Index模块中创建蓝图

`App/Index/__init__.py`

```python
from flask import Blueprint

from config import TEMPLATES_DIR, STATICFILES_DIR

index = Blueprint('index', __name__, 
                  template_folder=TEMPLATES_DIR,
                 static_folder=STATICFILES_DIR)  # 创建一个蓝图对象，设置别名，模板文件地址，静态文件地址

from App.Index import views  # 这里导入是为了在解释时，蓝图能加载到views文件中的路由数据

```

再接着在views.py中为蓝图设置路由

```python
from App.Index import index  # 获取蓝图

from App.Index.models import *  # 获取数据库模型对象和SQLAlchemy对象db，注意不可使用App模块中的db

@index.route('/')  # 设置路由
def index():  # 执行的方法
    return 'This Page Is Index'

```

（Admin模块蓝图的创建方法和上述过程相似不做过多描述）

最后在manager.py文件中对蓝图进行注册

```python
from flask_script import Manager

from App import create_app
from App.Index import index
from App.Admin import admin

app = create_app()  # 创建app
app.register_blueprint(index, url_prefix='/index')  # 注册蓝图
app.register_blueprint(admin, url_prefix='/admin')  # 注册蓝图

manager = Manager(app)  # 通过app创建manager对象

if __name__ == '__mian__':
    manager.run()  # 运行服务器

```

> **注意**
>
> 在这里一定要严格在遵守Python导入包的编写顺序
>
> 1. Python Build In 内构包
> 2. Python 第三方库
> 3. 用户自定义模块

好了配置为完成，运行服务器

```bash
(venv) [tianjun@localhost Flask_TTC]$ python manager.py runserver -h 127.0.0.1 -p 5000 -d
 * Serving Flask app "App" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 244-604-680
```

访问页面

![页面访问](./img/蓝图_浏览器_展示.png)

## 参考文档

[Flask Blueprints](http://flask.pocoo.org/docs/0.12/blueprints/#blueprints)