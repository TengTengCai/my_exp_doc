# Flask Template模版引擎(Jinja2)

Flask利用Jinja2作为模版引擎。模版引擎包含了变量和表达式，当模版被渲染时，它们被替换为值和标签，它们控制着模版的逻辑。

Jinja2默认的几种分割符：

- `{% ... %}`  表示声明
- `{{ ... }}`  表达式打印到模版输出
- `{# ... #}`  对于未包含在模板输出中的注释
- `# ... ##` 行语句

## 变量

传递给模版的上下文字典定义了模版的变量

通过`{{ ... }}`在模版中打印变量

```jinja2
{{ foo.bar }}
{{ foo['bar'] }}
```

双花括号不是变量的一部分，而是打印语句。如果访问标签内的变量，在双花括号周围不要再次出现大括号。

如果变量或属性不存在，将会返回一个未定义的值，默认的行为是打印或迭代计算为空字符串，并且每次操作都会失败。

在Flask中如果需要加载静态文件的URL，有两种实现的方法：

```jinja2
<link rel='stylesheel' href='/static/css/style.css'>
<link rel='stylesheel' href='{{ url_for('static', filename='css/style.css') }}'>
```



## 过滤器

变量可以通过过滤器进行修改。过滤器通过管道符合（`|`）与变量分开，并且可以在括号中包含可选参数。可以链接多个过滤器。一个过滤器的输出应用于下一个过滤器。

例如`{{ name | striptags | length }}`从name变量中移除所有的HTML标签，并计算长度进行打印输出。

接受参数的过滤器在参数周围有括号，就像函数调用一样。例如：`{{ listx | join (',') }}`将以逗号为间隔将列表进行打印输出，与Python的`str.join(',', listx)`相似。

## 转义

有时需要或必要让Jinja2忽略它会以变量或块的形式处理的部分。例如，如果使用迷人语法，你希望使用{{作为模版中的原始字符串并且不启动变量。

```jinja2
{{ '{{' }}
```

对于更大的部分，将原始块标记为块是有意义的，使用`{% raw %}`。例如，要在模版中包含示例Jinja语法，且不执行代码。

```jinja2
{% raw %}
	<ul>
	{% for item in seq %}
		<li>{{ item }}</li>
	{% endfor %}
	</ul>
{% endraw %}
```

## 基本模版

通常会创建一个base.html模版，它定义了一个简单的HTML骨架文档，你可以将它用于简单的三段式页面。让孩子模版来完成空白块内容的填充工作。

```Jinja2
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <title>{% block title %}{% endblock %}</title>
    {% block extCSS %}
		<link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
    {% endblock %}
</head>
<body>
{% block header %}

{% endblock %}

{% block content %}

{% endblock %}

{% block footer %}

{% endblock %}
</body>
</html>
```

在本例中，`{% block %} ` 标签定义了五个可以填充子模版的块，所有块标签都会告诉模板引擎，子模板可能会覆盖模板中的占位符。

## 子模板

下面介绍下子模版中的示例：

```jinja2
{% extends 'base.html' %}
{% block title %}Index{% endblock %}
{% block extCSS %}
	{{ super() }}
	<style>
		.test{color: red;}
	</style>
{% endblock %}
{% block header %}<p>header</p>{% endblock %}
{% block content %}<p>content</p>{% endblock %}
{% block footer %}<p>footer</p>{% endblock %}
```

`{% extends %}`标签是这里的关键。它告诉模版引擎该模板‘扩展’了另一个模板。当模版系统读取这个模板时，它会首先找到父模板。扩展标签应该是模板中的第一个标签。

通过`{{ super() }}` 来渲染父模块的内容。这将返回父模块的结果。

## 结构控制语句

控制结构是指所有那些控制程序流程的东西-条件（if/elif/else）, for循环，以及宏和块。使用默认语法，控制结构出现在`{% ... %}`块内。

### For 

循环顺序中的每个项目。例如，显示一个名为users的变量中提供的用户列表。

```jinja2
<h1>User List</h1>
<ul>
{% for user in users %}
	<li>user.username</li>
{% endfor %}
</ul>
```

由于模板中的变量保留了它们的对象属性，因此可以像dict一样遍历容器：

```jinja2
<dl>
{% for key, value in my_dict.iteritems() %}
	<dt>{{ key }}</dt>
	<dd>{{ value }}</dd>
{% endfor %}
</dl>
```

在for循环块的内部， 可以访问一些特殊变量

| 值                 | 描述                                                 |
| ------------------ | ---------------------------------------------------- |
| loop.index         | 循环的当前迭代（1开始）                              |
| loop.index0        | 循环的当前迭代（0开始）                              |
| loop.revindex      | 循环倒叙的迭代（到1结束）                            |
| loop.revindex0     | 循环倒叙的迭代（到0结束）                            |
| loop.first         | 如果第一次迭代，则为真                               |
| loop.last          | 如果最后次迭代， 则为真                              |
| loop.length        | 序列中的项目数量。                                   |
| loop.cycle         | 在一系列序列之间循环的辅助函数。请参阅下面的说明。   |
| loop.depth         | 指示当前呈现的递归循环中有多深。从1开始              |
| loop.depth0        | 指示当前呈现的递归循环中有多深。从0开始              |
| loop.previtem      | 来自上一次循环迭代的项目。在第一次迭代期间未定义。   |
| loop.nextitem      | 来自下一次循环迭代的项目。在最后一次迭代期间未定义。 |
| loop.changed(*val) | 如果以前用不同的值调用（或根本不调用），则为真。     |

### IF

与Python中的if语句相当。用最简单的形式，你可以用它来测试一个变量是否被定义，不是空的和不是false

```jinja2
{% if users %}
<ul>
{% for user in users %}
	<li>{{ user.username }}</li>
{% endfor %}
</ul>
{% endif %}
```

对于多个分支，elif和else可以像在Python中一样使用， 也可以使用更复杂的表达式。

```jinja2
{% if age < 18 %}
	你还未成年
{% elif age == 18 %}
	刚好成年
{% else %}
	你已经不小了
{% endif %}
```

## Macros

宏与常规编程语言中的函数具有可比性。将常用惯用语放入可重复使用的函数中，以避免重复干同样的事情。

```jinja2
{% macro input(name, value='', type='text', size=20) -%}
    <input type="{{ type }}" name="{{ name }}" value="{{
        value|e }}" size="{{ size }}">
{%- endmacro %}
```

宏可以被当做一个函数调用：

```jinja2
<div>{{ input('username') }}</div>
<div>{{ input('password', type='password') }}</div>
```

## Blocks

块用于继承，同时用作占位符和替换。它们在“模板继承”部分详细记录。 

## Include

include语句对于包含模板并将该文件的渲染内容返回到当前名称空间非常有用： 

```jinja2
{% include 'header.html' %}
    Body
{% include 'footer.html' %}
```

