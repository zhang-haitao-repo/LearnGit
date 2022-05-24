# git学习笔记

## 1 基础篇

### 1.1 git commit 命令

#### 1.1.1 简介

``` cmd
git commit
```

git中的每个节点都是一个**提交记录**（提交记录非常轻量，可以快速地在这些提交记录之间切换！），可以通过命令在不同节点间切换。

执行git commit命令时，条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的**差异**打包到一起作为一个提交记录。

![git常用命令流程图](C:\Users\Administrator\Desktop\LearnGit\Pic\git常用命令流程图.png)

执行git commit之后会生成新的节点，节点默认命名为Cx。

#### 1.1.2 相关用法

提交暂存区到本地仓库中:

```cmd
git commit -m [message]
```

[message] 可以是一些备注信息。

提交暂存区的指定文件到仓库区：

```cmd
git commit [file1] [file2] ... -m [message]
```

**-a** 参数设置修改文件后不需要执行 git add 命令，直接来提交

```cmd
git commit -a
```

### 1.2 git branch (分支管理)

#### 1.2.1 简介

使用**分支**意味着你可以从开发**主线上分离**开来，然后在不影响主线的同时继续工作。其实就相当于在说：“我想基于这个提交以及它所有的父提交**进行新的工作**。”

![git-brance](C:\Users\Administrator\Desktop\LearnGit\Pic\git-brance.svg)

#### 1.2.2 相关用法

分支在本地完成，速度快。要创建一个新的分支，我们使用branch命令。

```
git branch test
```

branch命令不会将我们带入分支，只是创建一个新分支。所以我们使用checkout命令来更改分支。

```
git checkout test
```

第一个分支，或主分支，被称为"main"。

```
git checkout main
```

对其他分支的更改不会反映在主分支上。如果想将更改提交到主分支，则需切换回master分支，然后使用合并。

```
git checkout main
git merge test
```

如果您想删除分支，我们使用-d标识。

```
git branch -d test
```

#### 1.2.3 应用案例

- 开始前我们先创建一个测试目录：

```shell
$ mkdir gitdemo
$ cd gitdemo/
$ git init
Initialized empty Git repository...
$ touch README
$ git add README
$ git commit -m '第一次版本提交'
[master (root-commit) 3b58100] 第一次版本提交
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README
```

- 列出分支基本命令：git branch

```shell
$ git branch
* master
```

> 没有参数时，**git branch** 会列出你在本地的分支。
>
> 此例的意思就是，我们有一个叫做 **master** 的分支，并且该分支是当前分支。
>
> 当你执行 **git init** 的时候，默认情况下 Git 就会为你创建 **master** 分支。
>
> 如果我们要手动创建一个分支。执行 **git branch (branchname)** 即可。

```shell
$ git branch testing
$ git branch
* master
  testing
```

> 现在我们可以看到，有了一个新分支 **testing**。

- 接下来使用git checkout (branch) 切换到我们要修改的分支。

```shell
$ ls
README
$ echo 'runoob.com' > test.txt
$ git add .
$ git commit -m 'add test.txt'
[master 3e92c19] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
$ ls
README        test.txt
$ git checkout testing
Switched to branch 'testing'
$ ls
README
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```

> 当我们切换到 **testing** 分支的时候，我们添加的新文件 test.txt 被移除了。切换回 **master** 分支的时候，它们又重新出现了。

- 使用 git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。

```shell
$ git checkout -b newtest
Switched to a new branch 'newtest'
$ git rm test.txt 
rm 'test.txt'
$ ls
README
$ touch runoob.php
$ git add .
$ git commit -am 'removed test.txt、add runoob.php'
[newtest c1501a2] removed test.txt、add runoob.php
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```
