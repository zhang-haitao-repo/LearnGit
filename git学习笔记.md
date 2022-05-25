# Git学习笔记

# 1 基础篇

## 1.1 git commit （提交修改）-常用

### 1.1.1 简介

git中的每个节点都是一个**提交记录**（提交记录非常轻量，可以快速地在这些提交记录之间切换！），可以通过命令在不同节点间切换。

执行git commit命令时，条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的**差异**打包到一起作为一个提交记录。

![git常用命令流程图](C:\Users\Administrator\Desktop\LearnGit\Pic\git常用命令流程图.png)

执行git commit之后会生成新的节点，（**learngitbranch**）中为了方便理解节点默认命名为**Cx**，在**实际应用中对应一串哈希值**。

**在进一步学习之前，还需知道工作区、暂存区和版本库的具体含义：**

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 .git 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。
- 当对**工作区**修改（或新增）的文件执行git add命令时，**暂存区**的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作git commit时，**暂存区**的目录树写到**版本库（对象库）**中，master分支会做相应的更新。即 master指向的目录树就是提交时暂存区的目录树。



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

## 1.2 git checkout (检出)-常用

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

## 1.3 git branch (分支管理)-常用

### 1.3.1 简介

使用**分支**意味着你可以从开发**主线上分离**开来，然后在不影响主线的同时继续工作。其实就相当于在说：“我想基于这个提交以及它所有的父提交**进行新的工作**。”

![git-brance](C:\Users\Administrator\Desktop\LearnGit\Pic\git-brance.svg)

### 1.3.2 相关用法

- 分支在本地完成，速度快。要创建一个新的分支，我们使用branch命令。


```shell
git branch test
```

- 将其他分支上的本地代码硬重置到某个commit Id下.（常用）

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

## 1.4 git rebase (衍合/变基)-较难理解

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

```shell
git rebase [Abranch] [Bbranch]
```

