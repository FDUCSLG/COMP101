<center><font size=7>Git/Github</font></center>
<p align='right'>2023.9.24</p>

## 前言
此篇笔记将基于一本叫做Pro Git的书，简短讲述git和github的使用。并且基于一些模拟实验让你快速上手。  
### 学习资源
- [Pro Git](https://git-scm.com/book/en/v2)  /  [Pro Git中文版](https://git-scm.com/book/zh/v2)，推荐阅读1-3章
- [学习git的在线游戏](https://learngitbranching.js.org/)，挺好玩的
- [ohshitgit](https://ohshitgit.com/)，简短的介绍了如何从 Git 错误中恢复
- [Git for Computer Scientists](https://eagain.net/articles/git-for-computer-scientists/)，简短的介绍了 Git 的数据模型
- [git-from-the-bottom-up](https://jwiegley.github.io/git-from-the-bottom-up/)，详细的介绍了 Git 的实现细节
- [explain-git-in-simple-words](https://xosh.org/explain-git-in-simple-words/)，如其名
- [用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)，一篇精美文章


### 我们为什么需要git
git是一个分布式版本控制软件  
在项目开发中，它能方便我们跟踪文件的信息，包括谁修改了它、变化了哪些部分；可以让我们更方便地添加、调试新功能，如果不行还可以回退到上一个版本；可以让我们更加方便地团队协作，和其他人进行信息交互，加快工作效率  
世界上最大的开源代码社区github就是基于git的  
从短期好处来看，学习git可以方便你进行代码的管理、调试，从长远角度来看，将来你一定会和别人协同工作，那么学会使用git很可能是一个必不可少的技能  

### 该如何学git
我分享一下我的看法：git的功能实在是太多了，想一口气掌握所有的功能实在是不可能。  
我的建议是可以先掌握基本的用法，在实践中再慢慢摸索。我是通过[学习git的在线游戏](https://learngitbranching.js.org/)上手git的  
[Pro Git](https://git-scm.com/book/en/v2)  /  [Pro Git中文版](https://git-scm.com/book/zh/v2)电子书详尽介绍了git的使用，可以把它作为可以时常翻阅的手册，如果你愿意看官方文档可以直接`git help <command> / git <command> --help`，简短用法的查看可以`git <command> -h`  
结合代码托管网站使用，比如github。在github上你可以拥有一个属于自己的代码仓库，可以放置你的课程项目或者大作业，可以把你的一些新奇的有趣的想法记录下来。更进一步，你还可以查看别人的项目，在`issue`栏与来自世界各地的开发者参与讨论，感兴趣可以`fork`别人的项目然后添加新的功能，说不定还能和项目拥有者成为朋友。在使用中，你也能加深对git的理解  
如果你也是一名开源爱好者，那么就开始吧。

## 起步
### 下载和安装
[git下载](https://git-scm.com/download/win)  
如果官网速度太慢可以去镜像网站：
[阿里镜像](https://registry.npmmirror.com/binary.html?path=git-for-windows/)  
安装配置可以参考以下博客：
- https://zhuanlan.zhihu.com/p/443527549
- https://blog.csdn.net/qq_41521682/article/details/122764915  

### 创建一个你的项目（当然是模拟的）
新建文件夹  
在项目文件中打开git bash，可以按住shift+右键打开

### git配置
使用`git config`来设置 Git 外观和行为的配置。根据命令参数的不同可以修改不同级别的配置：
1. `--system` 选项：修改/etc/gitconfig 文件，包含系统上每一个用户及他们仓库的通用配置。
2. `--global` 选项：修改~/.gitconfig 或 ~/.config/git/config 文件，只针对当前用户。
3. `--local` 选项（默认）：修改当前使用仓库的 Git 目录中的 config 文件，针对该仓库。

每一级别会覆盖上一级别的配置
- 配置名字和邮箱
```shell
git config --global user.name <your name>
git config --global user.email <your email>
```
- 查看配置
```shell
git config --list
```
- 找到core.editor，如果没配置文本编辑器，就配置一下
```shell
git config --global core.editor "D:\Microsoft VS Code\bin\code"
```
- 查看你的配置
```shell
git config user.name
git config user.email
```
- 初始化git
```shell
git init
```
### github
git有两种项目仓库：本地仓库和远程仓库。本地仓库就在你的电脑上，远程仓库可以是代码托管网站，这里使用github。  
#### 注册账号
https://www.github.com
#### 创建一个仓库
进入your profile/repositories，点击new新建一个仓库
#### 在git中配置你的远程仓库
使用HTTP作为传输协议，获取你的远程仓库的url（仓库的网址就是）  
```shell
git remote add <shortname> <url>
```
意思是创建一个远程仓库，将其命名为某个shortname，以后就可以用shortname来代指这个远程仓库  
查看远程仓库：
```shell
git remote -v
```
我们也可以使用
```shell
git remote rename <oldname> <newname>
```
来重命名远程仓库

## git基本使用
### git add
Git 管理下的文件有3种状态：已提交（committed）、已修改（modified） 和已暂存（staged）
- 已修改表示修改了文件，但还没保存到本地仓库中。
- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- 已提交表示数据已经安全地保存在本地仓库中
 
在项目文件夹中新建一个文件a.c  
使用`git status`命令查看文件的状态
```shell
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.c

nothing added to commit but untracked files present (use "git add" to track)
```
我们发现a.c文件处于未跟踪的状态，说明还没有纳入git的管理之下  
我们使用`git add`命令跟踪文件
```shell
git add a.c
```
再输入`git status`
```shell
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a.c
```
git add支持字符串通配符
```shell
git add lib/*.so
```
跟踪lib目录下的所有.so文件
### git commit
`git commit`用于提交文件  
我们修改a.c，里面插入代码
```c
#include <stdio.h>

int main()
{
  printf("hello a\n");
  return 0;
}
```
`git status`
```shell
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a.c

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.c
```
显示我们修改了a.c  
这时我们还要再执行一次`git add a.c`暂存我们的修改（`git add`是个多功能命令，既可以跟踪新文件又可以暂存更改）  
然后用`git commit`提交我们的修改  
```shell
$ git commit -m "A is written"
[master (root-commit) e6e0cbd] A is written
 1 file changed, 7 insertions(+)
 create mode 100644 a.c
```
-m 后面跟的是message，表示此次提交的消息，一般用于说明这次提交修改了什么，消息是必须的  
#### git commit -am
如果我们新建了很多文件，又不想一个一个暂存然后提交怎么办？  
答：使用`git commit -am <message>`，可以看作是git add和git commit的组合，会把所有已跟踪的、修改过的文件提交，这个命令十分常用

#### git commit --amend
有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了，可以使用
```shell
git commit --amend
```
比如刚才提交的信息写错了，我们可以使用：
```shell
git commit --amend -m "sorry A still has some bugs"
```

### git status -s
输出的状态信息太长怎么办？
```shell
git status -s
```
前面的字符说明文件的状态，??为未跟踪，A为新添加到暂存区的文件，M为修改过的文件  
尽量使用简洁的`git status -s`，当你的文件多起来

### .gitignore
我们总有文件不需要加入git的管理，但也不希望它们老是出现在未跟踪的文件列表，.gitignore文件很好地解决了这个问题  
现在我们添加一个log.md文件，用于记录我们的工作日志，显然不想git跟踪它，所以我们再新建一个.gitignore文件  
```shell
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        log.md

nothing added to commit but untracked files present (use "git add" to track)
```
向.gitignore里面加入/log.md，'/'表示当前项目根目录下
```shell
$ echo /log.md > .gitignore
```
```shell
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
```
这样git就不再跟踪log.md文件了  
.gitignore也支持字符串通配符的写法，详细请看[官方gitignore教程](https://github.com/github/gitignore)

### git diff
这个命令用于显示文件间的差异  
这个用命令行打印不是很直观，不过现在许多软件都可视化了这个功能，比如vscode、git Fork、github desktop等等  
也可以使用其他工具，查看你的系统支持哪些git diff插件：
```shell
git difftool --tool-help
```
装好这些插件，可以使用`git difftool`命令来调用这些工具

### git rm
现在我们添加一个b.c文件，并使用`git add`添加跟踪  
突然我们不想要这个文件了，
```shell
git rm -f b.c
``` 
注意此操作会把文件从你的工作目录中也一并删除  
如果你只是想把此文件从暂存区移除（而且不跟踪），但仍保留在工作目录中
```shell
git rm --cached b.c
```
`git rm`也支持字符串通配符

### git mv
`git mv`和传统的`mv`操作不同，它等于`git rm`+`git add`，所以可以使用它来进行重命名操作

### 撤销修改
- `git reset HEAD <file>`
可以取消暂存
- `git checkout -- <file>`
可以取消对文件的修改  
`git checkout -- <file>`是一个危险的命令。你对那个文件在本地的任何修改都会消失，Git会用最近提交的版本覆盖掉它。

### git 别名
如果你觉得每次写`git reset HEAD`太麻烦了，可以使用
```shell
git config --global alias.unstage 'reset HEAD'
```
以后就可以用`git unstage`代替`git reset HEAD`了
> 有点像typedef

## git分支
这是git的亮点特性  
使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线
### git 底层原理简述
#### git 对象
推荐学习：[MIT-Missing Semester-Git](https://missing-semester-cn.github.io/2020/version-control/)  
git通过快照机制保存数据。
git中主要有三种对象：blob对象、树对象、提交对象，blob对象用于存储文件，树对象存储项目列表和文件索引，提交对象包含提交的信息，这些对象都属于快照（快照可以理解为数据在某一时刻的状态）  
git通过SHA-1校验和（一种哈希算法）来唯一地标识每个对象，树对象存储的文件索引也是SHA-1哈希值  
当我们暂存某些文件时，git会为每一个文件计算SHA-1校验和（一种哈希算法），然后为每一个文件创建一个blob对象，并将校验和加入暂存区中  
当提交时，git会产生一个树对象和一个提交对象。树对象记录了项目目录的结构和blob对象的索引（SHA-1校验和）；提交对象保存了指向树对象的指针和所有提交信息（message、author、date etc.），还会保存对上一次提交对象（父对象）的指针。  
请放心，多次提交会不会造成文件的冗余，git聪明地设计了许多机制来避免文件冗余，有兴趣可以查看[Pro Git](https://git-scm.com/book/en/v2)  /  [Pro Git中文版](https://git-scm.com/book/zh/v2)中`Git 内部原理/Git 对象`章节，或者搜索`git gc`命令
#### git 分支
git分支，本质上只是指向提交对象的可变指针，具体地说，仅是包含提交对象校验和（长度为40的SHA-1值字符串）的文件，所以它的创建和销毁都异常高效。所以我们鼓励多建分支  
当然分支也是文件，也有校验和  

### 分支创建和切换
我们想要基于a.c开发两种不同的功能  
使用`git branch`创建分支b1 
```shell
git branch b1
```
查看分支信息
```shell
git branch -v
```
```shell
$ git branch -v
  b1     986cfb7 sorry A still has some bugs
* master 986cfb7 sorry A still has some bugs
```
打`*`号的是当前所在分支，master为`git init`创建的默认分支  
使用`git checkout`切换到b1分支
```shell
git checkout b1
```
```shell
$ git branch -v
* b1     986cfb7 sorry A still has some bugs
  master 986cfb7 sorry A still has some bugs
```
我们也可以同时创建和切换分支
```shell
git checkout -b b2
```

### HEAD指针
HEAD指针是一个指针，指向当前所在的本地分支  
有关HEAD指针有许多奇妙用法

### 项目分支和历史
我们在b2分支下新建两个文件f1.c和f2.c，向f1.c中写入
```c
#include <stdio.h>

int main()
{
  printf("hello b2\n");
  return 0;
}
```
```shell
git add f1.c f2.c
```
```shell
git commit -am "function2"
```
然后
```shell
git checkout b1
```
神奇的事情发生了，在我们的项目目录下f1.c和f2.c不见了！这就是分支管理的奇妙之处了，b2的修改对b1是不可见的，b1完全是基于原来的版本进行开发  
我们再在b1分支下新建两个文件f1.c和f3.c，向f1.c中写入
```c
#include <stdio.h>

int main()
{
  printf("hello b1\n");
  return 0;
}
```
```shell
git add f1.c f3.c
```
```shell
git commit -am "function1"
```
有重名文件也完全没有问题，它们可以作为两个不同的开发方向走下去

### 分支合并
假设我们已经将两个分支上的功能开发完成了，想要把它们整合在一起  
使用`git merge`命令（现在在b1分支）
```shell
git merge b2
```
发生错误了：merge conflict  
这是因为b1和b2包含了同名但内容不同的文件f1.c，这时我们需要修改两个文件让它们一致  
可以使用`git mergetool`，像`git difftool`一样使用外部插件；一些软件也包含了可视化的mergetool  
我将b2下的f1.c改为了b1状态下的f1.c  
此时执行`git status`还显示未merge的状态，执行
```shell
git commit -am "merge b2 into b1"
```
完成merge

### 分支删除
```shell
git branch -d <branch name>
```

### 分支移动
```shell
git branch -f <branch> <index>
```
index可以是提交对象的哈希值，也可以是一个branch

### git tag
tag类似于一个稳定的引用  
当我们在分支上提交时，分支也会指向最新的提交，但我们可以在一些拥有比较稳定的功能的提交状态上打tag，tag不会移动，以后这个tag用于标识这个项目的某个稳定版本  
```shell
git tag v1
```
创建v1的tag  
再新建f4.c
```shell
git add f4.c
```
```shell
git commit -am "new function"
```
我们可以通过
```shell
git checkout v1
```
来返回v1版本

### git log
`git log`是非常实用的命令，因为在你创建了很多分支、提交了很多次文件之后，你肯定已经记不清提交历史是怎么样的了，这时我们可以使用`git log`来查看提交历史  
```shell
git log
```
这时就会打印你所有的提交和分支的历史  
我们也可以让打印变得漂亮一点
```shell
git log --pretty=oneline
```
甚至使用ascii字符串生成图表进行演示
```shell
git log --pretty=oneline --graph
```
当然也有可视化的工具，比如`git Fork`  
还有很多其他选项，可以让我们限制输出的长度、限制提交的时间段、限制提交文件的路径等等

### git 工作流
一种比较常见的工作流就是有一个项目的主分支（比如master），在需要开发新功能时创建分支，分支功能开发、调试完成后merge回主分支

### git rebase
使用
```shell
git log --pretty=oneline --graph
```
```shell
$ git log --pretty=oneline --graph
* d115d37591b2e24be184b5c59d451f763b75a975 (HEAD -> b1) new function
*   f28e6102ba620e4fb11ff5cb5aea694ebb31de26 (tag: v1) merge b2 into b1
|\
| * d58efa73baecee65458e8f42a5b666c80676c236 (b2) function2
* | d844f1335fedf487557c0693b6484b2354d292b4 function1
|/
* 986cfb73c0c788e736c4746d32912eabd90e73e3 (master) sorry A still has some bugs
* 03c24c089a1134869ae9acaa2ccd8fb16de24921 A is written
```
我们发现分支的合并其实就是新建了一个提交对象，它有两个父对象分别是两个合并的分支  
假如我们希望我们的工作线是一条单一的、没有分支的，那么我们可以使用rebase来代替merge  
我们再在master分支下新建一个1.txt文件
```shell
git checkout master && echo 1 > 1.txt
```
```shell
git add 1.txt && git commit -am "A has new function"
```
我们用rebase把master分支和b1分支整合起来
```shell
git rebase <br1> <br2>
```
rebase会找到两个分支的共同祖先，然后朝br1走，找到br1后，把br2沿着br1向下接  
```shell
git rebase b1 master
```
再查看分支历史
```shell
$ git log --pretty=oneline --graph
* b2df402928a4999ac975e4412fe5f6c0fa045829 (HEAD -> master) A has new function
* d115d37591b2e24be184b5c59d451f763b75a975 (b1) new function
*   f28e6102ba620e4fb11ff5cb5aea694ebb31de26 (tag: v1) merge b2 into b1
|\
| * d58efa73baecee65458e8f42a5b666c80676c236 (b2) function2
* | d844f1335fedf487557c0693b6484b2354d292b4 function1
|/
* 986cfb73c0c788e736c4746d32912eabd90e73e3 sorry A still has some bugs
* 03c24c089a1134869ae9acaa2ccd8fb16de24921 A is written
```
发现master和b1的融合就不是两个分支合并了，而是master的新提交被接在了b1后面  
rebase还有许多有意思的功能，比如`git rebase -i`，这个可以自行体会

## git与github
你的github项目有可能是多人协作的，你在本地完成的工作可以提交到github上，也可以把别人提交的功能拉取到你本地  
### git fetch
在你的github仓库上新建一个文件steve.c，然后在你的本地仓库上执行
```shell
git fetch origin main
```
查看一下分支,使用-r查看远程分支
```shell
$ git branch -r
  origin/main
```
我们切换到这个分支
```shell
git checkout origin/main
```
可以发现，现在我们的环境和github的一致了
如果我们想在这个分支上工作
```shell
git checkout main
```
git会自动帮你创建一个分支main，并且跟踪你的远程分支main  
或者你可以自己给你的分支取名字，然后执行
```shell
git branch -u origin/main
```
来跟踪远程main分支  
使用`git log`查看分支历史，会发现main分支会在初始分支下面，当然也有其他参数可以指定fetch的位置
#### origin/main
怎么理解它呢？  
可以把它看作一个标志位，在它上面提交不会导致它移动，但是对远程仓库的拉取会导致它移动，使它一直指向拉取时远程仓库最新的状态

### git pull && git pull --rebase
`git pull` = `git fetch` + `git merge`  
`git pull --rebase` = `git fetch` + `git rebase`  
它们用于将github环境拉取过来并且和本地环境整合

### git push
将自己的工作推送至远程仓库
```shell
git push origin master
```
你可以在你的github仓库中看到这些更改  
```shell
git branch -r
```
会发现同样添加了origin/master作为标志位  
当然也有参数可以指定push到的远程仓库的分支名和位置  
#### push rejected
当我们push到远程仓库的某个分支时候，可能远程仓库的分支相比于我们本地的做了一些修改，那么会出现push rejected现象  
在github上修改1.txt并提交  
在本地master分支下修改f4.c  
```shell
git checkout master
echo 666 > f4.c
git commit -am "edit f4.c"
```
然后
```shell
git push origin master
```
报错了，说明远程分支上有我们本地分支没有做的工作  
解决方案是在push之前先pull一下，把别人做的工作拉取到我们的本地目录来，再push
```shell
git pull origin master
git push origin master
```
#### 删除远程分支
```shell
git push origin --delete <branch>
```

## git 工具
市面上有许多git可视化工具，推荐两个：
- [GitHub Desktop](https://desktop.github.com/)
- [Git Fork](https://git-fork.com/)

## 结语
此篇笔记讲述的仅仅是git的冰山一角，git还有许多有趣的命令等待你们去挖掘  
不过你已经对git上手了，在实践中慢慢掌握git吧！

<p align='right'>to be continued</p>
<p align='right'>2023.9.27</p>

