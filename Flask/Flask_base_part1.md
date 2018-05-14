[TOC]

# Flask基础Part1

## 安装Flask模块

```bash
pip install flask
```

## 创建一个Flask项目

创建一个app.py文件

```python
from flask import Flask  # 导入Flask包

app = Flask(__name__)  # 获取Flask对象，以当前模块名为参数


# 路由默认为（127.0.0.1:5000）
@app.route('/')  # 装饰器对该方法进行路由设置，请求的地址
def hello_world():  # 方法名称
    return 'Hello World!'  # 返回响应的内容


if __name__ == '__main__':
    app.run()

```

## 运行Flask

通过python 执行app.py文件

```bash
python app.py
```

服务会默认的起在127.0.0.1:5000

```ba
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

## 运行参数

1. 在文件中修改参数进行变更debug，port， host

```python
app.run(debug=True, port='5000', host='127.0.0.1')
```

2. 使用sys模块，在控制台中输入参数进行参数变更

```python
if __name__ == '__main__':
    import sys
    argvs = sys.argv
    app.run(host=argvs[0], port=argvs[1], debug=argvs[2])
```

```bash
python app.py 127.0.0.1 5000 True
```

3. 使用flask-script包中的Manger模块

安装Flask-Script包

```bash
pip install flask-script
```

app.py文件

```python
from flask import Flask
from flask_script import Manager

app = Flask(__name__)

manager = Manager(app)


@app.route('/')
def hello_world():
    return 'Hello World'


if __name__ == '__main__':
    manager.run()
```

在控制台进行启动服务(-h 主机地址  -p端口号   -d 是否debug模式，加了为True，不加为False)

```bash
python app.py runserver -h 127.0.0.1 -p 5000 -d
```

## 在网页页面中进行控制台调试

当我们通过flask-script包进行项目启动后，在出现bug后可在网页上输入PIN码然后进行调试，这点非常的方便。

我们先在代码中手动添加一个除数为零的错误。

```python
from flask import Flask
from flask_script import Manager

app = Flask(__name__)

manager = Manager(app)

@app.route('/')
def hello_world():
    name = 'TengTengCai'
    print(1/0)
    return 'Hello World'

if __name__ == '__main__':
    manager = run()
```

运行服务器

```bash
python app.py runserver -h 127.0.0.1 -p 5000 -d
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 157-852-630
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

```

访问服务器

![访问服务器](https://raw.githubusercontent.com/TengTengCai/my_exp_doc/master/Flask/img/fangwenfuwuqi.png)

当我们将鼠标放在每一行代码时都会出现一个控制台的小图标

![控制台按钮](https://raw.githubusercontent.com/TengTengCai/my_exp_doc/master/Flask/img/wangyekongzhitai.png)

点击按钮,输入在启动服务器时，显示的PIN值

![输入PIN](https://raw.githubusercontent.com/TengTengCai/my_exp_doc/master/Flask/img/shurupin.png)

进行调试

![进行调试](https://raw.githubusercontent.com/TengTengCai/my_exp_doc/master/Flask/img/kongzhitaishuchu.png)



