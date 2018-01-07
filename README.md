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

---

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

---

## 更改提交的操作

### git reset——回溯历史版本

#### 回溯到创建 feature-A 分支前

```
$ git reset --hard 09c87c50c6b09ca50c39b4a1e3be2515c14de2ce
```

#### 创建 fix-B 分支

```
$ git checkout -b fix-B
```
在 README.md 文件中添加一行文字，然后提交。
```
$ git add README.md
$ git commit -m "Fix B"
```

#### 推进至 feature-A 分支合并后的状态

首先，查看当前仓库执行过的操作日志。
```
$ git reflog
```
在日志记录中找到 feature-A 分支合并后的状态所对应的哈希值。
```
$ git checkout master
$ git reset --hard d59e11a
```

*消除冲突*
进行合并操作。
```
$ git merge --no-ff fix-B
```
*此时可能会产生冲突。用编辑器打开发生冲突的文件，查看冲突部分并将其解决。*
提交解决后的结果
```
$ git add README.md
$ git commit -m "Fix conflict" 
```

### git commit --amend——修改提交信息

```
$ git commit --amend
```
执行上面的命令后，编辑器就会启动。请将提交信息的部分修改为 Merge branch 'fix-B'，然后保存文件，关闭编辑器。随后修改就完成了。

### git rebase -i——压缩历史

#### 创建 feature-C 分支

```
$ git checkout -b feature-C
```
然后在 README.md 文件中添加一行文字，并故意留下拼写错误，一边之后修正。

提交这部分内容
```
$ git commit -am "Add feature-C"
```

#### 修正拼写错误

现在打开 README.md 文件，并修正错误。然后进行提交。
```
$ git commit -am "Fix typo"
```

#### 更改历史

因为这类错误并不需要在历史记录中展现出来，所以我们将这次提交与之前的一次提交合并。
```
$ git rebase -i HEAD~2
```
*上述命令可以选定当前分支中包含 HEAD （最新提交）在内的`两个`最新历史记录为对象，并在编辑器中打开。 *
```
pick 7a34294 Add feature-C
pick 6fba227 Fix typo
```

我们将`6fba227`的`Fix typo`的历史记录压缩到`7a34294`的`Add feature-C`里。
按照下面所示，将`6fba227`左侧的`pick`改写为`fixup`。
```
pick 7a34294 Add feature-C
fixup 6fba227 Fix typo
```
保存并关闭编辑器。
系统提示 rebase 成功。也就是将“Fix typo”的内容合并到“Add feature-C”中，并改写成一个新的提交（哈希值会更改）。

#### 合并至 master 分支

```
$ git checkout master
$ git merge --no-ff feature-C
```

## 推送至远程仓库

首先，在 GitHub 上创建一个仓库，并将其设置为本地仓库的远程仓库。
*为防止与其他仓库混淆，仓库名请与本地仓库保持一致，即 git-tutorial。*

### git remote add——添加远程仓库

```
$ git remote add origin git@github.com:用户名/git-tutorial.git
```
*执行命令之后，git 会自动将远程仓库的名称设置为 origin。*

### git push——推送至远程仓库

#### 推送至 master 分支

如果想将**当前分支下**本地仓库中的内容推送给远程仓库，需要用到 git push命令。
```
$ git push -u origin master
```
*-u 参数可以在推送的同时，将 origin 仓库的 master 分支设置为本地仓库当前分支的 upstream（上游）。添加了这个参数，将来运行 `git pull` 命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从 origin 的 master 分支获取内容，省去了另外添加参数的的麻烦。*

#### 推送至 master 以外的分支

```
$ git checkout -b feature-D
$ git push -u origin feature-D
```

## 从远程仓库获取

从实际开发者的角度出发，在另一个目录下新建一个本地仓库，学习从远程仓库获取内容的相关操作。

### git clone——获取远程仓库

#### 获取远程仓库

首先，切换到其他目录下，将 GitHub 上的仓库 clone 到本地。
```
$ git clone git@github.com:用户名/git-tutorial.git
```
*执行`git clone`命令后默认处于 master 分支下，同时系统会自动将 origin 设置成改远程仓库的额标识符。*

查看当前分支的相关信息
```
$ git branch -a
```
*添加`-a`参数可以同时显示本地仓库和远程仓库的分支信息。*

#### 获取远程的 feature-D 分支

```
$ git checkout -b feature-D origin/feature-D
```
*为了便于理解，我们仍将其命名为 feature-D。*

#### 向本地的 feature-D 分支提交更改

在 README.md 文件中添加一行文字。然后提交。
```
$ git commit -am "Add feature-D"
```

#### 推送 feature-D 分支

```
$ git push
```

### git pull——获取最新的远程仓库分支

现在，回到原先的那个目录下。
将当前分支切换到 feature-D 分支。
```
$ git pull origin feature-D
```


<!--[TOC]-->

