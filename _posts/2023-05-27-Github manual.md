---
title: Github常用指令操作手册🔥
date: 2023-05-27 08:50:00 +/-0800  
categories: [devtools, github]  
tags: [github,git command, development, git, tools]
author: seven_altman
img_path: /assets/img/devtools/github/
#image:
#  path: 5.jpg
#  alt: Github  
pin: true

---

![github light img](github-light.png){: .light }
![github dark img](github-dark.png){: .dark }

>你是否对Git命令还不够熟悉，或厌倦了到处查找相同的常用 Git 命令，这里有一个详细的常用命令清单供参考，可以涵盖日常开发中几乎全部的需求。  
{: .prompt-info }

本文主要包括以下主题：

- [设置 Git 常用属性](#设置-git-常用属性)  
- [初始化或克隆一个Git仓库](#初始化或克隆一个git仓库)  
- [修改并提交修改](#修改并提交修改)  
- [Git远程仓库概念](#git-远程仓库概念)  
- [分支功能](#分支功能)  
- [变基（rebase）](#变基rebase)  
- [与撤消相关的操作](#与撤消相关的操作)  
- [查看状态和提交记录](#查看状态和提交记录)  
- [临时存栈当前修改(stash)](#临时存栈当前修改stash)  
- [同步本地和远程仓库](#同步本地和远程仓库)  
- [附：打印常用命令挂起来！](#打印常用命令挂起来)

## 设置 Git 常用属性
设置全局user name 和email，若设置某个仓库则替换`--global`为`--local`

```shell
$ git config --global user.name "your name"
$ git config --global user.email "your_email@xxx.com"
```
可以用以下命令查看当前configs

```shell
git config -l
```

## 初始化或克隆一个Git仓库
创建本地仓库（省略<directory>将当前目录初始化为 git 仓库）

```shell
$ git init <directory>
```

下载远程仓库

```shell
$ git clone <url>
```
## 修改并提交修改
将文件添加到暂存区

```shell
$ git add <file>
```
暂存所有文件

```shell
$ git add .
```
将所有暂存文件提交到 git  

```shell
$ git commit -m "commit message"
```

添加对跟踪文件所做的所有更改并提交

```shell
$ git commit -am "commit message"
```

## Git 远程仓库概念
main : 默认开发分支  
origin : 默认上游 repo  
HEAD : 指向当前活跃分支的游标  
HEAD^ : HEAD时间序-1的位置  
HEAD~4 : HEAD时间序-4的位置  

## 分支功能
列出所有本地分支。添加-r标志以显示所有远程分支。-a所有分支的标志。 
 
```shell
$ git branch
```
创建一个新分支 

```shell
$ git branch <new-branch>
```
切换到分支并更新工作目录  

```shell
$ git checkout <branch>
```
创建一个新分支并切换到它  

```shell
$ git checkout -b <newbranch>
```
删除合并的分支  

```shell
$ git branch -d <branch>
```
删除分支，无论是否合并  

```shell
$ git branch -D <branch>
```
为当前提交添加标签（通常用于新版本发布）  

```shell
$ git tag <tag-name>
```
**合并分支**  
将 branch 合并a为 branch b。--no-ff为非快进合并添加选项

```shell
$ git checkout b
$ git merge a
```

将所有提交合并并压缩为一个新提交  

```shell
$ git merge --squash a
```

## 变基(rebase)
将功能分支重新设置到主分支上（以合并对主分支所做的新更改）。防止不必要的合并提交到功能中，保持历史清洁

```shell
$ git checkout feature
$ git rebase main
```

在 rebase 到 main 之前交互式地清理分支提交

```shell
$ git rebase -i main
```

交互式地重新设置当前分支上的最后 3 次提交

```shell
$ git rebase -i Head~3
```

## 与撤消相关的操作
移动（和/或重命名）文件和阶段移动

```shell
$ git mv <existing_path> <new_path>
```

从工作目录和暂存区中删除文件，然后暂存删除

```shell
$ git rm <file>
```

仅从暂存区移除

```shell
$ git rm --cached <file>
```

查看以前的提交（只读）

```shell
$ git checkout <commit_ID>
```
创建一个新的提交，从指定的提交中恢复更改

```shell
$ git revert <commit_ID>
```

返回上一个提交并删除它之前的所有提交（还原更安全）。添加--hard标志以同时删除工作区更改（非常小心）

```shell
$ git reset <commit_ID>
```

## 查看状态和提交记录
列出尚未提交的新文件或已修改文件

```shell
$ git status
```
列出带有相应 ID 的提交历史记录

```shell
$ git log --oneline
```
显示对未暂存文件的更改。对于暂存文件的更改，添加--cached选项

```shell
$ git diff
```
显示两次提交之间的变化

```shell
$ git diff commit1_ID commit2_ID
```

## 临时存栈当前修改(stash)
就是将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。  
**注意**：要包括未跟踪的文件，请添加-u标志。对于未跟踪和忽略的文件，添加-a标志。

```shell
$ git stash
```
同上，但添加注释。

```shell
$ git stash save "comment"
```
部分存栈。仅存储单个文件、一组文件或文件中的单个更改

```shell
$ git stash -p
```

列出所有存栈的文件

```shell
$ git stash list
```

只取出栈顶元素，但是不删除

```shell
$ git stash apply
```

重新应用索引 2 处的存储，然后将其从存储列表中删除。省略stash@{n}弹出最近的存储。

```shell
$ git stash pop stash@{2}
```

显示 stash 1 的差异摘要。传递标志-p以查看完整的差异。

```shell
$ git stash show stash@{1}
```

删除索引 1 处的存储。省略stash@{n}以删除最后创建的存储

```shell
$ git stash drop stash@{1}
```

清空栈

```shell
$ git stash clear
```

>stash使用场景一：本地修改后，发现远程分支已经被改动了，此时本地也被改动了就造成了冲突，无法push或者pull，此时可以：

```shell
git stash save "本地缓存内容标识" //把本地的改动暂存起来;
git pull //拉取远端分支（此时本地分支会回滚到上次commit的情况，新的改动都已保存在了stash中）;
git stash pop // 将栈顶改动内容重新加回本地分支，就可以继续修改了，当然，如果改好了就是add,commit,push操作。
```

>stash使用场景二：不小心改动了不该改动的分支,如本应该在dev分支开发的内容，却在master上进行了开发,可以用git stash将内容保存至堆栈中，切回到dev分支后，再次恢复内容即可

```shell
git stash save "本地缓存内容标识" // 把本地当前改动暂存起来，此时master分支就恢复到了上次拉取时的状态
git checkout test // 切换到需要改动的分支
git stash pop　　　 // 将改动pop到自己当前的分支
```


## 同步本地和远程仓库
添加远程仓库

```shell
$ git remote add <alias> <url>
```

查看所有远程连接。添加-v标志以查看 url。

```shell
$ git remote
```

删除连接

```shell
$ git remote remove <alias>
```

重命名连接

```shell
$ git remote rename <old> <new>
```
从远程仓库中获取所有分支（不合并）

```shell
$ git fetch <alias>
```
获取特定分支

```shell
$ git fetch <alias> <branch>
```
获取当前分支的远程仓库副本，然后合并

```shell
$ git pull
```
将您的本地更改移动（变基）到对远程仓库所做的新更改的顶部（为了干净、线性的历史记录）

```shell
$ git pull --rebase <alias>
```
上传本地内容到远程仓库

```shell
$ git push <alias>
```
上传到分支（然后可以拉取请求）

```shell
$ git push <alias> <branch>
```

## 打印常用命令挂起来!
![git cheat sheet dark](git_cheat_sheet_dark.png){: .dark}
![git cheat sheet light](git_cheat_sheet_light.png){: .light}

---
> 附1：图中可打印资源来源网页(Reference): [dev.to][refer1]  
> 附2：详细的git教程，廖雪峰老师讲的比较详细，[点击查看][refer2]


[refer1]: https://doabledanny.gumroad.com/l/git-commands-cheat-sheet-pdf
[refer2]: https://www.liaoxuefeng.com/wiki/896043488029600