> 将A分支合并到B分支，合并方式为变基合并。（具体可参考()[https://learngitbranching.js.org/]->高级话题->3.纠缠不清的分支）

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



## 2.4 撤销变更-reset

### 2.4.1 简介

**官方解释：**在 Git 里撤销变更的方法很多。和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。我们这个应用主要关注的是后者。

**撤销修改**分为以下三种情况：

1. 已经`push`推送到远程仓库。
2. 已经`commit`提交到版本库。
3. 已经`add`提交到暂存区。
4. 暂未提交到暂存区，所有修改都在工作区。



**学习重点：**要搞清楚git reset与git reserve的区别

### 2.4.2 相关用法

**已修改，未暂存**

如果我们只是在编辑器里修改了文件，但还没有执行`git add .`，这时候我们的文件还在`工作区`，并没有进入`暂存区`，我们可以用：

```erlang
git checkout .
```

或者

```pgsql
git reset --hard
```

来进行撤销操作。

![clipboard.png](C:\Users\Administrator\Desktop\LearnGit\Pic\撤销操作-1.png)

可以看到，在执行完`git checkout .`之后，修改已被撤销，`git diff`没有任何内容了。

> **一对反义词**
> `git add .`的反义词是`git checkout .`。做完修改之后，如果你想向前走一步，让修改进入`暂存区`，就执行`git add .`，如果你想向后退一步，撤销刚才的修改，就执行`git checkout .`。

**已暂存，未提交**

你已经执行了`git add .`，但还没有执行`git commit -m "comment"`。这时候你意识到了错误，想要撤销，你可以执行：

```shell
git reset
git checkout .
```

或者

```shell
git reset --hard
```

`git reset`只是把修改退回到了`git add .`之前的状态，也就是说文件本身还处于`已修改未暂存`状态，你如果想退回`未修改`状态，还需要执行`git checkout .`。

或许你已经注意到了，以上两个步骤都可以用同一个命令`git reset --hard`来完成。是的，就是这个强大的命令，可以一步到位地把你的修改完全恢复到`未修改`的状态。

**已提交，未推送**

你的手太快，你既执行了`git add .`，又执行了`git commit`，这时候你的代码已经进入了你的`本地仓库`，然而你后悔了，怎么办？不要着急，还有办法。

```shell
git reset --hard origin/master
```

还是这个`git reset --hard`命令，只不过这次多了一个参数`origin/master`，正如我们上面讲过的，`origin/master`代表`远程仓库`，既然你已经污染了你的`本地仓库`，那么就从`远程仓库`把代码取回来吧。

已推送

很不幸，你的手实在是太快了，你既`git add`了，又`git commit`了，并且还`git push`了，这时你的代码已经进入`远程仓库`。如果你想恢复的话，还好，由于你的`本地仓库`和`远程仓库`是等价的，你只需要先恢复`本地仓库`，再强制`push`到`远程仓库`就好了：

```shell
git reset --hard HEAD^
git push -f
```

![撤销操作-2](C:\Users\Administrator\Desktop\LearnGit\Pic\撤销操作-2.png)

### 2.4.3 应用案例

参考**2.4.2**相关用法。

## 2.5 撤销变更-revert

### 2.5.1 简介

revert 可以取消指定的某次提交内容。当讨论 revert 时，需要分两种情况，因为 commit 分为两种：⼀种是常规的`commit`，也就是使⽤提交的`commit`；另⼀种是 `merge commit`，在使⽤`git commit`合并两个分⽀之后，你将会得到⼀个新的`merge commit`。

使用`git show`命令可以查看两种`commit`的不同。（具体可以在练手小项目中，尝试查看下）。

### 2.5.2 相关用法

**revert 之后重新上线**  (参考一篇比较好理解的文章)

- 假设狗蛋在⾃⼰分⽀ goudan/a-cool-feature 上开发了⼀个功能，并合并到了 master 上，之后 master 上⼜提交了⼀个修改 h，这时提交历史如下：

  a -> b -> c -> f -- g -> h (master)

  ​           \      /

  ​            d -> e   (goudan/a-cool-feature)

- 突然，⼤家发现狗蛋的分⽀存在严重的 bug，需要 revert 掉，于是⼤家把 g 这个 merge commit revert 掉了，记为 G，如下：

  a -> b -> c -> f -- g -> h -> G (master)

  ​           \      /

  ​            d -> e   (goudan/a-cool-feature)

- 然后狗蛋回到⾃⼰的分⽀进⾏ bugfix，修好之后想重新合并到 master，直觉上只需要再 merge 到 master 即可（或者使⽤ cherry-pick），像这样：

  a -> b -> c -> f -- g -> h -> G -> i (master)

  ​           \      /               /

  ​            d -> e -> j -> k ----    (goudan/a-cool-feature)

- i 是新的 merge commit。但需要注意的是，这不能得到我们期望的结果。因为 d 和 e 两个提交曾经被丢弃过，如此合并到 master 的代码，并不会重新包含 d 和 e 两个提交的内容，相当于只有 goudan/a-cool-feature 上的新 commit 被合并了进来，⽽ goudan/a-cool-feature 分⽀之前的内容，依然是被 revert 掉了。所以，如果想恢复整个 goudan/a-cool-feature 所做的修改，应该先把 G revert 掉：

  a -> b -> c -> f -- g -> h -> G -> G' -> i (master)

  ​           \      /                     /

  ​            d -> e -> j -> k ----------    (goudan/a-cool-feature)

- 其中 G' 是对 G 的 revert 操作⽣成的 commit，把之前撤销合并时丢弃的代码恢复了回来，然后再 merge 狗蛋的分⽀，把解决 bug 写的新代码合并到 master 分⽀。



# 3 移动提交记录

## 3.1 git cherry-pick-做题常用

### 3.1.1 简介

对于多分支的代码库，将代码从一个分支转移到另一个分支是常见需求。

这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（`git merge`）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。

cherry-pick命令"复制"一个提交节点并在当前分支做一次完全一样的新提交。

### 3.1.2 相关用法

- **转移单个提交**

`git cherry-pick`命令的作用，就是将指定的提交（commit）应用于其他分支。

```shell
$ git cherry-pick <commitHash>
```

> 上面命令就会将指定的提交`commitHash`，应用于当前分支。这会在当前分支产生一个新的提交，当然它们的哈希值会不一样。

举例来说，代码仓库有`master`和`feature`两个分支。

> ```bash
>     a - b - c - d   Master
>          \
>            e - f - g Feature
> ```

现在将提交`f`应用到`master`分支。

> ```bash
> # 切换到 master 分支
> $ git checkout master
> 
> # Cherry pick 操作
> $ git cherry-pick f
> ```

上面的操作完成以后，代码库就变成了下面的样子。

> ```bash
>     a - b - c - d - f   Master
>          \
>            e - f - g Feature
> ```

从上面可以看到，`master`分支的末尾增加了一个提交`f`。

`git cherry-pick`命令的参数，不一定是提交的哈希值，分支名也是可以的，表示转移该分支的最新提交。

> ```bash
> $ git cherry-pick feature
> ```

上面代码表示将`feature`分支的最近一次提交，转移到当前分支。

- **转移多个提交**

Cherry pick 支持一次转移多个提交。

> ```bash
> $ git cherry-pick <HashA> <HashB>
> ```

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。

- **配置项**

**`git cherry-pick`命令的常用配置项如下。**

（1）`-e`，`--edit`

打开外部编辑器，编辑提交信息。

（2）`-n`，`--no-commit`

只更新工作区和暂存区，不产生新的提交。

（3）`-x`

在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。

（4）`-s`，`--signoff`

在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。

（5）`-m parent-number`，`--mainline parent-number`

如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。

`-m`配置项告诉 Git，应该采用哪个分支的变动。它的参数`parent-number`是一个从`1`开始的整数，代表原始提交的父分支编号。

> ```bash
> $ git cherry-pick -m 1 <commitHash>
> ```

上面命令表示，Cherry pick 采用提交`commitHash`来自编号1的父分支的变动。

- **代码冲突**

如果操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作。

（1）`--continue`

用户解决代码冲突后，第一步将修改的文件重新加入暂存区（`git add .`），第二步使用下面的命令，让 Cherry pick 过程继续执行。

> ```bash
> $ git cherry-pick --continue
> ```

（2）`--abort`

发生代码冲突后，放弃合并，回到操作前的样子。

（3）`--quit`

发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子。



## 3.2 交互式git rebase-做题常用

### 3.2.1 简介

在`rebase`命令后增加了`-i`这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。

在实际使用时，所谓的 UI 窗口一般会在文本编辑器 —— 如 Vim —— 中打开一个文件。 

### 3.2.2 相关应用

当你运行 `git rebase -i` 时，你会进入一个编辑器会话，其中列出了所有正在被变基的提交，以及可以对其执行的操作的多个选项。默认的选择是选择（`Pick`）。

- `Pick`：会在你的历史记录中保留该提交。
- `Reword`：允许你修改提交信息，可能是修复一个错别字或添加其它注释。
- `Edit`：允许你在重放分支的过程中对提交进行修改。
- `Squash`：可以将多个提交合并为一个。
- 你可以通过在文件中移动来重新排序提交。

当你完成后，只需保存最终结果，变基操作就会执行。在你选择修改提交的每个阶段（无论是用 `reword`、`edit`、`squash` 还是发生冲突时），变基都会停止，并允许你在继续提交之前进行适当的修改。

### 3.2.3  应用案例

```shell
# 我们初始化一个项目
git init


## 制造一些提交
touch base.txt
git add .
git commit -m "add base"

touch 1.txt
git add .
git commit -m "add 1"

touch 2.txt
git add .
git commit -m "add 2"

touch 3.txt
git add .
git commit -m "add 3"

touch 4.txt
git add .
git commit -m "add 4"

touch 5.txt
git add .
git commit -m "add 5"


## 查看现在的提交
git log
commit a75ed742838ebc1ef1073502623478f73e1ec21f
Author: 
Date:   Wed Mar 4 10:02:51 2020 +0800

    add 5

commit 8b485bb4768b2abf8f6400dcba069f1a650ed5ec
Author: 
Date:   Wed Mar 4 09:59:27 2020 +0800

    add 4

commit 63ce9fb010da550c668aca66758c45fbfad46e2b
Author:
Date:   Wed Mar 4 09:59:04 2020 +0800

    add 3

commit 9cd34c4d42f52cfb40026dae613c8ad29d7cbc66
Author: 
Date:   Wed Mar 4 09:58:45 2020 +0800

    add 2

commit 77bd0eb1a97e1676367ea236c1c47c155eaa8430
Author: 
Date:   Wed Mar 4 09:58:23 2020 +0800

    add 1

```

> 现在我们已经有了一些提交了，接下来开始练习：

1. 告诉git 我要改变倒数第2次后的提交

```shell
pick 8b485bb add 4
pick a75ed74 add 5

# Rebase 63ce9fb..a75ed74 onto 63ce9fb (2 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
~
~
~
```

> 解释：我们刚刚执行的命令中 `HEAD~2`代表选择离HEAD最近的两条提交
>
> 下面注释的是提示，我们不需要管，只要专注前两行就ok

2. 把 第一行 和 第二行 交换顺序

```shell
pick a75ed74 add 5
pick 8b485bb add 4
```

> 接着 `Esc`,`:wq` 保存退出.
>
> `git log`查看，4 和 5 的顺序改变了

我们来删除 add 4 的那条提交

```shell
git rebase -i HEAD~2
```

出现如下:

```shell
pick a75ed74 add 5
pick 8b485bb add 4

# Rebase 575fd8b..bb2a77d onto 575fd8b (1 command(s))
# .....略
```

> 接着 `Esc`,`:wq` 保存退出
>
> `git log`查看

# 4 杂项

**说明：**由于杂项中用到的命令都是前文介绍过的，因此本节主要分析https://learngitbranching.js.org/中的解题思路。

## 4.1 只取一个提交记录

### 解题思路

本题可以分别使用`cherry-pick`和`rebase -i`命令完成。

- **解法1：**其中使用cherry-pick命令时，需要切换至main分支，然后提取题目需求的记录。

```shell
git checkout main
git cherry-pick bugFix
```

- **解法2：**直接在当前节点执行`rebase -i`命令，注意搞清楚需要在第几个节点终止！然后将main分支移动至当前位置。

```SHELL
git rebase -i HEAD~3

git branch -f main HEAD
```

## 4.2 提交的技巧 #1

### 解题思路

- 先用 `git rebase -i` 将提交重新排序，然后把我们想要修改的提交记录挪到最前
- 然后用 `git commit --amend` 来进行一些小修改
- 接着再用 `git rebase -i` 来将他们调回原来的顺序
- 最后我们把 main 移到修改的最前端（用你自己喜欢的方法），就大功告成啦！

```shell
git rebase -i HEAD~2
git commit --amend    # 可用git rebase -i HEAD~1 替换
git rebase -i HEAD~2
git branch -f main caption
```

## 4.3 提交的技巧 #2

### 解题思路

- 先切换至main分支
- 使用`cherry-pick`命令提取newImage分支的提交
- 将main分支切换至c1
- 提取`c2'` 和`c3`节点

```shell
git checkout main
git cherry-pick newImage
git branch -f main c1
git cherry-pick c2' c3
```

## 4.4 git tag



# 5  高级话题

该章相当于对前文所学知识的总结和运用，因此不在赘述。

# 参考链接：

[Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)

**git rebase详解：https://blog.csdn.net/weixin_42310154/article/details/119004977**

**git checkout详解：https://blog.csdn.net/raoxiaoya/article/details/111321583**

**菜鸟教程：https://www.runoob.com/git/git-tutorial.html**

**Learn Git Branching 总结：https://www.jianshu.com/p/6e94b5592c40**

**保姆级 Git 入门教程：https://baijiahao.baidu.com/s?id=1700195384333056004&wfr=spider&for=pc**

**git指令学习：https://www.cnblogs.com/qxlzzj/p/15037178.html#1%E5%BC%80%E5%8F%91%E6%9C%89%E6%B2%A1%E6%9C%89%E5%8F%AF%E8%83%BD%E6%98%AF%E4%B8%80%E4%B8%AA%E4%BA%BA**

**git撤销merge,彻底学会gitrevert的⽤法：https://wenku.baidu.com/view/b2050a36f28583d049649b6648d7c1c708a10b0b.html**

**git cherry-pick 教程：https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html**

**git rebase -i 命令详解：(https://blog.csdn.net/the_power/article/details/104651772/**

**git tag创建、远程推送、回退以及强推：https://blog.csdn.net/QH_JAVA/article/details/77979622**

