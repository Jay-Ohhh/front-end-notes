### Git

#### git常用命令

##### clone 克隆仓库

```sh
git clone [url]
```

##### add 添加到暂存区

```sh
git add . 将当前目录下的所有文件添加到暂存区
git add 文件/文件夹相对路径  将指定文件/文件夹添加到暂存区
```

- If you are located directly at the *working directory*, then `git add -A` and `git add .` work without the difference.
- If you are in any subdirectory of the *working directory*, `git add -A` will add all files from the entire *working directory*, and `git add .` will add files from your *current directory*.

##### commit 将暂存区内容添加到本地仓库中

```sh
git commit -m [message]
```

提交暂存区的指定文件到仓库区：

```sh
git commit [file1] [file2] ... -m [message]
```

**-a** Tell the command to automatically stage files that have been modified and deleted, but new files you have not told git about are not affected.

```sh
git commit -a
```

如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。命令格式如下：

```
git commit -a
```

**如果新建了文件，还是需要** 

```sh
git add .
```

使用 **git commit -am** 可以省略使用git add 命令将已跟踪文件（不包括新建文件）放到暂存区的功能。

我们先修改 hello.php 文件为以下内容：

```php
<?php
echo '菜鸟教程：www.runoob.com';
echo '菜鸟教程：www.runoob.com';
?>
```

再执行以下命令：

```sh
git commit -am '修改 hello.php 文件'
[master 71ee2cb] 修改 hello.php 文件
 1 file changed, 1 insertion(+)
```

##### push 将本地的分支版本上传到远程仓库

```sh
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```sh
git push <远程主机名> <本地分支名>
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

> 会把之前的push记录覆盖掉

```sh
git push --force origin master
```

某分支第一次push时可以使用 **-u** 选项：

如果当前分支与多个主机存在追踪关系，则可以使用 **- u**选项指定一个默认主机，这样后面可以直接用git push。

执行 **git push -u origin master** 相当于执行了 **git push origin master** 和 **git branch --set-upstream master origin/master**

```sh
git push -u origin 远程分支名 // 同时会创建远程新分支
```

```sh
git push -u origin master 
```

上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机。

##### checkout 新建分支并切换到该分支

```sh
git checkout -b 新分支名 [已存在的分支，可选，默认为当前分支]
```

##### 基于远程分支创建本地分支

```bash
git fetch origin 远程分支名
git checkout -b 新的分支名 远程仓库别名/远程分支名
```



##### checkout 切换到指定分支

```sh
git checkout 指定分支名
```

##### checkout --orphan 清空commit历史

它会基于当前所在分支新建一个赤裸裸的分支，没有任何的提交历史。

```sh
# 新分支
git checkout --orphan branch_name
# 添加文件
git add -A
# 提交commit
git commit -am "Init commit"
# 删除原来分支
git branch -D master
# 重命名分支
git branch -m master
# 提交到远程
git push -f origin master
```

##### merge 合并指定分支到当前分支

在 Git 中整合来自不同分支的修改主要有两种方法：`merge` 以及 `rebase`。 

先切换到当前分支，然后

```sh
git merge [指定分支名]
```

合并后可以直接进行push，不需git add . 和 git commit

**git merge --no-ff**

https://www.cnblogs.com/phpper/p/8034480.html

##### rebase 变基

优点

- rebase最大的好处是你的项目历史会非常整洁
- rebase 导致最后的项目历史呈现出完美的线性——你可以从项目终点到起点浏览而不需要任何的 fork。这让你更容易使用 git log、git bisect 和 gitk 来查看项目历史

缺点

- 安全性，`如果你在公共分支上使用rebase`，重写项目历史可能会给你的协作工作流带来灾难性的影响。**不要在master等公共分支使用rebase**。
  
  

https://www.jianshu.com/p/4a8f4af4e803

###### 同分支内合并多个commit为一个新commit使用：

```sh
git rebase -i [startpoint] [endpoint]
```

其中`-i`的意思是`--interactive`，即弹出交互式的界面让用户编辑完成合并操作，`[startpoint]`  `[endpoint]`则指定了一个编辑区间，如果不指定`[endpoint]`，则该区间的终点默认是当前分支`HEAD`所指向的`commit`(注：该区间指定的是一个前开后开的区间)。

例如 把最后三个提交合并为一个提交：

```sh
git rebase -i HEAD~3 // 此处省略了[endpoint]
```

执行后会弹出一个界面：

```
pick：保留该commit（缩写:p）
reword：保留该commit，但我需要修改该commit的注释（缩写:r）
edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
squash：将该commit和区间第一个commit合并（缩写:s）
fixup：将该commit和区间第一个commit合并，但我不要保留该提交的注释信息（缩写:f）
exec：执行shell命令（缩写:x）
drop：我要丢弃该commit（缩写:d）
```

点击**start**开始变基

点击**abort**，退出rebase，处于未rebase之前的状态。

有冲突就解决冲突，然后

```
git add .
git rebase --contine
```

直至解决所有冲突。

###### 将dev分支多个commit合并到主分支，并形成一个新commit

```sh
git rebase [startpoint]  [endpoint] --onto <newbase>
```

其中，`[startpoint]` `[endpoint]`仍然和上一个命令一样指定了一个编辑区间(前开后开)，`--onto`的意思是以谁为基点。

把dev分支最后三个提交合并到主分支，并形成一个新commit：

切换到dev分支，然后

```sh
git rebase HEAD~3 --onto master
```

当前HEAD处于游离状态，master分支是没有任何变化，需要：

```sh
git checkout master
git merge dev
```

###### 代码冲突

rebase 和 merge的另一个区别是rebase 的冲突是一个一个解决，如果有十个冲突，先解决第一个，然后用命令

```sh
git add -u  // 提交所有被删除和修改的文件到数据暂存区
// 或使用 git add .
git rebase --continue // 会弹窗让你修改提交信息，默认是原始的提交信息
```

在解决git rebase的冲突后不需要commit，如果你愿意，也可以commit。最后push之前，可以先看看 git status。

继续后才会出现第二个冲突，直到所有冲突解决完，而merge 是所有的冲突都会显示出来需要一起解决。
另外如果rebase过程中，你想中途退出，恢复rebase前的代码则可以用命令

```sh
git rebase --abort
```

所以rebase的工作流就是

```sh
git rebase 
while(存在冲突) {
    git status
    找到当前冲突文件，编辑解决冲突
    git add -u
    git rebase --continue
    if( git rebase --abort )
        break; 
}
```

###### 实际用法

