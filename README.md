# Git学习笔记

## 常用操作

### 设置 SSH Key

```
$ ssh-keygen -t rsa -C "your_email@example.com"
```
*`"your_email@example.com"`的部分请改成自己的GitHub注册邮箱地址。*
*id_rsa 文件是私有密钥，id_rsa.pub 是公开密钥。*

### 添加公开密钥

```
$ cat ~/.ssh/id_rsa.pub
```
*将上面命令执行后显示的公钥内容复制粘贴到 GitHub 中*
*公钥添加成功之后，邮箱会收到一封提示“公共密钥添加完成”的邮件。*

*完成以上设置后，就可以用手中的私有密钥与GitHub进行认证和通信了。*
```
$ git -T git@github.com
```


### clone 已有仓库

```
$ git clone git@github.com:lw1024/Git-learning-notes.git
```

### 查看当前状态

```
$ git status
```

### 提交

```
$ git add [要提交的文件]
$ git commit -m "提交信息描述"
```
### 查看提交日志

```
$ git log
```

### 进行 push

```
$ git push
```

## 基本操作

### git init——初始化仓库

```
$ mkdir git-tutorial
$ cd git-tutorial
$ git init
```

### git status——查看仓库的状态

```
$ touch README.md
$ git status
```

### git add——向暂存区中添加文件

```
$ git add README.md
$ git status
```

### git commit——保存仓库的历史记录

```
$ git commit -m "First commit"
```
**记述一行提交信息**
> -m 参数后的“First commit”称作提交信息，是对这个提交的概述。

**记述详细提交信息**
> 如果想要记述得更加详细，请不要加`- m`，直接执行`git commit`命令。执行后编辑器就会启动。
```
在编辑器中记述提交信息的格式如下：
1. 第一行：用一行文字简述提交的更改内容
2. 第二行：空行
3. 第三行以后：记述更改的原因和详细内容
```
> 将提交信息按格式记述完毕后，保存并关闭编辑器。刚才记述的提交信息就会被提交。

**终止提交**
> 如果在编辑器启动后想终止提交，请将提交信息留空并直接关闭编辑器，随后提交就会被终止。

### git log——查看提交日志

```
$ git log
```

**只显示提交信息的第一行**
```
$ git log --pretty=short
```

**只显示指定目录、文件的日志**
```
$ git log README.md
```

**显示文件的改动**
```
$ git log -p
$ git log -p README.md
```

### git diff——查看更改前后的差别

**查看工作树和暂存区的差别**

```
$ git diff
```

**查看工作树和最新提交的差别**

```
$ git add README.md
$ git diff HEAD
```
*这里的`HEAD`是指向当前分支中最新一次提交的指针。*

## 分支的操作

### git branch——显示分支一览表

```
$ git branch
* master
```
*`*`表示当前所在分支*

### git checkout -b——创建、切换分支

#### 切换到 feature-A 分支并进行提交

```
$ git checkout -b feature-A
Switched to a new branch 'feature-A'
```
*连续执行下面两条命令也能收到同样效果。*
```
$ git branch feature-A
$ git checkout feature-A
```

#### 切换到 master 分支

```
$ git checkout master
Switched to branch 'master'
```

#### 切换回上一个分支

```
$ git checkout -
Switched to branch 'feature-A'
```

### git merge——分支合并

首先切换到 `master` 分支
```
$ git checkout master
```

然后合并 `feature-A` 分支
```
$ git merge --no-ff feature-A
```

随后编辑器会启动，用于录入合并提交的信息。
默认信息中已经包含了相关内容，所以可以不必做任何修改。将编辑器中显示的内容保存，关闭编辑器，然后就会执行合并提交。

### git log --graph——以图表形式查看分支

```
$ git log --graph
```

[TOC]

