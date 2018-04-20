# Linux-Part1-文件管理

[TOC]



## cat命令

**命令：cat**

cat命令用于连接文件并打印到标准输出设备上。

**使用权限**

所有使用者

**语法格式**

```shell
cat [选项列表] [文件列表]
```

**参数说明**

**-A, --show-all **

**-b, --number-nonblank**  给非空输出行编号

**-E, --show-ends**  在每行结束显示$

**-s, --squeeze-blank**  将所有的连续的多个空行替换为一个空行

**-T,  --show-tabs** 把TAB字符显示为^I

**-v,  --show-nonprinting**  除了LFD和TAB之外所有控制符用^和M-记方式显示

**--help**  显示帮助信息并退出

**--version**  显示版本信息并退出

**实例**

查看textfile1.txt文件的内容并并打印到标准输出设备上

```shell
cat textfile1.txt
```

把textfile1.txt的文档内容加上行号后输入textfile2.txt文档中：

```shell
cat -n textfile1.txt > textfile2.txt
```

把textfile1.txt和textfile.txt的文档内容加上行号（空白行不加）之后将内容附加到textfile3.txt文档里

```shell
cat -b textfile1.txt textfile2.txt >> textfile3.txt
```

## chmod命令

**命令：chmod**

chmod - 改变文件的访问权限

**使用权限**

所有使用者

**语法格式**

```shell
chmod [-cfvR] [--help] [--version] [--]
```

**参数说明**

其中：

- u表示文件的拥有者，g表示与该文件的拥有者属于同一个群体（group）者，o表示其他以外的人， a表示这三者皆是（所有用户）。
- +表示增加权限、-表示取消权限、=表示唯一设定权限。
- r表示可读，w表示可写， x表示可执行，X表示只有当该文件是一个子目录或者该文件已经被设定过为可执行。

其他参数说明：

**-c, --changes** 只有在文件的权限确实改变时才进行详细的说明

**-f, --silent, --quiet** 不能输出权限不能改变的文件的错误信息

**-v, --verbose** 详细说明权限的变化

**-R, --recursive** 改变目录及其所有子目录的文件的权限

**--help** 显示辅助说明

**--version** 显示版本信息

**实例**

将文件file.txt设置为所有人皆可读取：

```shell
chmod ugo+r file1.txt
chmod a+r file1.txt
```

将文件file.txt设置为该文件拥有者，与其他同一个群体者可写入，但其他以外的人则不可写入

```shell
chmod ug+w,o-w file.txt
```

将hello.py设定为只有该文件拥有者才可以执行

```shell
chmod u+x hello.py
```

将目录下的所有文件与子目录皆设为任何人可读取

```shell
chmod -R a+r *
```

用数字来表示权限

用的三位二进制数来表示，如 rwx为111，转为十进制为7，rw-为110，十进制为6，r-x为101，为5，r--为100，为4

设置为所有人可读可写可执行

```shell
chmod 777 hello.py
```

## chown 命令

**命令chown**

修改文件所有者和组别

chown修改每个由第一个非选项参数声明的给定file文件的用户和/或组的所有权

**使用权限**

所有使用者

**语法格式**

```shell
chown [options] user [:group] file...
```

**参数说明**

**-R**  递归地修改目录及其下面内容的所有权

**-c, --changes** 详尽地描述每一个file实际改变了哪些所有权

**-f, --silent, --quiet**  不打印文件所有全部能修改的报错信息

**-h, --no-dereference**  只作用于其本身的符号链接，而部修改他们所指向的文件， 这只在提供了lchown系统调用的情况下才使用

**-v, --verbose** 详尽地描述对每个file所执行的操作（或者无操作）

**--dereference**  修改符号链接目标端的所有权，而非符号链接自身

**--reference=rfile**  修改file的所有权为rfile的所有权

**--help**  在标准输出上打印一条用法信息，并以成功状态退出

**--version**  在标准输出上打印版本信息，然后以成功状态退出

**实例**

将文件file1.txt的拥有者设为users群体的使用者tengtengcai

将文件file2.txt的拥有者设为root超级管理员的使用者

```shell
chown tengtengcai:users file1.txt
chown root file2.txt
```

## cmp命令

**命令cmp**

比较两个文件一个字节一个字节的不同

**语法**

```shell
cmp [-clsv][-i <字符数目>] [--help] [文件名称1] [文件名称2]
```

**参数**

**-b, --print-bytes** 输出不同的字节

**-i, --ignore-initial** 跳过两个输入的第一个跳过字节

