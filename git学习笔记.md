# git学习笔记

# 1 基础篇

## 1.1 git commit （提交修改）

### 1.1.1 简介

``` shell
git commit
```

git中的每个节点都是一个**提交记录**（提交记录非常轻量，可以快速地在这些提交记录之间切换！），可以通过命令在不同节点间切换。

执行git commit命令时，条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的**差异**打包到一起作为一个提交记录。

![git常用命令流程图](C:\Users\Administrator\Desktop\LearnGit\Pic\git常用命令流程图.png)

执行git commit之后会生成新的节点，（**learngitbranch**）中为了方便理解节点默认命名为**Cx**，在**实际应用中对应一串哈希值**。

### 1.1.2 相关用法

- 提交暂存区到本地仓库中:


```shell
git commit -m [message]
```

> [message] 可以是一些备注信息。

- 提交暂存区的指定文件到仓库区：


```shell
git commit [file1] [file2] ... -m [message]
```

- **-a** 参数设置修改文件后不需要执行 git add 命令，直接来提交

```shell
git commit -a
```

## 1.2 git checkout (检出)

### 1.2.1 简介

checkout 本意是检出的意思，也就是将某次commit的状态检出到工作区；所以它的过程是先将`HEAD`指向某个分支的最近一次commit，然后从commit恢复index，最后从index恢复工作区。

### 1.2.1 相关用法

- 切换分支：

```shell
git checkout branchname
```

- 创建分支：

```shell
git checkout -b branchname
# 相当于以下两条命令
git branch branchname
git checkout branchname
```

- 放弃修改：

```shell
# 放弃工作区中的全部修改
git checkout .
# 放弃工作区某个文件的修改
git checkout --filename
# 强制放弃index和工作区的修改
git checkout -f
```

> 如果不指定切换到哪个分支，那就是切换到当前分支，虽然HEAD的指向没有变化，但是后面的两个恢复过程依然会执行，于是就可以理解为放弃index和工作区的变动。但是出于安全考虑 git 会保持 index 的变动不被覆盖。

### 1.2.3 应用案例

待补充：

## 1.3 git branch (分支管理)

### 1.3.1 简介

使用**分支**意味着你可以从开发**主线上分离**开来，然后在不影响主线的同时继续工作。其实就相当于在说：“我想基于这个提交以及它所有的父提交**进行新的工作**。”

![git-brance](C:\Users\Administrator\Desktop\LearnGit\Pic\git-brance.svg)

### 1.3.2 相关用法

- 分支在本地完成，速度快。要创建一个新的分支，我们使用branch命令。


```shell
git branch test
```

- 将其他分支上的本地代码硬重置到某个commit Id下.

```SHELL
git branch -f [branchA] [branchB]
```

- 如果您想删除分支，我们使用-d标识。

```shell
git branch -d test
```

- 可以列出每一个分支的最后一次提交。

```shell
git branch -v
# 输出分别为 分支名  提交(commit)id  提交时间
```

- 查看哪些分支已经合并到当前分支，此列表下没有* 标记的分支可以删除，不会报错。

```shell
git branch --merged
```

- 查看还未合并到当前分支的分支，此时如果去删除未合并的分支，会报错的。

```shell
git branch --no-merged
```

### 1.3.3 应用案例

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

## 1.4 git rebase （衍合/变基）***

### 1.4.1 简介

**官方解释：**衍合是合并命令的另一种选择。合并把两个父分支合并进行一次提交，提交历史不是线性的。衍合在当前分支上重演另一个分支的历史，提交历史是线性的。 本质上，这是线性化的自动的 cherry-pick

