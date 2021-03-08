# github学习指南

[TOC]

## 关于git的知识

git跟踪的是修改而非文件

## 本地仓库的构成(严格来说并不能称为本地仓库)

本地仓库由git维护的三棵树组成，第一个是你的 `工作目录（working dir）`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `最新本地版本库（HEAD）`，它指向你最后一次提交的结果。

![image-20200307173705675](C:\E\typora\Pictures\image-20200307173705675.png)

## 将本地仓库与github远程仓库关联(git remote)

使用`git remote add origin git@github.com:[用户名]/[项目名].git`即可(其中origin这个参数其实是远程仓库在本地的副本的名字，是可以任意修改的，比如改成"github")

关联后的第一次push需要使用`git push -u origin master`命令，由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

使用`git remote rm `命令删除原来的库后，可以关联多个库，比如用以下的例子可以同时关联github和gitee的远程仓库：

```shell
git remote rm origin
git remote add github git@github.com:michaelliao/learngit.git
git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
```

现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

```shell
git remote -v
gitee	git@gitee.com:liaoxuefeng/learngit.git (fetch)
gitee	git@gitee.com:liaoxuefeng/learngit.git (push)
github	git@github.com:michaelliao/learngit.git (fetch)
github	git@github.com:michaelliao/learngit.git (push)
```

如果要推送到GitHub，使用命令：

```shell
git push github master
```

如果要推送到Gitee，使用命令：

```shell
git push gitee master
```

从本地项目建立一个新git项目的方式：

```shell
cd "本地存在项目的路径"
git init
git remote add origin git@gitlab.com:USERNAME/PROJECTNAME.git
git add .
git commit -m 'first git demo'
git push -u origin master
```

## 多人协作工作模式

1. 首先，可以试图用`git push origin `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。

## 本地远程分支同步（git fetch | git pull | git rebase）