**-l, --verbose** 输出两个文件不同字节数的值

**-n, --bytes=LIMIT** 最多比较limit个字节

**-s, --quiet, --silent**  支持所有的普通输出

**--help** 显示帮助信息并正常退出

**-v, --version** 输出版本信息并正常退出

**实例**

比较file1.py和file2.py两文件字节的不同

```shell
cmp file1.py file2.py
```

## diff命令

**命令diff**

找出两个文件的不同点

**语法**

```shell
diff [选项] 源文件 目标文件
```

**参数**

**-a** 所有的文件都视为文本文件来逐行比较，甚至他们似乎不是文本文件

**-b** 忽略空格引起的变化

**-B** 忽略插入删除空行引起的变化

**--brief** 仅报告文件是否相异，在乎差别的细节

**-c** 使用上下问输出格式

**-C 行数**（一个整数）

**--context[=lines]** 使用上下问输出格式，显示以指定行数（一个整数）， 或者是三行（当行数没有给出时，对于正确的操作，上下文至少要有两行）

**-i** 忽略大小写

**-r** 比较子目录中的文件

**-v** 显示版本信息，并正常退出

**-help** 显示帮助信息，并正常退出

**实例**

比较两个文件的不同

```shell
diff cal.py cal1.py 
3c3
< 
---
> # 这是注释
30c30
< 
---
> # 这也是个注释
```

## file命令

**命令file**

确定文件类型

**语法**

```shell
file [-bcnsvzL][-f 命令文件][-m 幻数文件] file...
```

**参数**

**-b** 不输出文件名（简要模式）

**-c** 检查时打印输出幻数文件的解析结果，常与-m一起使用，用了在安装幻数文件之前调试它.

**-f  命名文件**  从参数表前的[命名文件], 中读出将要检查的文件名（每行一个文件）， 要有[命名文件]，或者至少有一个文件名参数；如果要检查标准输入，使用‘’-‘’作为文件参数.

**-m list** 指定包含幻数的文件列表，可以是单个文件，也可以是用冒号分开的多个文件.

**-n** 每检查完一个文件就强制刷新标准输出，仅在检查一组文件时才有效，一般在将文件类型输出到管道时才采用此选项.

**-v** 打印程序版本并退出.

**-z** 视图查看压缩文件内部信息.

**-L** (在支持符号链接的系统上)选项显示符号链接文件的原文件，就像**ls**命令的like-name选项

**-s** 通常，file只是视图去检查在文件列表中那些stat报告为正常文件的文件类型，由于读特殊文件将可能导致不可知后果，所以这样可以防止发生问题。使用-s选项时file命令也将去读取文件列表中的块特殊文件和字符特殊文件.

**实例**

显示文件类型：

```shell
file test.py 
test.py: Python script, UTF-8 Unicode text executable

# 不显示文件名称
file -b test.py   
Python script, UTF-8 Unicode text executable

# 显示MIME类型
file -i test.py
test.py: text/x-python; charset=utf-8

# 不显示名字而且显示出MiME类型
file -bi test.py
text/x-python; charset=utf-8
```

显示符号链接的文件类型

```bash
file nginxhost/
nginxhost/: directory

file nginxhost
nginxhost: symbolic link to `/usr/share/nginx/html/'
```

## git命令

**命令git**

git是文字模式下的管理员，git是用来管理文件的程序，是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。Git是Linus Torvalds为了帮助管理Linux内核开发而开发的一个开放源码的版本控制软件。

**语法**

```bash
git [选项参数]
```

**参数**

**add** 添加文件到索引

**clone** 将存储库克隆到新目录中

**commit** 记录对数据库的更改

**diff** 显示提交和提交工作树之间的更改

**init** 创建一个空Git存储库或重新初始化一个已存在的

**log** 显示提交日志

**mv ** 移动或重命名文件、目录或symlink

**pull** 从另一个存储库或本地分支获取和合并

**push** 与相关对象一起更新远程引用

**rm** 从工作树和索引中删除文件

**show** 显示各种类型的对象

**status** 显示工作数状态

**tag** 创建、列出、删除或验证用签名标记的对象

**实例**

初始化一个仓库

```bash
git init
Initialized empty Git repository in /home/tianjun/pycode/.git/
```

克隆一个github上的一个项目

```bash
git clone https://github.com/TengTengCai/BookCrawler.git
Cloning into 'BookCrawler'...
remote: Counting objects: 118, done.
remote: Compressing objects: 100% (48/48), done.
remote: Total 118 (delta 36), reused 49 (delta 19), pack-reused 48
Receiving objects: 100% (118/118), 71.65 KiB | 13.00 KiB/s, done.
Resolving deltas: 100% (62/62), done.
```

文件到索引

```bash
git add code.py
```

将记录变更到文件库

```bash
git commit -m '版本说明,消息'
```

创建分支

```bash
git checkout -b tj
```

查看分支

```bash
git branch
  master