**通俗解释：**rebase，变基，可以直接理解为改变基底。feature分支是基于master分支的B拉出来的分支，feature的基底是B。而master在B之后有新的提交，就相当于此时要用master上新的提交来作为feature分支的新基底。实际操作为把B之后feature的提交存下来，然后删掉原来这些提交，再找到master的最新提交位置，把存下来的提交再接上去（新节点新commit id），如此feature分支的基底就相当于变成了M而不是原来的B了。（注意，如果master上在B以后没有新提交，那么就还是用原来的B作为基，rebase操作相当于无效，此时和git merge就基本没区别了，差异只在于git merge会多一条记录Merge操作的提交记录）
![在这里插入图片描述](https://img-blog.csdnimg.cn/36efc2704d174acab598c4b9addd3694.png?)

此时执行

```shell
git checkout feature
git rebase master
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/12b959efcc454da5a15b9fdec493d61b.png?)

### 1.4.2 相关用法

1. 拉公共分支最新代码的时候使用rebase，也就是git pull -r或git pull --rebase。这样的好处很明显，我用rebase拉代码下来，但有个缺点就是rebase以后我就不知道我的当前分支最早是从哪个分支拉出来的了，因为基底变了嘛。
2. 往公共分支上合代码的时候，使用merge。如果使用rebase，那么其他开发人员想看主分支的历史，就不是原来的历史了，历史已经被你篡改了。举个例子解释下，比如张三和李四从共同的节点拉出来开发，张三先开发完提交了两次然后merge上去了，李四后来开发完如果rebase上去（注意李四需要切换到自己本地的主分支，假设先pull了张三的最新改动下来，然后执行<git rebase 李四的开发分支>，然后再git push到远端），则李四的新提交变成了张三的新提交的新基底，本来李四的提交是最新的，结果最新的提交显示反而是张三的，就乱套了。
3. 正因如此，大部分公司其实会禁用rebase，不管是拉代码还是push代码统一都使用merge，虽然会多出无意义的一条提交记录“Merge … to …”，但至少能清楚地知道主线上谁合了的代码以及他们合代码的时间先后顺序。

### 1.4.3 应用案例

按照上面的图解构造了提交记录，如下图所示：（ABM是master分支线，ABCD是feature分支线。这里画成了master变色分叉出来，这不影响理解，知道是表示两个分支两条线即可！）

![在这里插入图片描述](https://img-blog.csdnimg.cn/7c8dec65210643ddaa9de34641636838.png?)

此时，在feature分支上执行git rebase master

变基完成以后，ABCD是原来的feature分支线，ABMC’D’是新的feature分支线，ABM是master分支线（没有变化）

![在这里插入图片描述](https://img-blog.csdnimg.cn/edf11c808bca47d6b6eb9caf64785c07.png?)



# 2 高级篇

## 2.1 分离HEAD

### 2.1.1 简介

如果想看 HEAD 指向，可以通过 **`cat .git/HEAD`** 查看

## 2.2 相对引用

### 2.2.1 简介

通过指定提交记录哈希值的方式在 Git 中移动不太方便。在实际应用时，并没有像本程序中这么漂亮的可视化提交树供你参考，所以你就不得不用 `git log` 来查查看提交记录的哈希值。并且哈希值在真实的 Git 世界中也会更长。因此需要一种**相对引用**方法：

![img](file:///C:\Users\Administrator\AppData\Roaming\Tencent\Users\2649825643\QQ\WinTemp\RichOle\T%@}DW2WU8{DXKKEX0]FOPW.png)

### 2.2.2 相关用法

- 使用 `^` 向上移动 1 个提交记录

```shell
git checkout bugFix^
# 等效于git checkout c3   （这里的c3对应learngitbranch相对引用题目里的c3）
```

- 使用 `~<num>` 向上移动多个提交记录，如 `~3`

```shell
git checkout bugFix~3
```

## 2.3 相对引用2（略，较为简单）



## 2.4 撤销变更

### 2.4.1 简介

**官方解释：**在 Git 里撤销变更的方法很多。和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。我们这个应用主要关注的是后者。

**学习重点：**要搞清楚git reset与git reserve的区别

### 2.4.2 相关用法





git cherry-pick

cherry-pick命令"复制"一个提交节点并在当前分支做一次完全一样的新提交。

# 参考链接：

https://learngitbranching.js.org/?locale=zh_CN

https://blog.csdn.net/weixin_42310154/article/details/119004977

https://blog.csdn.net/raoxiaoya/article/details/111321583

https://www.runoob.com/git/git-tutorial.html

https://www.jianshu.com/p/6e94b5592c40

https://baijiahao.baidu.com/s?id=1700195384333056004&wfr=spider&for=pc

https://www.cnblogs.com/qxlzzj/p/15037178.html#1%E5%BC%80%E5%8F%91%E6%9C%89%E6%B2%A1%E6%9C%89%E5%8F%AF%E8%83%BD%E6%98%AF%E4%B8%80%E4%B8%AA%E4%BA%BA