因此整个流程可以为：

```sh
git checkout dev
// 多个commit之后
git rebase -i HEAD~3 // 合并提交
git rebase - master //将dev的提交移至（变基）到master---->解决冲突--->git add -u--->git rebase --continue
git checkout master
git merge dev
```

###### merge和rebase区别

> merge命令不会保留merge的分支的commit，rebase会保留所有的commit（除非使用 rebase 修改提交）

**merge**

![merge](http://jay_ohhh.gitee.io/imagehosting/Git/merge.jpg)

**rebase**

![rebase](http://jay_ohhh.gitee.io/imagehosting/Git/rebase.jpg)

##### branch 删除本地分支

```sh
git branch -D 分支名 // 在其它分支才能删除该分支
```

##### branch 删除远程分支

```sh
git push origin --delete 分支名
```

##### branch 修改本地分支名

```sh
git branch -m oldName newName
```

##### branch 修改远程分支名

```sh
git push --delete origin 远程分支名 // 删除远程分支名
git push -u origin 新分支名
```

##### fetch 从远程获取代码库

`git fetch` 是 Git 中用来从远程仓库下载代码更新并将其同步到本地仓库的命令，它只会将远程仓库中最新的代码更新下载到本地，并不会将这些更新自动合并到当前分支中。

```shell
git fetch <远程主机名> <远程分支名>:<本地分支名> // 注意空格，本地分支名选项是可选项
```

`<远程主机名>` 是您想要从中下载代码的远程仓库的名称，例如 "origin" 或 "upstream"。`<远程分支名>` 是您想要下载的远程分支的名称，例如 "master" 或 "dev"。`<本地分支名>` 是您想要将远程分支下载到的本地分支的名称，例如 "local-master" 或 "local-dev"。

使用这个语法时，Git 将会将远程分支下载到本地，并使用 `<本地分支名>` 作为本地分支名称。例如，如果您运行以下命令：

```bash
git fetch origin master:local-master
```



下面是一些 `git fetch` 命令的使用例子：

1. `git fetch`: 这个命令将从远程仓库下载当前分支代码更新（前提：当前分支已指定远程分支，`git branch --set-upstream master origin/master`），但不会将这些更新自动合并到当前分支中。如果您想查看远程分支的状态，可以使用 `git branch -r` 命令。
2. `git fetch origin`: 这个命令将从名为 "origin" 的远程仓库下载代码更新。这是 Git 默认使用的远程仓库名称，通常用于与 GitHub 上的代码仓库进行交互。
3. `git fetch upstream`: 这个命令将从名为 "upstream" 的远程仓库下载代码更新。这个命令通常用于与上游代码库进行交互，例如，如果您要将您的分支更新为上游分支的最新版本。
4. `git fetch --all`: 这个命令将从所有已配置的远程仓库下载代码更新。这是一个很有用的命令，因为它可以帮助您保持所有远程仓库的最新状态。



##### pull 从远程获取代码并合并本地的版本

取回远程主机的远程分支，与本地分支合并

```sh
git pull <远程主机名> <远程分支名>:<本地分支名> // 注意空格，本地分支名选项是可选项，默认为本地当前分支
```

git pull = git fetch + git merge

**git pull --rebase** = git fetch + git rebase

常见问题

- refusing to merge unrelated histories，通常出现于两个远程仓库拉pull到本地仓库的情况
  
  出现这个问题的最主要原因还是在于本地仓库和远程仓库实际上是独立的两个仓库。
  
  解决：在git pull 或 git merge 命令后加上--allow-unrelated-histories

##### 查看本地工作区、暂存区中文件的修改状态

```sh
git status
```

##### 查看提交历史

```sh
git log
```

退出 git log：英文状态下按Q

```sh
git log --graph // 分支合并图
```

##### 查看分支

查看本地的所有分支：git branch

查看远程仓库所有分支：git branch -r

查看本地和远程仓库的所有分支：git branch -a

##### Tag

tag 对应某次commit, 是一个点，是不可移动的。  
branch 对应一系列commit，是很多点连成的一根线，有一个HEAD 指针，是可以依靠 HEAD 指针移动的。  
所以，两者的区别决定了使用方式，改动代码用 branch ,不改动只查看用 tag。  
tag 和 branch 的相互配合使用，有时候起到非常方便的效果，例如：已经发布了 v1.0 v2.0 v3.0 三个版本，这个时候，我突然想不改现有代码的前提下，在 v2.0 的基础上加个新功能，作为 v4.0 发布。就可以检出 v2.0 的代码作为一个 branch ，然后作为开发分支。

###### tag_name

```
v<major>.<minor>.<patch>
```

- major：重大修改或向后不兼容
- minor: 以向后兼容的方式添加新功能
- patch: 以向后兼容的方式修复bug

###### tag类型

一共有两种`tag`类型：

- 附注标签（Annonated）
- 轻量标签（Lightweight）

**附注标签**

附注标签存储一个额外的信息，比如作者、发行说明、tag 信息存储为Git仓库中完整的数据，这些数据对于一个公开的项目是非常重要的

`-a`表示该tag是附注标签

```sh
git tag -a [tag_name] -m [msg]
git tag -a [tag_name] [commit_id] -m [msg]
```

**轻量标签**

轻量标签时最简单的打tag的方式，它只存储tag name和关联的commit的hash值，不包含额外的信息，就类似于一个书签

```sh
git tag v2.1-lw
```

###### 显示标签

```sh
git tag
git tag --list
git show [tag_name]
```

使用`-l`或者`--list`选项利用正则表达式进行过滤

```
git tag -l "1.0*"
```

###### 创建远程标签

```sh
git push [主机名] [branch] [tag_name]

// 推送全部tags
git push [主机名] [branch] --tags
```

###### 删除本地标签

```shell
git tag -d [tag_name]
```

###### 删除远程标签

```sh
git tag push [主机名] [branch] :refs/tags/[tag_name]

git push [主机名] [branch] --delete <tag_name>
```

###### 拉取远程标签

```shell
git pull [主机名] [branch] --tags
```

###### 检出tag

```shell
git checkout -b <new_branch> <tag_name>
```

##### releases

**tag是 Git 中的概念，而 releases 则是 Github、码云等源码托管商所提供的更高层的概念。Git 本身是没有 releases 这个概念，只有 tag。两者之间的关系则是，release 基于 tag，为 tag 添加更丰富的信息，一般是编译好的文件。**

##### reset 回退版本

有时候，进行了错误的提交，但是还没有push到远程分支，想要撤销本次提交，可以使用命令：

```sh
git reset [--soft | --mixed | --hard] [HEAD]
```

```sh
git reset [--soft | --mixed | --hard] [commit-id]
```

git reset 分为三种：软 --soft，中 ---mixed，硬 --hard 对应着三种回滚的程度，程度越硬，回滚的越“狠”

1. --soft 已 add，但尚未 commit （保留工作目录、暂存区）
2. --mixed（git reset 的默认设定，可以省略不写），文件会回退到未 add（未暂存）的状态 （保留工作目录）
3. --hard 硬核，彻底，会彻底返回到回退前的版本状态，了无痕迹 （全部删除）

**commit-id** 可以通过 git log 查看本地的所有提交

> commit-id 就是 commit 的哈希值

##### revert 反做版本

https://blog.csdn.net/yxlshk/article/details/79944535

将现有的提交还原，恢复提交的内容，并生成一条还原记录。

**适用场景：** 如果我们想撤销之前的某一版本，但是又想保留该目标版本后面的版本，记录下这整个版本变动流程，就可以用这种方法。

当讨论 revert 时，需要分两种情况，因为 commit 分为两种：

- 一种是常规的 commit，也就是使用 git commit 提交的 commit；
- 另一种是 merge commit，在使用 git merge 合并两个分支之后，你将会得到一个新的 merge commit

merge commit 和普通 commit 的不同之处在于 merge commit 包含两个 parent commit，代表该 merge commit 是从哪两个 commit 合并过来的。


**应用场景**

应用场景：有一天测试突然跟你说，你开发上线的功能有问题，需要马上撤回，否则会影响到系统使用。这时可能会想到用 reset 回退，可是你看了看分支上最新的提交还有其他同事的代码，用 reset 会把这部分代码也撤回了。由于情况紧急，又想不到好方法，还是任性的使用 reset，然后再让同事把他的代码合一遍（同事听到想打人），于是你的技术形象在同事眼里一落千丈。



![clipboard.png](https://img-blog.csdnimg.cn/img_convert/d7b9511fe5093a0de33e9cf65360eb34.png)

在上图所示的红框中有一个 merge commit，使用 git show 命令可以查看 commit 的详细信息

```sh
➜  git show bd86846
commit bd868465569400a6b9408050643e5949e8f2b8f5
Merge: ba25a9d 1c7036f
```

这代表该 merge commit 是从 ba25a9d 和 1c7036f 两个 commit 合并过来的。

而常规的 commit 则没有 Merge 行

```sh
➜  git show 3e853bd
commit 3e853bdcb2d8ce45be87d4f902c0ff6ad00f240a
```



###### revert 常规 commit

使用 `git revert <commit id>` 即可，git 会生成一个新的 commit，将指定的 commit 内容从当前分支上撤除。

```sh
git revert <commit id>
```

###### revert 合并 commit

evert merge commit 有一些不同，这时需要添加 -m 选项以代表这次 revert 的是一个 merge commit

但如果直接使用 git revert <commit id>，git 也不知道到底要撤除哪一条分支上的内容，这时需要指定一个 parent number 标识出"主线"，主线的内容将会保留，而另一条分支的内容将被 revert。

如上面的例子中，从 git show 命令的结果中可以看到，merge commit 的 parent 分别为 ba25a9d 和 1c7036f，其中 ba25a9d 代表 master 分支（从图中可以看出），1c7036f 代表 will-be-revert 分支。需要注意的是 -m 选项接收的参数是一个数，此选项指定主线的父编号（从1开始），数字取值为 1 和 2，也就是 Merge 行里面列出来的第一个还是第二个。

-m 后面要跟一个 parent number 标识出"主线"，一般使用 1 保留主分支代码，操作如下：

```sh
git revert -m 1 bd86846
```



###### revert 合并commit后，再次合并分支会失效

假设tom在自己分支 tom/feature 上开发了一个功能，并合并到了 master 上，之后 master 上又提交了一个修改 h，这时提交历史如下：

```
a -> b -> c -> f -- g -> h (master)
           \      /
            d -> e   (tom/feature)
```

突然，大家发现tom的分支存在严重的 bug，需要 revert 掉，于是大家把 g 这个 merge commit revert 掉了，记为 G，如下：

```
a -> b -> c -> f -- g -> h -> G (master)
           \      /
            d -> e   (tom/feature)
```

然后tom回到自己的分支进行 bugfix，修好之后想重新合并到 master，直觉上只需要再 merge 到 master 即可，像这样：

```
a -> b -> c -> f -- g -> h -> G -> i (master)
           \      /               /
            d -> e -> j -> k ----    (tom/feature)
```

i 是新的 merge commit。但需要注意的是，这 不能 得到我们期望的结果。因为 d 和 e 两个提交曾经被丢弃过，如此合并到 master 的代码，并不会重新包含 d 和 e 两个提交的内容，相当于只有 tom/feature 上的新 commit （j、k）被合并了进来，而 tom/feature 分支之前的内容，依然是被 revert 掉了。
所以，如果想恢复整个 tom/feature 所做的修改，应该先把 G revert 掉：

```
a -> b -> c -> f -- g -> h -> G -> G' -> i (master)
           \      /                     /
            d -> e -> j -> k ----------    (tom/feature)
```

其中 G’ 是对 G 的 revert 操作生成的 commit，把之前撤销合并时丢弃的代码恢复了回来，然后再把tom的分支merge到master分支上，把解决 bug 写的新代码合并到 master 分支。


##### cherry-pick

将某一个分支的单个或多个提交，并作为一个新的提交引入到当前分支上。

###### 复制单个提交

```sh
git cherry-pick <branchName>
git cherry-pick <commitHash>
```

上面命令就会将指定的提交commitHash，应用于当前分支。

举例来说，代码仓库有`master`和`feature`两个分支。

```
 a - b - c - d   Master
      \
        e - f - g Feature
```

现在将提交`f`应用到`master`分支。

```bash
# 切换到 master 分支
git checkout master

# Cherry pick 操作
git cherry-pick f // f是指Feature分支的f节点的commitHash，即commit-id
```

上面的操作完成以后，代码库就变成了下面的样子。

```
a - b - c - d - f   Master
      \
        e - f - g Feature
```

###### 复制多个提交

```bash
git cherry-pick <HashA> <HashB>
```

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。

如果想要转移一系列的连续提交，可以使用下面的简便语法。

```bash
git cherry-pick A..B // 前开后闭的区间
```

上面的命令可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。

注意，使用上面的命令，提交 A 将不会包含在 Cherry pick 中。如果要包含提交 A，可以使用下面的语法。

```bash
git cherry-pick A^..B 
```

###### 配置项

`git cherry-pick`命令的常用配置项如下。

**（1）`-e`，`--edit`**

打开外部编辑器，编辑提交信息。

**（2）`-n`，`--no-commit`**

只更新工作区和暂存区，不产生新的提交。

**（3）`-x`**

在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。

**（4）`-s`，`--signoff`**

在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。

**（5）`-m parent-number`，`--mainline parent-number`**

如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。

`-m`配置项告诉 Git，应该采用哪个分支的变动。它的参数`parent-number`是一个从`1`开始的整数，代表原始提交的父分支编号。

```bash
git cherry-pick -m 1 <commitHash>
```

上面命令表示，Cherry pick 采用提交`commitHash`来自编号1的父分支的变动。

一般来说，1号父分支是接受变动的分支（the branch being merged into），2号父分支是作为变动来源的分支（the branch being merged from）。 

###### 代码冲突

如果操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作。

**（1）`--continue`**

用户解决代码冲突后，第一步将修改的文件重新加入暂存区（`git add .`），第二步使用下面的命令，让 Cherry pick 过程继续执行。

```
git cherry-pick --continue
```

**（2）`--abort`**

发生代码冲突后，放弃合并，回到操作前的样子。

**（3）`--quit`**

发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子。即保留已经`cherry-pick`成功的 commit，并退出`cherry-pick`流程。



##### stash 藏匿未commit的代码

官方解释：当您想记录工作目录和索引的当前状态，但又想返回一个干净的工作目录时，请使用git stash。该命令将保存本地修改，并恢复工作目录以匹配头部提交。

stash 命令能够将还未 commit 的代码存起来，让你的工作目录变得干净。

**应用场景**

我猜你心里一定在想：为什么要变干净？

应用场景：某一天你正在 feature 分支开发新需求，突然产品经理跑过来说线上有bug，必须马上修复。而此时你的功能开发到一半，于是你急忙想切到 master 分支，然后你就会看到以下报错：

![图片](https://mmbiz.qpic.cn/mmbiz/pfCCZhlbMQQwiaxAEIbdrMvBwOGz381Mt5ywg0EvhAy2QEusE1YXBth0wSBiabZJ5soiclTkWs7YUVtLRYdvzSIyw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

因为当前有文件更改了，需要提交commit保持工作区干净才能切分支。由于情况紧急，你只有急忙 commit 上去，commit 信息也随便写了个“暂存代码”，于是该分支提交记录就留了一条黑历史…(真人真事，看过这种提交)。

**命令使用**

如果你学会 stash，就不用那么狼狈了。你只需要：

```sh
git stash
```

就这么简单，代码就被存起来了。

当你修复完线上问题，切回 feature 分支，想恢复代码也只需要：

```sh
git stash apply
```

stash是本地的，不会通过 `git push` 命令上传到 git server 上。

**相关命令**

```sh
# 保存当前未commit的代码
git stash

# 保存当前未commit的代码并添加备注
git stash save "备注的内容"

# 列出stash的所有记录
git stash list

# 删除stash的所有记录
git stash clear

# 应用最近一次的stash
git stash apply

# 应用最近一次的stash，随后删除该记录
git stash pop

# 删除最近的一次stash
git stash drop
```

当有多条 stash，可以指定操作stash，首先使用stash list 列出所有记录：

```sh
$ git stash list
stash@{0}: WIP on ...
stash@{1}: WIP on ...
stash@{2}: On ...
```

应用第二条记录：

```sh
$ git stash apply stash@{1}
```

pop，drop 同理。



##### reflog

此命令管理记录的信息。

如果说`reset --soft`是后悔药，那 **reflog** 就是强力后悔药。它记录了所有的操作记录，便于错误操作后找回记录。

**应用场景**

应用场景：某天你眼花，发现自己在其他人分支提交了代码还推到远程分支，这时因为分支只有你的最新提交，就想着使用`reset --hard`，结果紧张不小心记错了 commitHash，reset 过头，把同事的 commit 搞没了。没办法，`reset --hard`是强制回退的，找不到 commitHash 了，只能让同事从本地分支再推一次（同事瞬间拳头就硬了，怎么又是你）。于是，你的技术形象又一落千丈。

**命令使用**

```sh
git log
```

```
commit 333 (HEAD->master)
  update(c):自己的错误提交
commit 222
  update(b):b
commit 111
	update(a):a
```

分支记录如上，本来想 reset --hard c，却 reset 了 b。

```sh
git reset --hard 222

git log
```

```
commit 111 (HEAD->master)
	update(a):a
```

误操作 reset 过头，b 没了，最新的只剩下 a。

![图片](https://mmbiz.qpic.cn/mmbiz/nDMNE6lrvW68Oia91x5oib039hSqIO3a9ibM3tjBobn741AtMA622S8PmQyEPwTAugmFCLeiaDMsib0YbyicrYJfRMXg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

`git reflog`查看历史记录，把错误提交的那次 commitHash 记下。

```sh
git reflog
111 (HEAD -> master) HEAD@{0}: reset: moving to 111
333 HEAD@{1}: commit: update(c): 自己的错误提交
```

```sh
git reset --hard 错误的commitHash 
```

再次 reset 回去，就会发现 b 回来了。



##### config

```shell
git config --global user.name = Jay # 全局配置
git config user.name = Jay # 项目配置
git config --list --global # 查看配置
```

项目 **.git/config**

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = http://10.10.10.24:8070/scm/pt/ef-oa.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[branch "sales-ng"]
	remote = origin
	merge = refs/heads/sales-ng
[user]
	name = Jay
	email = weijian.ou@ecoflow.com
```



#### 设置 git 短命令

###### 方式一

```sh
git config --global alias.ps push
```

###### 方式二

打开全局配置文件

```sh
vim ~/.gitconfig
```

写入内容

```
[alias] 
        co = checkout
        ps = push
        pl = pull
        mer = merge --no-ff
        cp = cherry-pick
```

###### 使用

```sh
# 等同于 git cherry-pick <commitHash>
git cp <commitHash>
```



#### 初始化仓库

在没有仓库（.git文件夹）的情况下

**方法1：**

在码云创建仓库后，

git clone 远程仓库地址

然后将克隆下来的.git文件夹放到项目的根目录，完成初始化

**方法2：**

在码云创建仓库后，项目根目录执行（有README.md）：

```sh
git add .
git commit -m "first commit"
git remote add origin 远程仓库地址
git push -u origin master
```

（无README.md）：

```sh
git add README.md
git commit -m "first commit"
git remote add origin 远程仓库地址
git push -u origin master
```

已有仓库（.git文件夹）的情况下：

```sh
git remote add origin 远程仓库地址
git push -u origin master
```

#### git修改本地和远程分支名称

1、修改本地分支名称

```sh
git branch -m 旧分支名 新分支名
```

2、删除远程旧分支

```sh
git push --delete origin 旧分支名 // 简写:  git push -d origin 旧分支名
```

3、将新分支名推上去

```sh
git push -u origin 新分支名
```

#### git删除中间某次提交

git log获取commit信息

```sh
commit 58211e7a5da5e74171e90d8b90b2f00881a48d3a
Author: test <test@36nu.com>
Date:   Fri Sep 22 20:55:38 2017 +0800

    add d.txt

commit 0fb295fe0e0276f0c81df61c4fd853b7a000bb5c
Author: test <test@36nu.com>
Date:   Fri Sep 22 20:32:45 2017 +0800

    add c.txt

commit 7753f40d892a8e0d14176a42f6e12ae0179a3210
Author: test <test@36nu.com>
Date:   Fri Sep 22 20:31:39 2017 +0800

    init
```

假如要删除备注为add c.txt commit为0fb295fe0e0276f0c81df61c4fd853b7a000bb5c的这次提交

1、首先找到此次提交之前的一次提交的commit 7753f40d892a8e0d14176a42f6e12ae0179a3210
2、执行如下命令
git rebase -i (commit-id)
commit-id 为要删除的commit的下一个commit号

```sh
git rebase -i 7753f40
```

3、编辑文件，将要删除的commit之前的单词改为drop ，然后按照提示保存退出

4、此已经删除了指定的commit，可以使用git log查看下

#### HEAD 说明

- HEAD 表示当前版本

- HEAD^、HEAD^1 第一次提交

- HEAD^^、HEAD^2 第二次提交

- 以此类推...

- HEAD~0 表示当前版本

- HEAD~1 前一次提交

- HEAD~2 前两次提交

- 以此类推...

```
B和C都是提A的父节点

G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A
A =      = A^0
B = A^   = A^1     = A~1
C = A^2  = A^2
D = A^^  = A^1^1   = A~2
E = B^2  = A^^2
F = B^3  = A^^3
G = A^^^ = A^1^1^1 = A~3
H = D^2  = B^^2    = A^^^2  = A~2^2
I = F^   = B^3^    = A^^3^
J = F^2  = B^3^2   = A^^3^2
```

> ^和~也可以用在 commit-id 后面

#### git只提交部分修改的文件（提交指定文件）

1、 git status -s 查看仓库状态

2、 git add src/components/文件名 添加需要提交的文件名（加路径--参考git status 打印出来的文件路径）

3、 git stash -u -k 忽略其他文件，把现修改的隐藏起来，这样提交的时候就不会提交未被add的文件

4、 git commit -m "哪里做了修改可写入..."

5、git pull 拉取合并

6、git push 推送到远程仓库

7、 git stash pop 恢复之前忽略的文件（非常重要的一步）



#### 入职公司拉取代码

1、先拉取代码

```sh
git clone 项目的地址
```

2、拉取你要开发的初始版本，比如develop分支的最新代码

> 也有可能是这样的顺序：
> 
> ```
> git clone 项目的地址
> git branch -a 查看所有分支，分支命名规范，远程主机名
> git log 查看commit规范
> git checkout -b wyn
> git push -u origin wyn  
> // or git push --set-upstream origin feat-wyn
> git pull origin develop
> git add .
> git commit -m ‘描述’
> git pull origin yyy （拉取要合并的分支代码，通常是dev分支）
> 有冲突的话跟同事协商解决冲突，然后 add commit push
> 
> git clone 项目的地址
> git branch -a 查看所有分支，分支命名规范，远程主机名
> git log 查看commit规范
> git checkout -b wyn
> git push -u origin wyn
> git pull origin develop
> git add .
> git commit -m ‘描述’
> git pull --rebase origin yyy （拉取要合并的分支代码，通常是dev分支）
> 有冲突的话跟同事协商解决冲突，然后
> git add .--->git rebase --continue
> 直至冲突全部解决
> git push
> ```

```sh
git pull origin develop
```

3、创建自己的分支，编写代码(我创建了wyn分支)

```sh
git checkout -b wyn
```

4、先编写代码，写好后进行提交（add，commit），再拉取合并同事代码

```sh
git add .
git commit -m ‘描述’
git pull origin yyy （拉取要合并的分支代码）
```

5、与同事协商，解决冲突

```sh
git push -u origin wyn
```

6、请求合并

```
gitlab -> merge request
github -> pull request
```

如果是直接在拉取下来的 devlop 分支上进行开发，则

1、先拉取代码

```sh
git clone 项目的地址
```

2、本地创建 devlop 分支

```sh
git checkout -b devlop 
```

3、拉取 devlop 分支代码

```sh
git pull origin devlop 
```

#### 覆盖分支

1.我想将test分支上的代码完全覆盖dev分支，首先切换到dev分支

```sh
git checkout dev
```

2.然后直接设置为远程的test分支上的代码

```sh
git reset --hard origin/test
```

3.执行上面的命令后dev分支上的代码就完全被test分支上的代码覆盖了，注意只是本地分支，这时候还需要将本地分支强行推到远程分支。

```sh
git push -f
```

#### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再add 、commit、merge。

解决冲突就是把Git合并失败的文件手动编辑为协商好的版本，再提交。需要与领导同事协商。



#### git远程操作

https://www.ruanyifeng.com/blog/2014/06/git_remote.html

有时我们push完之后，发现代码写错了，于是回退，回退修改完代码后，在push时会报错（non-fast-forward），因为回退，我们本地库HEAD指向的版本比远程库的要旧，这时可以使用 git push -f 。

##### 查看远程分支

```sh
git remote -v
```

##### 添加远程仓库地址

```sh
git remote add 远程仓库主机名 远程仓库地址
```

> 远程仓库主机名是自定义

##### 更换远程仓库地址

方法一：

直接在本地修改远程仓库地址即可：

```git
git remote set-url origin 远程仓库地址
```

方法二：

先删除，然后添加地址：

```git
git remote rm origin
git remote add origin 远程仓库地址
```

##### 删除远程仓库

```sh
git remote rm 远程仓库主机名
```

#### 配置公钥

https://blog.csdn.net/lqlqlq007/article/details/78983879

#### 修改仓库语言

项目仓库是根据根目录下的文件类型进行判断，哪种类型多，仓库就在仓库列表界面显示哪种语言类型

1. 添加文件 `.gitattributes`
   
   > 注意 .gitattributes 中内容的路径都是相对于.gitattributes的路径

2. 输入以下内容（重置识别类型）：

https://github.com/github/linguist/blob/master/docs/overrides.md

| Git attribute            | Defined in                                                                                           | Effect on file                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `linguist-detectable`    | [`languages.yml`](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)         | Included in stats, even if language's type is `data` or `prose` |
| `linguist-documentation` | [`documentation.yml`](https://github.com/github/linguist/blob/master/lib/linguist/documentation.yml) | Excluded from stats                                             |
| `linguist-generated`     | [`generated.rb`](https://github.com/github/linguist/blob/master/lib/linguist/generated.rb)           | Excluded from stats, hidden in diffs                            |
| `linguist-language`=name | [`languages.yml`](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)         | Highlighted and classified as name                              |
| `linguist-vendored`      | [`vendor.yml`](https://github.com/github/linguist/blob/master/lib/linguist/vendor.yml)               | Excluded from stats                                             |

除了 linguist-language，其余Git attribute后面可跟 `=false`

##### linguist-detectable 可检索

```sh
*.js linguist-detectable  
*.css linguist-detectable=false
```

**linguist-documentation**

```sh
# Apply override to all files in the directory
project-docs/* linguist-documentation
# Apply override to a specific file
docs/formatter.rb -linguist-documentation
# Apply override to all files and directories in the directory
ano-dir/** linguist-documentation
```

##### linguist-generated

Use the `linguist-generated` attribute to mark or unmark paths as generated.

Generated files like minified JavaScript and compiled CoffeeScript .

```
Api.elm linguist-generate
```

##### linguist-language

```
*.[扩展名] linguist-language=[将此文件识别为哪种语言]

*.js linguist-language=vue
*.css linguist-language=vue
*.html linguist-language=vue
*.sh linguist-language=vue
```

##### linguist-vendored

```sh
# Apply override to all files in the directory
special-vendored-path/* linguist-vendored
# Apply override to a specific file
jquery.js -linguist-vendored
# Apply override to all files and directories in the directory
ano-dir/** linguist-vendored
```

#### Git Flow

https://blog.csdn.net/Z_kenshou/article/details/103407521

https://leehao.blog.csdn.net/article/details/47789419?utm_medium=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.wap_baidujs&depth_1-utm_source=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.wap_baidujs

#### 基于gitlab的团队工作流程

https://www.jianshu.com/p/709d425f7f8f?tt_from=copy_link

#### gitlab管理

https://www.cnblogs.com/busigulang/articles/11224401.html

#### gitlab权限说明

- Guest(匿名用户) - 创建项目、写留言薄

- Reporter（报告人）- 创建项目、写留言薄、拉项目、下载项目、创建代码片段

- Developer（开发者）- 创建项目、写留言薄、拉项目、下载项目、创建代码片段、创建合并请求、创建新分支、推送不受保护的分支、移除不受保护的分支 、创建标签、编写wiki

- Master（管理者）- 创建项目、写留言薄、拉项目、下载项目、创建代码片段、创建合并请求、创建新分支、推送不受保护的分支、移除不受保护的分支 、创建标签、编写wiki、增加团队成员、推送受保护的分支、移除受保护的分支、编辑项目、添加部署密钥、配置项目钩子

- Owner（所有者）- 创建项目、写留言薄、拉项目、下载项目、创建代码片段、创建合并请求、创建新分支、推送不受保护的分支、移除不受保护的分支 、创建标签、编写wiki、增加团队成员、推送受保护的分支、移除受保护的分支、编辑项目、添加部署密钥、配置项目钩子、开关公有模式、将项目转移到另一个名称空间、删除项目

#### Gitee权限说明

在 Gitee 平台，仓库成员权限可以以下几种：

| 成员角色     | 权限                                                                                                                                           |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 访客（登录用户） | 对于公有仓库：创建 Issue、评论、Clone 和 Pull 仓库、打包下载代码、Fork 仓库、 Fork 仓库提交 Pull Request、下载附件                                                               |
| 报告者      | 继承访客的权限。 私有仓库：不能查看代码、不能下载代码、不能 Push 、不能 Fork 、 不能提交 Pull Request、可下载附件，不能上传附件，不能删除附件                                                         |
| 观察者      | 继承报告者权限 私有仓库：创建 Wiki、可以 Clone 下载代码、可以 Pull、不能 Fork                                                                                           |
| 开发者      | 创建 Issue、评论、Clone 和 Pull 仓库、Fork 仓库、打包下载代码、创建 Pull Request、 创建分支、推送分支、删除分支、创建标签（里程碑）、 创建 Wiki、可上传附件，可删除自己上传的附件，不能删除他人上传的附件、                  |
| 管理员      | 创建 Issue、评论、Clone 和 Pull 仓库、打包下载代码、创建 Pull Request、 创建分支、推送分支、删除分支、创建标签（里程碑）、创建 Wiki、 添加仓库成员、强制推送分支、编辑仓库属性、可上传附件，可删除自己或他人上传的附件、 不能转移/清空/删除仓库 |

#### 将某个文件夹下的文件上传到远程仓库

```sh
// 远程分支不存在，则自动创建
git subtree push --prefix=文件夹名 主机名 远程分支名
```

#### git使用不同的邮箱配置不同的SSH

https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/about-ssh

https://blog.csdn.net/wangcongcsdn/article/details/106661793

https://www.pianshen.com/article/302848775/

https://www.cnblogs.com/feiquan/p/11538433.html

本章节是针对于 Clone With SSH 的情况。

如果是 Clone With HTTP，则在克隆时输入用户名和密码，同时仓库管理员给你设置好权限就可以了。

首先,介绍一下配置背景:

1. 有两个邮箱A@mail.com  , B@mail.com

2. 两个git账号,一个公司gitlab上的A@mail ,另一个github上的B@mail

3. 目标:提交公司任务到gitLab 上,自己写的代码提交到github上

介绍配置过程:

1. 检查是否设置了全局user.name ，user.email ，如果设置了就取消，取消步骤:
- git config -- global --unset user.name

- git config --global --unset user.email
2. 配置两个不同邮箱下的ssh
- 生成key命令  ssh-keygen -t rsa -C "your_email"

  > ```
  > # 添加一条注释，说明是哪个用户何时在哪台机器上创建的密钥。
  > ssh-keygen -t rsa -C "$(whoami)__$(uname -n)__$(date +"%Y-%m-%d %H:%M:%S")"

- 会提示你输入文件名,可以输入对应的网址的名称，比如~/.ssh/id_rsa_gitlab

- 会提示你是否需要私钥密码

- 这样,会生成两个文件，比如id_rsa_gitlab，id_rsa_gitlab.pub

- 重复以上步骤，生成id_rsa_github，id_rsa_github.pub

在.ssh文件下新建并配置config 文件：

工作中常见的使用就是把公钥配置到Github/Gitee/Coding/华为云等网站, 通过ssh来管理托管在这些平台上的项目和代码。

ssh：

https://wiki.archlinux.org/title/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

主要配置项说明：

https://www.ssh.com/academy/ssh/config

https://man.archlinux.org/man/ssh_config.5

https://www.lainme.com/doku.php/blog/2011/02/%E4%BD%BF%E7%94%A8ssh_config

```
Host    　　主机别名
HostName　　服务器真实地址
IdentityFile　　私钥文件路径
PreferredAuthentications　　认证方式  // publickey | password
User　　用户名
```

注意，github的Host必须写成“github.com”。你可以会有其他要求，比如指定端口号、绑定本地端口，这些都可以通过man来查询，比如

```
Port 端口号
DynamicForward 本地端口号
```

```
Host XXXX                    // 例如 git@saasdev.fastlion.cn 或 github.com
HostName XXXX.com    // 例如 git@saasdev.fastlion.cn 或 github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github

Host XXXX
hostName XXX.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitlab
```

 当前我的配置

```
#Default gitee user Self
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsagitee
User git


#Add gitlabOfCompany user
Host git@saasdev.fastlion.cn
HostName git@saasdev.fastlion.cn
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsagitlab
User git

#Add github user Self
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsagithub
User git
```

配置项

**IdentitiesOnly**

Specifies that ssh should only use the [identity keys](https://www.ssh.com/ssh/identity-key) configured in the `ssh_config` files, even if [ssh-agent](https://www.ssh.com/ssh/agent) offers more identities.

只能用私钥访问，即使 ssh-agent 提供多种认证方式（密码...）。



**User**

Specifies the user to log in as. This can be useful when a different user name is used on different machines. This saves the trouble of having to remember to give the user name on the command line.



添加密钥到ssh：

ssh-agent 是用来控制保存公钥身份证所使用的私钥的程序，其实ssh-agent 就是一个密钥管理器,运行ssh-agent以后，使用ssh-add将私钥交给ssh-agent保管,其他程序需要身份验证的，时候,可以将验证申请交给ssh-agent来完成整个认证过程.

 这个过程在终端输入: 

```sh
ssh-agent bash

// 此处add后边是id_rsa_gitlab的绝对路径，~/ 代表前登录用户的用户目录
// ~/.ssh/id_rsa_gitlab无效的话，可以直接跟绝对路径，例如：C:\\Users\\honor\\.ssh\\id_rsagitee

ssh-add ~/.ssh/id_rsa_gitlab

ssh -T 地址 // 测试是否连上
// 例如：
// ssh -T git@github.com
// ssh -T git@gitee.com
// ssh -T git@saasdev.fastlion.cn 
// github的时候，如果user 不是 git，则ssh -T时需要在github.com前加上git@，即
// ssh -T git@github.com
```

> 如果仓库是公司内网的话，需要通过公司提供的VPN连上内网，ssh-add和 ssh -T 才能添加成功

#### github 在线 IDE

仓库地址前缀加上 https://stackblitz.com/

例如：`https://stackblitz.com/github/Jay-Ohhh/rolib-cli`

**1s github**

例如：`https://github1s.com/Jay-Ohhh/rolib-cli`

8个github技巧

https://mp.weixin.qq.com/s/BpmyTfdI9BUrMnL8k2r1BQ

#### 使用 Travis CI 自动更新

https://cli.vuejs.org/zh/guide/deployment.html#github-pages

#### 提交规范(Angular)

使用VSCode插件 git-commit-plugin

https://marketplace.visualstudio.com/items?itemName=redjue.git-commit-plugin

**Format**

This extension follows the [Angular Team Commit Specification](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines), as follows:

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

See info on the fields below.

**Type**

Must be one of the following:

| Type         | Description                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------------ |
| **feat**     | A new feature                                                                                          |
| **fix**      | A bug fix                                                                                              |
| **docs**     | Documentation only changes                                                                             |
| **style**:   | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) |
| **refactor** | A code change that neither fixes a bug nor adds a feature                                              |
| **perf**     | A code change that improves performance                                                                |
| **test**     | Adding missing or correcting existing tests                                                            |
| **chore**    | Changes to the build process or auxiliary tools and libraries such as documentation generation         |

**Scope**

The scope could be anything specifying place of the commit change. For example `$location`, `$browser`, `$compile`, `$rootScope`, `ngHref`, `ngClick`, `ngView`, etc...

You can use `*` when the change affects more than a single scope.

**Subject**

The subject contains succinct description of the change:

- use the imperative, present tense: "change" not "changed" nor "changes"
- don't capitalize first letter
- no dot (`.`) at the end

**Body**

Just as in the **subject**, use the imperative, present tense: "change" not "changed" nor "changes". The body should include the motivation for the change and contrast this with previous behavior.

**Footer**

The footer should contain any information about **Breaking Changes** and is also the place to [reference GitHub issues that this commit closes](https://help.github.com/articles/closing-issues-via-commit-messages/).

**Breaking Changes** should start with the word `BREAKING CHANGE:` with a space or two newlines. The rest of the commit message is then used for this.

A detailed explanation can be found in this [document](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#).

#### 常见问题

##### refusing to merge unrelated histories

出现这个问题的最主要原因还是在于本地仓库和远程仓库实际上是独立的两个仓库。

解决：在git pull 或 git merge 命令后加上--allow-unrelated-histories

##### .gitignore无效

`.gitignore` 只对**未跟踪的文件**起作用

> 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次提交中有它们的记录。

解决：

例如我想让已经被跟踪的 yarn.lock 被 git 忽略，但还想保存在本地，gitigonore 加上 yarn.lock

- 先把本地的 yarn.lock 剪切到项目以外的目录（相当于删除了该文件），然后提交到远程仓库
- 其他协作者 git pull，也会删除掉 yarn.lock
- 然后把 yarn.lock 拷贝回本项目



##### 加速几十倍 git clone 速度的 --depth 1，它的后遗症怎么解决？

https://mp.weixin.qq.com/s/6VaV70zbnfzWBRxHGbmTcw



##### 用 git bisect 快速定位你想找的 commit

https://mp.weixin.qq.com/s/vKi0nKlmrIvLP42aDdnKZg



##### vscode解决 windows换行crlf与lf冲突

vscode 点击文件 --》首选项 --》 设置 --》 搜索 eol，改变eol为\n(指lf)或者改为(\r\n)，有一个统一的标准就好了。

git在维护版本库的时候统一使用的是LF，这样就可以保证文件跨平台的时候保持一致。
在Linux下默认的换行符也是LF，那也就不存在什么问题。
在Windows下默认的换行符是CRLF，那么我们需要保证在文件提交到版本库的时候文件的换行符是LF，通常来说就是上面的两种方法。

如果你同事中有使用其它系统开发的
你需要先执行上面的操作，再行 以下代码才能解决

```bash
git config --global core.autocrlf false
```



#### Github

##### Github 搜索技巧

Awesome + keyword：关键字 Awesome,帮忙找到优秀的工具列表

关键字 in 是用来限定搜索的范围，可以指定是在名称、描述、readme文档中搜索关键字

```
in:name xxx 名称条件
in:name xxx xxx 多个名称条件
in:description xxx 项目描述 description
in:readme xxx 搜索readme里的内容
stars:>xxx 点赞大于
stars:start..end 点赞数量区间
fork:>xxx fork数大于
fork:start..end fork数量区间
size:>=xxx 项目大小，单位kb
created:>YYYY-MM-DD 创建日期晚于YYYY-MM-DD
pushed:>YYYY-MM-DD  更新日期晚于YYYY-MM-DD
c:xxx 项目使用语言
topics:xxx 主题
user:xxx 作者名称
location:China 项目填写的地址
jack in:fullname 匹配用户名为jack
license:xxx
is:public
is:private
```

##### Gitee搜索技巧

用户可以通过组合「关键字」+「开发语言类型」+「项目收藏数(Watch)」+「项目克隆数(Fork)」+「项目更新时间」等筛选条件，查找用户需要的项目。



##### pull request

我尝试用类比的方法来解释一下 pull reqeust。想想我们中学考试，老师改卷的场景吧。你做的试卷就像仓库，你的试卷肯定会有很多错误，就相当于程序里的 bug。老师把你的试卷拿过来，相当于先 fork。在你的卷子上做一些修改批注，相当于 git commit。最后把改好的试卷给你，相当于发 pull request，你拿到试卷重新改正错误，相当于 merge。

当你想更正别人仓库里的错误时，要走一个流程：

1. 先 fork 别人的仓库，相当于拷贝一份，相信我，不会有人直接让你改修原仓库的
2. clone 到本地分支，做一些 bug fix
3. 发起 pull request 给原仓库，让他看到你修改的 bug
4. 原仓库 review 这个 bug，如果是正确的话，就会 merge 到他自己的项目中

至此，整个 pull request 的过程就结束了。

https://zhuanlan.zhihu.com/p/87603185

**pull request 步骤**

- fork 到自己的仓库

- git clone 到本地

- 上游建立连接
  `git remote add upstream 开源项目地址`

- 创建开发分支 (非必须)
  `git checkout -b test`

- 推送到远程仓库

  `git push -u origin test`

- 修改提交代码
  `git status` `git add .` `git commit -m`  `git pull upstream dev ` `git push`

- 提交pr
  去自己github仓库对应fork的项目下new pull request

**pr之后自动关闭相应的issue**

Github提供了自动关联功能，commit提交代码时只需要在注释中包含issue编号，#issue_id

关闭相应的issue：在commit 的最后加上

- `fixes #xxx`
- `fixed #xxx`
- `fix #xxx`
- `closes #xxx`
- `close #xxx`
- `closed #xxx`

```sh
git commit -m "Fix screwup, fixes #12"
```

**pr之后自动删除 remote 分支**

拥有仓库管理员权限的用户可以配置 PR 合并后自动删除相应的分支。

1. 打开仓库主页面
2. 打开 **Settings**
3. 在 **Merge button** 下面，勾选 **Automatically delete head branches**



##### 跳过工作流程运行

https://docs.github.com/zh/actions/managing-workflow-runs/skipping-workflow-runs

如果将以下任何字符串添加到推送中的提交消息，或拉取请求的 HEAD 提交中，则不会触发原本使用 `on: push` 或 `on: pull_request` 进行触发的工作流：

- `[skip ci]`
- `[ci skip]`
- `[no ci]`
- `[skip actions]`
- `[actions skip]`

或者，可以用两个空行结束提交消息，后跟：

- `skip-checks:true`
- `skip-checks: true`



##### README CHANGELOG CONTRIBUILDING

如果将 README.md 文件放在仓库隐藏的 `.github` 目录、根目录或 `docs` 目录中，GitHub 将会识别您的自述文件并自动向仓库访问者显示。

如果仓库包含多个自述文件，则按以下顺序从位置中选择显示的文件：`.github` 目录，然后是仓库的根目录，最后是 `docs` 目录。

如果将 README 文件添加到与用户名同名的公共仓库的根目录，则该 README 将自动显示在您的个人资料页面上。 您可以使用 GitHub Flavored Markdown 编辑您的个人资料以在您的个人资料 README，以在您的个人资料上创建个性化的区域。 更多信息请参阅“[管理个人资料自述文件](https://docs.github.com/cn/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme)”。



#### Gitlab 

##### 请求合并

https://blog.csdn.net/panjunnn/article/details/106388986



#### 设置代理了 github push 依旧很慢

https://zhuanlan.zhihu.com/p/481574024

**全局设置（不推荐）**

```bash
#使用http代理 
git config --global http.proxy http://127.0.0.1:58591
git config --global https.proxy https://127.0.0.1:58591
#使用socks5代理
git config --global http.proxy socks5://127.0.0.1:51837
git config --global https.proxy socks5://127.0.0.1:51837
```


**只对Github代理（推荐）**

```bash
#使用socks5代理（推荐）
git config --global http.https://github.com.proxy socks5://127.0.0.1:51837
#使用http代理（不推荐）
git config --global http.https://github.com.proxy http://127.0.0.1:58591
```


**取消代理**
当你不需要使用代理时，可以取消之前设置的代理。

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --global --unset http.https://github.com.proxy

# 可以通过打开 .gitconfig 进行编辑
open ~/.gitconfig
```

