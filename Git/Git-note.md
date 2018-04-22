[TOC]
# Git常用命令

## git init
初始化代码仓库
**实例**

```bash
[tianjun@localhost ~]$ cd test/
[tianjun@localhost test]$ git init
初始化空的 Git 版本库于 /home/tianjun/test/.git/
```

## git add

将文件从工作目录添加到缓存区（Index）

**实例**

```bash
[tianjun@localhost test]$ touch README.md
[tianjun@localhost test]$ git add README.md
```

添加全部文件到缓存区

```bash
[tianjun@localhost test]$ git add *
```

## git commit

将缓存区的改动提交到HEAD中，但是并没有提交到远程仓库中

**实例**

```bash
[tianjun@localhost test]$ git commit -m '创建readme.md文件'
[master（根提交） d259848] 创建readme.md文件
 Committer: TianJun <tianjun@localhost.localdomain>
您的姓名和邮件地址基于登录名和主机名进行了自动设置。请检查它们正确
与否。您可以通过下面的命令对其进行明确地设置以免再出现本提示信息：

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

设置完毕后，您可以用下面的命令来修正本次提交所使用的用户身份：

    git commit --amend --reset-author

 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md

```

## git config

对当前系统中的Git进行配置相应参数

**实例**

配置用户名和邮箱

```bash
[tianjun@localhost test]$ git config --global user.name "TengTengCai"
[tianjun@localhost test]$ git config --global user.email 1021766585@qq.com
```

## git remote add origin &lt;server&gt;

设置远端仓库地址，可以将本地仓库与远程服务器进行链接

**实例**

```bash
[tianjun@localhost test]$ git remote add origin git@github.com:TengTengCai/test.git
```

**小技巧**
> 当源不可用或地址出错时，可以在.git/congif中手动的配置源的地址
> ```bash
> [tianjun@localhost test]$ cd .git/
> [tianjun@localhost .git]$ vim config 
>
>   1 [core]
>   2     repositoryformatversion = 0
>   3     filemode = true
>   4     bare = false
>   5     logallrefupdates = true
>   6 [remote "origin"]
>   7     url = https://github.com/TengTengCai/test.git  # 修改该URL地址即可
>   8     fetch = +refs/heads/*:refs/remotes/origin/*
>   9 [branch "master"]
>  10     remote = origin
>  11     merge = refs/heads/master
> ```
>

## git push -u origin &lt;branch&gt;

将本地仓库的参数分支推送到远端仓库中

**实例**

```bash
[tianjun@localhost test]$ git push -u origin master
Username for 'https://github.com': 1021766585@qq.com
Password for 'https://1021766585@qq.com@github.com': 
Counting objects: 3, done.
Writing objects: 100% (3/3), 237 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/TengTengCai/test.git
 * [new branch]      master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
```

## git checkout -b &lt;branch&gt;

在本地新建一个分支，并切换到新建分支

**实例**

```bash
[tianjun@localhost test]$ git checkout -b ttc
切换到一个新分支 'ttc'
```

## git branch

查看当前所有分支

**实例**

```bash
[tianjun@localhost test]$ git branch
  master
* ttc
```

## git checkout &lt;branch&gt;

切换分支

**实例**

```bash
[tianjun@localhost test]$ git checkout master 
切换到分支 'master'
```

## git branch -d &lt;branch&gt;

删除分支

**实例**

```bash
git branch -d ttc
已删除分支 ttc（曾为 d259848）。
```

## git diff &lt;branch1&gt; &lt;branch2&gt;

比较分支1和分支2的不同

**实例**

```bash
[tianjun@localhost test]$ git diff ttc master
diff --git a/README.md b/README.md
index ce071e8..e69de29 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +0,0 @@
-# test
- 
-This branch is TTC
```

## git merge &lt;branch&gt; 

将参数分支合并到当前分支

**实例**

将ttc分支合并到master分支

```bash
[tianjun@localhost test]$ git commit -m 'this is master'
[master 0a856b8] this is master
 1 file changed, 3 insertions(+)
[tianjun@localhost test]$ git merge ttc
自动合并 README.md
冲突（内容）：合并冲突于 README.md
自动合并失败，修正冲突然后提交修正的结果。
```

当合并分支时发生冲突，需要手动的修改冲突。我们可以打开冲突文件可以看到

```bash
  1 <<<<<<< HEAD
  2 # master
  3 
  4 This branch is master
  5 =======
  6 # test
  7 
  8 This branch is TTC
  9 >>>>>>> ttc

```

这时只需要删除你不需要的部分，保留你需要的部分

我将文件修改为

```bash
# master

This branch is master what have merged ttc. 
```

处理完成冲突后，需要将文件加到缓存区，然后再commit到HEAD，就完成了冲突的处理

```bash
[tianjun@localhost test]$ git add README.md
[tianjun@localhost test]$ git commit -m '合并了tcc分支'
[master 6b42974] 合并了tcc分支
```

## git tag -a [版本号] -m [注解]

为当前版本的文件添加一个版本标签和响应的注解

**实例**

为项目添加一个版本号为v0.0.1 注解为‘这是一个测试项目’的tag

```bash
[tianjun@localhost test]$ git tag -a v0.0.1 -m '这是一个测试项目'
```

## git push origin [版本号]

将与参数对应版本的tag push到远程仓库中

**实例**

将版本号为v0.0.1的tag 推送到远端服务器上

```bash
[tianjun@localhost test]$ git push origin v0.0.1
Username for 'https://github.com': 1021766585@qq.com
Password for 'https://1021766585@qq.com@github.com': 
Counting objects: 12, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (10/10), 923 bytes | 0 bytes/s, done.
Total 10 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/TengTengCai/test.git
 * [new tag]         v0.0.1 -> v0.0.1
```

## git push origin --delete &lt;branch&gt;

删除远端服务器的分支

**实例**

创建ttc分支，然后将分支push到服务器上，然后删除服务器上的分支

```bash
[tianjun@localhost test]$ git checkout ttc
切换到分支 'ttc'
[tianjun@localhost test]$ git push origin ttc
Username for 'https://github.com': 1021766585@qq.com
Password for 'https://1021766585@qq.com@github.com': 
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/TengTengCai/test.git
 * [new branch]      ttc -> ttc
[tianjun@localhost test]$ git push origin --delete ttc
Username for 'https://github.com': 1021766585@qq.com
Password for 'https://1021766585@qq.com@github.com': 
To https://github.com/TengTengCai/test.git
 - [deleted]         ttc
```

## git push origin --delete tag &lt;版本号&gt;

删除远端服务器的标签

**实例**

删除之前创建的tag v0.0.1

```bash
[tianjun@localhost test]$ git push origin --delete tag v0.0.1
Username for 'https://github.com': 1021766585@qq.com
Password for 'https://1021766585@qq.com@github.com': 
To https://github.com/TengTengCai/test.git
 - [deleted]         v0.0.1
```

## git stash 

缓存当前

## git stash list

查看缓存

## git stash apply stash@{x} &lt;缓存名称&gt;

应用对应的缓存