* tj
```

切换分支

```bash
git branch master
```

删除分支

```bash
git checkout -D tj
```

将文件库发送到远端服务器进行更新，只有配置了远端服务器才会生效

```bash
git push origin <分支名>
```

从远端服务器进行更新或回退文件库版本,只有配置了远端服务器才会生效

```bash
git pull origin <分支名>
```

## cut命令

**命令cut**

在文件的每一行中提取片断，在每个文件file的各行中，把提取的片段显示在标准输出

**语法**

```bash
cut [option] [file]
```

**参数**

**-b, --bytes=LIST** 输出这些字节，这些字符位置将忽略多字符边界，除非也指定了-n标志

**-c, --character=LIST** 输出这些字符

**-d, --delimiter=DELIM** 自定义分隔符，默认为制表符

**-f, --fields=LIST** 与-d一起使用，指定显示哪个区域

**-n** 取消分割多字节符。仅和-b标志一起使用。如果字符的最后一个字符落在由-b标志的List参数指示的范围之内，该字符将被写出，否则，该字符将被移除

**实例**

查看文件中每行的第一个字符

```bash
cat hello.txt 
你好
我是藤藤菜
他
cut -c 1 hello.txt 
你
我
他
```

## ln命令

**命令ln**

ln命令是一个飞车重要的命令，它的功能是为某一文件在另外一个位置建立一个同步的链接。当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只需要在某一个固定的目录，放上该文件，然后在其他的目录下用ln命令链接（link）它就可以，不必重复的占用磁盘空间。

**语法**

```bash
ln [参数] [源文件或目录] [目标文件或目录]
```

**软链接**

- 软链接，以路径的形式存在，类似于windows操作系统中的快捷方式
- 软链接可以跨文件系统，硬链接不可以
- 软链接可以对一个不存在的文件名进行链接
- 软连接可以对目录进行链接

**硬链接**

- 硬链接，以文件副本的形式存在。但不占用实际空间
- 不允许给目录创建硬链接
- 硬链接只有在同一个文件系统中才能创建

**参数**

**-b** 删除，覆盖以前建立的链接

**-d** 允许超级用户制作目录的硬链接

**-f** 强制执行

**-i** 交互模式，文件存在则提示用户是否覆盖

**-n** 把符号链接视为一般目录

**-s** 软链接（符号链接）

**-v** 显示详细的处理过程

**实例**

## less命令

**命令less**

它与more类似，但使用less可以随意浏览文件，而more仅能向前移动，却不能向后移动，而且less在查看之前不会加载整个文件

**语法**

```bash
less [参数] [文件]
```

**参数**

**-b** <缓冲区大小> 设置缓存区的大小

**-e** 当文件显示结束后，自动离开

**-f** 强迫打开特殊文件，例如外围设备代号、目录和二进制文件

**-g** 只标志最后搜索的关键词

**-i** 忽略搜索时的大小写

**-m** 显示类似more命令的百分比

**-N** 显示每行的行号

**-o <文件名>** 将less输出的内容在指定文件中保存起来

**-Q** 不使用警告音

**-s** 显示连续空行为一行

**-S** 行过长时将超出部分舍弃

**-x<数字>** 将“tab”键显示为规定的数字空格

**/字符串** 向下搜索字符串的功能

**?字符串** 向上搜索字符串的功能

**n** 重复前一个搜索（与/或?有关）

**N** 反向重复前一个搜索（与/或?有关）

**b** 向后翻一页

**d**向后翻半页

**h** 显示帮助界面

**Q** 退出less命令

**u** 向前滚动半页

**y** 向前滚动一行

**空格键** 滚动一页

**回车键** 滚动一行

**[pagedown]** 向下翻动一页

**[pageup]** 向上翻动一页

**实例**

查看文件

```bash
less myredis.conf
```

ps查看进程信息并通过less分页显示

```bash
ps -ef | less
```



## more命令

**命令more**

类似cat，不过会以一页一页的形式显示，更方便使用者逐页阅读，而基本的命令就是按空白键（space）就往下一页显示，按b键就会往回（back）一页显示，而且还有搜寻字符串的功能（与vi相似）使用中的说明文件，按h

**语法**

```bash
more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames...]
```

**参数**

**-num** 一次显示的行数

**-d** 提示使用者，在画面下方显示[Press space to continue, 'q' to quit], 如果使用者按错键，则会显示[Press 'h' for instructions] 而不是‘哔’一声

**-l** 取消遇见特殊字元^L（送纸字元）时会暂停的功能

**-f** 计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）

**-p** 不以卷动的方式显示每一页，而是先清除屏幕后再显示内容

**-c** 于-p相似，不同的是先显示内容再清除其他旧资料

**-s**当遇到有连续两行以上的空白行，就代换为一行的空白行

**-u** 不显示下引号

**+num**从第num行开始显示

**fileNames** 欲显示内容的文档，可为复数个数

**实例**

逐页显示myredis.conf文件

```bash
more myredis.conf
```

从第480行开始显示myredis.conf文件

```bash
more +480 myredis.conf
```



## mv命令

**命令mv**

用来为文件或目录改名、或将文件或目录移入其他位置。

**语法**

```bash
mv [选项] ... 源文件 目标文件
mv [选项] ... 源文件... 目录
mv [选项] ... --target-directory=DIRECTORY SOURCE...
```

**参数**

**-b** 为现有的每个目标文件作一个备份

**-f, --force** 覆盖前永不提示

**-i, --interactive** 覆盖前提示

**-u, --update** 只移动更老的或者标记新的非目录

**-v, --verbose** 说明完成了什么

**--help** 显示帮助并退出程序

**--version** 输出版本信息且退出程序

**实例**

将文件aaa更名为bbb

```bash
mv aaa bbb
```

将info目录放入logs目录中。注意，如果logs目录不存在，则该命令info改名为logs

```bash
mv info/ logs
```

将/home/tengtengcai 下的所有文件和目录移到当前目录下

```bash
mv /usr/student/* .
```

## rm命令

**命令rm**

移除文件或目录

**语法**

```bash
rm [操作] [文件或目录]
```

**参数**

**-i** 删除前逐一询问确认

**-f** 即使原档案属性设为唯读，亦直接删除，无需逐一确认

**-r** 将目录及以下之档案亦逐一删除

**实例**

删除文件可以直接使用rm命令，若删除目录则必须配合选项“-r”，例如

```bash
rm -i test.txt 
rm：是否删除普通空文件 "test.txt"？y
```

不询问，直接删除homework目录下的所有文件

```bash
rm -rf homework/*
```

文件一旦通过rm命令删除，则无法恢复，所以必须格外小心地使用该命令

## touch命令

**命令touch**

用于修改文件或者目录的时间属性，包括存取和更改时间。若文件不存在，系统会创建一个新的文件。

**语法**

```bash
touch [-acm] [-r ref_file(参照文件)] | -t time(时间值)] | file...
```

**参数**

**-a** 修改文件file的存取时间

**-c** 不创建文件file

**-m** 修改文件file file

**-r ref_file** 将参照文件 ref_file相应的时间戳记的数值作为指定文件 file时间戳记的新值

**-t time ** 使用指定的时间值time做为指定文件file相应时间戳记的新值，此处的time规定为如下形式的十进制数:[[CC]YY]MMDDhhmm[.SS]

**实例**

使用指令修改文件的时间属性为当前系统时间

```bash
touch test.txt
```

使用指令时，如果指定的文件不存在，则将创建一个新的空白文件，如，在当前目录下使用指令创建一个空白文件file

```bash
touch file
```

## cp命令

**命令cp**

复制文件和目录

**语法**

```bash
cp [选项] 文件路径
cp [选项] 文件...目录
```

**参数**

**-a, --archive**复制时，尽可能保持文件的结构和属性.但不保持目录结构 等同于 -dpR

**-d, --no-dereference** 复制符号链接作为符号链接而不是复制它指向的文件，并且保护在副本中原文件之间的硬链接.

**-f, --force** 删除存在的目标文件

**-i, --interactive** 无论是否覆盖现存文件都作提示

**-l, --link** 制作硬链接代替非目录拷贝

**-p, --parents** 保持原始文件的所有者，组，许可和时间表属性

**-r** 递归地复制目录，复制任何非目录和非符号链接

**-R, --recursive** 递归地复制目录，保留非目录

**实例**

使用命令将当前目录test下所有文件复制到新目录newdir下

```bash
cp -r test/* newdir
```