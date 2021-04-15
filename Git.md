### Git

#### git常用命令

##### 克隆仓库

```sh
 git clone [url]
```

##### 添加到暂存区

```sh
git add . 将当前目录下的所有文件添加到暂存区
git add 文件/文件夹相对路径  将指定文件/文件夹添加到暂存区
```

##### 将暂存区内容添加到本地仓库中

```sh
git commit -m [message]
```

提交暂存区的指定文件到仓库区：

```sh
git commit [file1] [file2] ... -m [message]
```

**-a** 参数设置修改文件后不需要执行 git add 命令，直接来提交

```sh
git commit -a
```

如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。命令格式如下：

```
git commit -a
```

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

##### 将本地的分支版本上传到远程仓库

```sh
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```sh
git push <远程主机名> <本地分支名
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

```sh
git push --force origin master
```

某分支第一次push时可以使用 **- u** 选项：

如果当前分支与多个主机存在追踪关系，则可以使用 **- u**选项指定一个默认主机，这样后面可以直接用git push。

```sh
git push -u origin 远程分支名 // 同时会创建远程新分支
```

```sh
git push -u origin master 
```

上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机。

##### 新建分支并切换到该分支

```sh
git checkout -b [新分支名]
```

##### 切换到指定分支

```sh
git checkout [指定分支名]
```

##### 合并指定分支到当前分支 

先切换到当前分支，然后

```sh
git merge [指定分支名]
```

合并后可以直接进行push，不需git add . 和 git commit

##### 删除本地分支

```sh
git branch -d 分支名 // 在其它分支才能删除该分支
```

##### 删除远程分支

```sh
git push origin --delete 分支名
```

##### 从远程获取代码库

```shell
git fetch <远程主机名> <远程分支名>:<本地分支名> // 注意空格，本地分支名选项是可选项
```

##### 从远程获取代码并合并本地的版本

```sh
git pull <远程主机名> <远程分支名>:<本地分支名> // 注意空格，本地分支名选项是可选项
```

git pull = git fetch + git merge

##### 查看本地工作区、暂存区中文件的修改状态

```sh
git status
```

##### 查看提交历史

```sh
git log
```

退出 git log：英文状态下按Q

##### 查看分支

查看本地的所有分支：git branch

查看远程仓库所有分支：git branch -r

查看本地和远程仓库的所有分支：git branch -a

##### Tag

**创建本地标签**

```sh
git tag -a [tagname] -m [msg]
git tag -a [tag_name] [commit_id] -m [msg]
```

**创建远程标签**

```sh
git push origin [tag_name]
```

**显示标签**

```sh
git tag --list
git show [tag_name]
```

**删除本地标签**

```
git tag -d [tag_name]
```

**删除远程标签**

```sh
git tag push origin :refs/tags/[tag_name]
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

#### git reset

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

**HEAD 说明：**

- HEAD 表示当前版本

- HEAD^ 上一个版本

- HEAD^^ 上上一个版本

- HEAD^^^ 上上上一个版本

- 以此类推...

  

可以使用 ～数字表示

- HEAD~0 表示当前版本
- HEAD~1 上一个版本
- HEAD^2 上上一个版本
- HEAD^3 上上上一个版本
- 以此类推...

**commit-id** 可以通过 git log 查看本地的所有提交

#### git只提交部分修改的文件（提交指定文件）

1、 git status -s 查看仓库状态

2、 git add src/components/文件名 添加需要提交的文件名（加路径--参考git status 打印出来的文件路径）

3、 git stash -u -k 忽略其他文件，把现修改的隐藏起来，这样提交的时候就不会提交未被add的文件

4、 git commit -m "哪里做了修改可写入..."

5、git pull 拉取合并

6、git push 推送到远程仓库

7、 git stash pop 恢复之前忽略的文件（非常重要的一步）

#### Github 搜索技巧

Awesome + keyword：关键字 Awesome,帮忙找到优秀的工具列表

关键字 in 是用来限定搜索的范围，可以指定是在名称、描述、readme文档中搜索关键字

in:name：指定搜索范围是仓库名称或关键字

in:description：指定搜索范围是摘要中

in:readme：指定搜索范围是readme文档中

\# 按照项目名/仓库名搜索（大小写不敏感）

in:name xxx

\# 按照README搜索（大小写不敏感）

in:readme xxx

\# 按照description搜索（大小写不敏感）

in:description xxx

\# stars数大于xxx

stars:>xxx

\# 筛选stars数量在start和end区间的仓库

stars:start..end

\# forks数大于xxx

forks:>xxx

\# 筛选fork数量在start和end区间的仓库

fork:start..end

\# 编程语言为xxx

language:xxx

\# 创建日期或更新日期晚于YYYY-MM-DD

created:>YYYY-MM-DD

pushed:>YYYY-MM-DD

\# 项目填写的地址

location:China

\# 匹配用户名为jack

jack in:fullname



#### Gitee搜索技巧

用户可以通过组合「关键字」+「开发语言类型」+「项目收藏数(Watch)」+「项目克隆数(Fork)」+「项目更新时间」等筛选条件，查找用户需要的项目。



#### 入职公司拉取代码

1、先拉取代码

```sh
git clone 项目的地址
```

2、拉取你要开发的初始版本，比如develop分支的最新代码

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

与同事协商，解决冲突

```sh
git push -u origin wyn
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



#### git远程仓库地址更换，本地如何修改

方法一：

直接在本地修改远程仓库地址即可：

```sh
git remote set-url origin 远程仓库地址
```

方法二：

先删除，然后添加地址：

```sh
git remote rm origin
git remote add origin 远程仓库地址
```

#### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再add 、commit、merge。

解决冲突就是把Git合并失败的文件手动编辑为协商好的版本，再提交。需要与领导同事协商。

#### 使用 Travis CI 自动更新

https://cli.vuejs.org/zh/guide/deployment.html#github-pages