`git fetch`只是将**所有**origin/*的代码（即远程仓库副本的代码）与远程仓库同步了，如果想要将本地仓库中的对应分支更新，还需要使用`git merge origin/[分支名]`命令。

在一个新的目录clone项目后，本地只会有master分支，如果想要在本地仓库中获得其他分支，可以首先使用`git fetch`命令同步远程仓库，然后使用`git switch -c [分支名] origin/[分支名]`来创建并合并分支并将本地分支与远程仓库副本分支关联（相当于本地分支分叉于对应远程副本分支）

```shell
$ git switch -c b2 origin/b2
Switched to a new branch 'b2'
Branch 'b2' set up to track remote branch 'b2' from 'origin'.
```

使用`git pull`会直接将本地分支和`origin/分支名 `（只对当前分支生效，并且当前分支必须有关联的远程仓库副本分支）的内容直接用远程代码库最新版本更新（merge一遍，并非覆盖），不过`git pull`命令不等于`git fetch`+`git merge`，只是比较类似。推荐还是使用`git fetch`+`git merge`的方式。(使用`git fetch `后使用`git merge`、`git merge origin/[分支名]`、`git rebase`、`git rebase origin/[分支名]`效果类似，**唯一区别是rebase有变基的效果**)

![img](C:\E\typora\Pictures\v2-af3bf6fee935820d481853e452ed2d55_720w.jpg)

以分支dev为例：在另一台电脑上修改某一分支内容的最快方法是：

1. `clone`仓库
2. `git switch -c dev origin/dev`建立dev分支，并且更新内容
3. 修改dev分支内代码
4. `git commit -a -m '[提交信息]'`提交代码
5. `git push`将修改推送到github


> 参考资料：https://blog.csdn.net/weixin_41975655/article/details/82887273

`refusing to merge unrelated histories`问题：在 2.9.2 之后，不可以合并不同没有相同结点的分支，如果需要合并两个不同结点的分支，那么需要在`git pull `添加一句代码`--allow-unrelated-histories`

## git help

使用`git help [命令名]`命令获取指定命令的帮助信息

## git rebase

rebase以把**本地未push**的log分叉提交历史整理成直线

不使用rebase，另一个电脑上做的修改（提交标签为src）就会变成一个分支：

```shell
$ git merge
Merge made by the 'recursive' strategy.
 test3.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

etan@DESKTOP-9JJLIMD MINGW64 ~/Desktop/gitt/test (master)
$ git log --graph
*   commit 2b4226024a832589767f2c1384b87512d2bc31b0 (HEAD -> master)
|\  Merge: dbbff8d 24b8f72
| | Author: groers <chenhuanyitan@qq.com>
| | Date:   Mon Mar 9 19:26:15 2020 +0800
| |
| |     Merge remote-tracking branch 'refs/remotes/origin/master'
| |     merge
| |
| * commit 24b8f72101ff48b852f85583d463cd3e1d89c76c (origin/master, origin/HEAD)
| | Author: groers <chenhuanyitan@qq.com>
| | Date:   Mon Mar 9 19:21:33 2020 +0800
| |
| |     src
| |
* | commit dbbff8d6870b7483c480627aedcf0d22c38044ac
|/  Author: groers <chenhuanyitan@qq.com>
|   Date:   Mon Mar 9 19:25:51 2020 +0800
|
|       jack

```



使用`rebase`，提交就会变成一条直线。在这个例子里，双方都是基于amy版本做的修改。我们在一个文件里加上了“jack”这个单词，而对方在另一个文件里加上了"src"这个单词。`rebase`后本地版本库中的jack版本相当于是我们在src版本上加上了"jack"这个单词，而非我们在amy版本这样做，这是副作用。如果使用`reset HEAD~`退回到上一个版本会退回到在另一台电脑推送（push）的src版本，而在上面的`merge`里则会直接退回到真正的jack版本，即在amy版本的基础上加上了"jack"这个单词：

```shell
etan@DESKTOP-9JJLIMD MINGW64 ~/Desktop/gitt/test (master)
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: jack

etan@DESKTOP-9JJLIMD MINGW64 ~/Desktop/gitt/test (master)
$ git log --graph --pretty=oneline
* 0812a5a16b0d3ea52ba56fc8d1b2d35213f2d1bc (HEAD -> master) jack
* 24b8f72101ff48b852f85583d463cd3e1d89c76c (origin/master, origin/HEAD) src
* 81a4e3bb0efde19c95706ccb270242934cfaa098 amy
* 179b0f9583be7c299aeace7b072f2ef6b05c30ff line
```



## git add

`git add`命令可以将文件提交到暂存区，使用`git add *`命令可以将目录下所有文件提交到暂存区。对于`.gitignore`文件无视的文件可以加上`-f`参数强制将此文件提交到暂存区。

## git commit

1. `-a`参数意味着将当前目录下所有**已追踪文件**加入暂存区再commit
2. `-m`参数意味着添加commit tag（不加`-m`参数就会弹出一个文件让你添加）
3. 使用`--no-edit`参数可以不加commit备注（**最好别用**）
4. 使用`--amend`参数可以将暂存区中内容追加到最近一次提交中，加上`-no-edit`参数可以不改变commit备注。如果不`add`文件直接`git commit --amend -m '[备注]'`可以修改上一次的commit备注。（`--amend`命令最好不要对已经`push`过的版本使用，否则会导致修正后的版本push时遇到冲突）

## git push

1. `git push origin [分支名]`是默认本地同名分支推远程同名分支，就算只输入git push也是如此，但是在本地新建分支后首次向远程push时必须使用`git push -u origin [分支名]`命令

2. 删除远程分支：使用`git push --delete origin [分支名]`可以删除github的远程分支

3. 不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用`–all`选项。可以加上`-u`选项使本地分支关联到远程分支。

      ```shell
      git push --all origin -u
      ```

   这样以后推送只需要简单的使用`git push`命令即可

## git branch

1. 单独使用`git branch`命令会列出所有分支，以及指出目前所在的分支

2. 使用`git branch [分支名]`命令会复制当前分支的内容建立一个全新的分支（如果同名分支已存在则会报错）

3. 使用`git branch -d [分支名]`命令可以删除一个分支，使用`git branch -D [分支名]`可以强行删除一个未合并的分支

4. 使用`git branch -a`可以查看远程和本地的所有分支,使用`-r`参数可以只查看远程仓库分支：

   ```shell
   $ git branch -a
   * dev
     master
     remotes/origin/HEAD -> origin/master
     remotes/origin/b1
     remotes/origin/b2
     remotes/origin/dev
     remotes/origin/master
   
   ```

5. 可以使用`git branch --set-upstream-to=origin/[分支名] [分支名]`将远程仓库副本分支与本地仓库分支关联起来


## git merge

1. 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

   如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

   使用`--no--ff`参数可以禁用`Fast forward`模式

   这是使用`Fast forward`模式merge的log：

   ```shell
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git merge -m 'merge hello' b2
   Updating 40e7662..680b672
   Fast-forward (no commit created; -m option ignored)
    test3.txt | 1 +
    1 file changed, 1 insertion(+)
   
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git log --graph --pretty=oneline -5
   * 680b67250c2acee9a69017ca1f1b773b2144841e (HEAD -> master, b2) hello you are
   *   40e7662d0e55a0f38aaf02cfad0f2917f969f797 conflict fixed
   |\
   | * 11525b552a57df244c7ec9df63da8dd88ef63fb6 (b1) test3.txt: nice a day
   | *   b35cf51e2ca15170fec64b364e0852863b650b9a Merge branch 'master' into b1 merge from master test3.txt 1
   | |\
   | | * c520d989f186be29ca843c6afdea2b612a964b8e test3.txt remain 1
   
   ```

   这是不使用`Fast forward`模式merge的log：

   ```shell
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git merge --no-ff -m 'merge hello' b2
   Merge made by the 'recursive' strategy.
    test3.txt | 1 +
    1 file changed, 1 insertion(+)
   
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git log --graph --pretty=oneline -5
   *   539cdb85b83ac6b8b0d37835a8a1ff29e4dcfd13 (HEAD -> master) merge hello
   |\
   | * 680b67250c2acee9a69017ca1f1b773b2144841e (b2) hello you are
   |/
   *   40e7662d0e55a0f38aaf02cfad0f2917f969f797 conflict fixed
   |\
   | * 11525b552a57df244c7ec9df63da8dd88ef63fb6 (b1) test3.txt: nice a day
   | *   b35cf51e2ca15170fec64b364e0852863b650b9a Merge branch 'master' into b1 merge from master test3.txt 1
   | |\
   
   ```

2. 使用`git merge`合并分支的时候可能会出现合并合并冲突，进而进入冲突解决状态，这个时候如果使用`git merge --abort`命令，那么可以退出冲突解决状态，将文件恢复为合并前的状态。

3. 警告：运行`git-merge`时含有大量的未commit文件很容易让你陷入困境，这将使你在冲突中难以回退。因此非常不鼓励在使用`git-merge`时存在未commit的文件，建议使用`git-stash`命令将这些未commit文件暂存起来，并在解决冲突以后使用`git stash pop`把这些未commit文件还原出来。

## git diff

1. `git diff [区域名，如HEAD|分支名] [文件名]`，不加参数是默认比较`工作目录`和`暂存区`的所有文件

2. 输出解释：

   a、b分别代表变动前（默认暂存区，用---表示）、变动后（默认工作区，用+++表示）的对应文件，即源文件和目标文件。第二行表示两个版本的git哈希值，其中最后的六位数字是对象的模式（普通文件，644权限）

   -开头的行，是只出现在源文件中的行；+开头的行，是只出现在目标文件中的行；空格开头的行，是源文件和目标文件中都出现的行；

   差异按照**差异小结**进行组织，每个差异小结的第一行都是定位语句，由@@开头，@@结尾；如下图，源文件hello.go 第2行开始的6行与目标文件hello.go 第2行开始的7行构成一个差异小结，目标文件中第七行新加了一行"//test"；源文件test1.txt 0行开始的0行与目标文件test1.txt 1行开始的3行构成一个差异小结，原因是暂存区不存在此文件，工作区新增了一个test1.txt 文件。

![image-20200307174028518](C:\E\typora\Pictures\image-20200307174028518.png)

## git staus

1. `git status`用于查看本地仓库状态，其中Changes not stage for commit 表示在工作区修改了，但是未提交（add）到暂存区的文件；Changes to be committed 表示已经提交到暂存区，但是还未commit的文件。

![image-20200307175840709](C:\E\typora\Pictures\image-20200307175840709.png)

2. 举一个例子，最开始没有任何改动时，git staus显示如下：

![image-20200307181005502](C:\E\typora\Pictures\image-20200307181005502.png)

​	现在我们在test2.txt文件中增加一行后，git staus显示内容如下：

![image-20200307181108375](C:\E\typora\Pictures\image-20200307181108375.png)

​	git add 后再输入git status命令显示内容如下：

![image-20200307181143485](C:\E\typora\Pictures\image-20200307181143485.png)

​	git commit 后输入git status命令显示内容如下，提示我们本地版本库已经超前远程分支了，这个时候需要git push：

![image-20200307181213825](C:\E\typora\Pictures\image-20200307181213825.png)

​	git push后就一切完好了：

![image-20200307181442629](C:\E\typora\Pictures\image-20200307181442629.png)

## git log

1. `git log`命令显示从最近到最远（最上面显示的是最近的）的提交日志（在每个分支输入此命令结果不一样，各自有自己的log），黄色的数字是提交的版本号,HEAD->master 代表这一行的版本是master 分支HEAD指向的版本。

![image-20200307182858754](C:\E\typora\Pictures\image-20200307182858754.png)

​		可以加上 `--pretty=oneline` 隐藏作者和日期：

![image-20200307183010031](C:\E\typora\Pictures\image-20200307183010031.png)

​	`	--stat`:是在git log 的基础上输出文件增删改的统计数据。

​	`-p`:控制输出每个commit具体修改的内容（和`--stat`类似），输出的形式以diff的形式给出。

​	`git show`命令同`git log -p`输出类似，只不过它只显示一个commit的内容，如果不指定commit hash, 它默认输出HEAD指向commit的内容。

​	加`--author="作者名"`用来过滤commit,限定输出给定的用户。

​	`-n`参数可以限制输出数目，如-2 、-3

​	`--graph`参数可以看到分支合并图

​	``--after``和`--before`可以限制日期范围：

```shell
git log --after '10-1-2019'
```

​	控制是否显示merge的commit：`--merges`或者`--no-merges`

​	当然，别忘了，用了`git log`后是按`q`退出

## git reset | git reflog

1. 使用`git log`命令可以看到每次commit的版本号，当前版本用`HEAD`表示，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

2. 要想把版本回退就可以使用`git reset [HEAD | 版本号] [文件名]`命令（参数也可以使用版本号，这个版本号可以不用写全，写前面几位就可以），使用这个命令可以使暂存区的所有文件，或指定的文件恢复到某一个commit了的版本，而工作区文件的恢复情况取决于是否加`--hard`参数：

   ```shell
   $ git log --pretty=oneline
   fb49c1948060e6c29e46aae61bf80eaabde2ac0f (HEAD -> master) Merge branch 'b1' merge test3.txt from b1 aaaa
   f086f4094669ae839af2e486bcc92b0d2dd4bdcd (b1) b1 new a test3.txt
   854df109ec1da7972dde786869009f5d4ad3a7ab (origin/master, origin/HEAD) nothing
   70f7d1376236e1689aa67eaae4b526c27cf1c914 test add
   986f397fd89737429182e2136ef7e70744ab1aaf (origin/b1) 第二次提交
   eab6e8a3757553e52ebb57d89abdf41d26661655 discard hello.go
   93fb290fc26ab510ee46004953fcbebc1879bebd 第一次提交
   b34eab6939f5e401ae8815347429df8daf3b0c30 initial hello.go
   
   ```

   ```shell
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git reset HEAD^
   
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git log --pretty=oneline
   854df109ec1da7972dde786869009f5d4ad3a7ab (HEAD -> master, origin/master, origin/HEAD) nothing
   70f7d1376236e1689aa67eaae4b526c27cf1c914 test add
   986f397fd89737429182e2136ef7e70744ab1aaf (origin/b1) 第二次提交
   eab6e8a3757553e52ebb57d89abdf41d26661655 discard hello.go
   93fb290fc26ab510ee46004953fcbebc1879bebd 第一次提交
   b34eab6939f5e401ae8815347429df8daf3b0c30 initial hello.go
   
   ```

   可以看到回退到了从b1分支merge前的版本

   ```shell
   $ git status
   On branch master
   Your branch is up to date with 'origin/master'.
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           test3.txt
   
   nothing added to commit but untracked files present (use "git add" to track)
   
   ```

   test3.txt文件虽然还存在于文件夹中，但是已经没有在工作区以及本地版本库中了。（如果`git reset`命令使用了`--hard`参数（使用此参数则不能选定reset的那个文件，只能reset所有文件），那么工作区只会保留回退到的那个版本的文件。即在这个例子中test3.txt文件会丢失）

   如果几天后又想恢复到之前丢掉的版本，那么可以使用`git reflog`命令查看操作日志：

   ```shell
   $ git reflog
   fb49c19 (HEAD -> master) HEAD@{0}: reset: moving to fb49c19
   fb49c19 (HEAD -> master) HEAD@{1}: reset: moving to fb49c19
   854df10 (origin/master, origin/HEAD) HEAD@{2}: reset: moving to HEAD^
   fb49c19 (HEAD -> master) HEAD@{3}: reset: moving to fb49c19
   854df10 (origin/master, origin/HEAD) HEAD@{4}: reset: moving to HEAD^
   fb49c19 (HEAD -> master) HEAD@{5}: checkout: moving from b1 to master
   f086f40 (b1) HEAD@{6}: checkout: moving from master to b1
   fb49c19 (HEAD -> master) HEAD@{7}: merge b1: Merge made by the 'recursive' strategy.
   ... ...
   ```

## git checkout | git switch

1. `git checkout`命令最常用的就是以`git checkout [分支名]`的方式切换工作分支，切换分支时工作区的内容必须提交（commit）到本地版本库。加上`-b`参数可以在新建立一个分支的同时，切换到当前分支。使用`-B`参数会强制创建新分支，并且会覆盖原来存在的同名分支。

   ```shell
   git checkout -b newBranch
   ```

2. 使用`git checkout origin/分支名`命令可以切换到远程分支查看已经push的内容

3. 假如你的某个分支上，积累了无数次的提交，你也懒得去打理，打印出的log也让你无力吐槽，那么这个命令将是你的神器，它会基于当前所在分支新建一个赤裸裸的分支，没有任何的提交历史，但是当前分支的内容一一俱全。新建的分支，严格意义上说，还不是一个分支，因为HEAD指向的引用中没有commit值，只有在进行一次提交后，它才算得上真正的分支。

   ```shell
   git checkout --orphan <new_branch>
   ```

4. 切换分支时，对于已经被追踪（tracked）的文件，工作区未添加（add）到暂存区的**文件修改**会丢失（这个时候git bash会报错，并且要求你处理后才能切换分支），所以切换分支前要先add文件。对于未追踪（untracked）的文件（即从未add过的文件），切换分支时无论该文件创建于哪一个分支，其依旧会存在于新切换到的分支，并且不会有任何改变。
5. 使用`git checkout [-- 文件名]`可以将暂存区中对应文件的版本覆盖到工作区，使用`git checkout [版本号] [--文件名]`可以将本地版本库中的内容覆盖到工作区和暂存区，相当于`git reset [版本号] [文件名]`
6. 其实使用`git switch`命令切换分支更加科学，创建并切换到新的`dev`分支，可以使用：`git switch -c [分支名]`，直接切换到已有的分支，可以使用：`git swicth [分支名]`

## git stash

1. `git stash`命令可以将当前工作区和暂存区的内储藏起来，并且使用命令后工作区和暂存区的内容会变成本地版本库`HEAD`的内容，以便于切换分支

2. 使用`git stash list`命令可以查看存储的stash列表

   ```shell
   etan@DESKTOP-9JJLIMD MINGW64 ~/go/src/test (master)
   $ git stash list
   stash@{0}: WIP on master: 539cdb8 merge hello
   ```

3. 使用`git stash apply [stash号]`命令可以恢复stash现场，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除

   ```shell
   $ git stash apply stash@{0}
   On branch master
   Your branch is ahead of 'origin/master' by 13 commits.
     (use "git push" to publish your local commits)
   
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   test3.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   ```

4. 也可以用`git stash pop`命令恢复的同时删除存储记录

## git cherry-pick

`git cherry-pick [提交号]`命令可以复制（merge）一个特定的提交到当前分支

在需要修复bug的时候，master更新了bug，要想在另一个分支比如b1更新这个bug但是不想和master merge，那么可以用这个命令将修复bug的那个分支的`commit`更新b1。

## git restore

1. 使用`git restore --staged [文件名]`命令可以取消追踪一个文件（但是如果已经commit过了，那就无法从本地版本库去除此文件了）

## git tag

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起，不同版本标签不能重名（不同分支也可能拥有同一个版本）

1. 可以使用`git tag [标签名]`给最新的提交（commit）版本打上标签，在同一版本可以同时打上许多标签

   也可以使用`git tag [标签名] [版本号]`给过去的提交打标签

2. 可以使用`git tag`命令查看所有标签（按字母顺序列出）

3. 可以使用`git tag -d [标签名]`命令删除某个标签

4. 将标签推送到远程可以使用`git push origin v`相当于命令`git push origin refs/tags/[源标签名]:refs/tags/[目的标签名]`

   可以在github选择带有特定标签的仓库版本，这样就可以存放多个version的软件啦

   ![image-20200309202742808](C:\E\typora\Pictures\image-20200309202742808.png)

   可以使用`git push origin --tags`命令一次性将所有的本地标签推送到远程

5. 参照推送标签的完整命令，要想删除远程标签（推荐首先删除本地标签），可以使用命令`git push origin :refs/tags/[目的标签名]`

## .gitignore

在`.gitignore`文件中定义的文件和目录会被git忽略（在gitignore文件中“#”开头的行是注释）

忽略文件的原则是：

1.  忽略操作系统自动生成的文件，比如缩略图等；
2.  忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3.  忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

使用`git check-ignore -v [文件名]`命令可以检查`.gitignore`文件中的哪一行无视了某一个文件，若没有输出，则说明没有无视该文件。

`.gitignore`填写规则：

```swift
/mtk/ 过滤整个文件夹

*.zip 过滤所有.zip文件

/mtk/do.c 过滤某个具体文件

!/mtk/one.txt 将指定文件添加到版本管理，可以绕过之前的ignore
```

> .gitignore文件库：https://github.com/github/gitignore

## 配置命令别名（git config）

使用`git config --global alias.[别名] "[原命令|命令串]"`可以给命令配置别名（`-global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用，即对当前用户生效，如果不加则只对当前仓库生效）：

```shell
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
```

## git rm

当被跟踪的文件里面有不想跟踪的文件时，使用命令`git rm`删除文件

`git rm --cached [文件名]`    删除文件的跟踪，并保留在本地。

`git rm --f [文件名]` 删除文件的跟踪，并且删除本地文件。

## git revert

`git revert [版本号]`命令可以反做指定版本的事情，对冲掉对应版本的操作，并将结果提交（commit）为一个新的版本。所以这个命令可以加上`-m`参数。这对于当我们已经`commit`版本2，但是想撤销版本1内容时十分有用，可以直接对冲版本1内容达到目的。这个命令可能引起merge冲突，同样的可以用`--abort`参数恢复到冲突前的状态。

## git blame

`git blame [文件名]`命令可以显示指定文件每一行最后修改的版本和作者

```shell
git blame <filename> -L 10,10 查看第十行

git blame <filename> -L 10 查看十行即以后

git blame <filename> -L 10,20 查看十至二十行


git blame <filename> -L 10,+5 查看十行和后五行

git blame <filename> -L 10,-5 查看第十行和前五行

```



## git cheat sheet（git 常用命令集及推荐做法）

1. 不要提交（`commit`）未完成的工作，如果要暂时离开，请用`git stash`功能
2. 一个功能点一个提交（`commit`），要勤于提交
3. 提交前记得写测试确保代码无误
4. 写好`commit messages`

>参考：https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf