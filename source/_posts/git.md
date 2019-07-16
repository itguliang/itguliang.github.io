---
title: Git 命令整理
date: 2019-06-30
categories:
  - IT
tags:
  - Git
abbrlink: ef906deb
toc: true
---

## git config

`git config --list` : 检查配置信息

`git config <key>` : 检查 Git 的某一项配置

`git config --global --edit` : 编辑配置文件

`git config --global user.name "itguliang"` : 全局配置用户名

`git config --global user.email johndoe@example.com` : 全局配置用户邮箱

`git config --global --unset user.name` : 删除配置信息

`git config --global --unset user.email` : 删除配置信息


## git clone

`git clone <版本库的网址>` : 与远程主机的版本库同名

`git clone <版本库的网址> <本地目录名>` : 指定不同的目录名

`git clone -o <其他的主机名> <版本库的网址>` : 指定用其他主机名

克隆版本库远程主机自动被Git命名为`origin`。如果想用其他的主机名，`git clone`命令的`-o`选项指定。



## git remote

为了便于管理，Git要求每个远程主机都必须指定一个主机名。

`git remote` : 列出所有远程主机

`git remote -v` : 查看远程主机的网址

`git remote show <主机名>` : 查看该主机的详细信息

`git remote add <主机名> <网址>` : 添加远程主机

`git remote rm <主机名>` : 删除远程主机

`git remote rename <原主机名> <新主机名>` : 远程主机的改名



## git branch

`git branch -a` : 查看远程分支

`git branch` : 查看本地分支

`git branch branchName` : 创建分支

`git branch -d branchName` : 删除本地分支



## git fetch

`git fetch <远程主机名>` : 将某个远程主机的更新，全部取回本地

`git fetch <远程主机名> <分支名>` : 只取回特定分支的更新
```bash
$ git fetch origin master #取回origin主机的master分支
```


## git pull

`git pull <远程主机名> <远程分支名>:<本地分支名>`
取回远程主机某个分支的更新，再与本地的指定分支合并
```bash
$ git pull origin master:dev #取回origin主机的master分支，与本地的dev分支合并
$ git pull origin master #取回origin/master分支，再与当前分支合并。实质上等同于先git fetch，再git merge
$ git fetch origin
$ git merge origin/master
```


## git status

`git status` : 检查当前文件状态输出十分详细

`git status -s` : 简短输出



## git diff 
查看具体修改了什么地方
`git diff` : 尚未缓存的改动

`git diff --stat` : 显示摘要而非整个diff：

`git diff --cached` : 查看已缓存的改动

`git diff HEAD` : 查看已缓存的与未缓存的所有改动


`git diff --staged`



## git commit

每次准备提交前，先用 git status 看下，是不是都已暂存起来了， 然后再运行提交命令 git commit

`git commit -a -m 'added new benchmarks'` 跳过暂存

`git commit --amend --no-edit` push前 追加提交 不修改提交说明



## git rm

`git rm log/\*.log` : 删除 log/ 目录下扩展名为 .log 的所有文件

`git rm \*~` : 删除以 ~ 结尾的所有文件。

`git rm -f <file>` : 删除之前修改过并且已经放到暂存区域的文件，加 -f

`git rm --cached <file>` : 如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，加 --cached



## git tag

`git tag` : 列出标签

`git tag tagName` : 添加标签

`git tag -d tagName` : 删除标签

`git push origin tagName` : 提交标签

`git push origin :refs/tags/v0.1` : 删除远程标签 v0.1



## git mv

用于移动或重命名一个文件、目录、软连接

`git mv file_from file_to`

## git merge

--no-ff：不使用fast-forward方式合并，保留分支的commit历史
--squash：使用squash方式合并，把多次分支commit历史压缩为一次

`git merge --ff` : fast-forward方式就是当条件允许的时候，git直接把HEAD指针指向合并分支的头，完成合并。属于“快进方式”，不过这种情况如果删除分支，则会丢失分支信息。因为在这个过程中没有创建commit

`git merge --squash` : 是用来把一些不必要commit进行压缩，比如说，你的feature在开发的时候写的commit很乱，那么我们合并的时候不希望把这些历史commit带过来，于是使用--squash进行合并，此时文件已经同合并后一样了，但不移动HEAD，不提交。需要进行一次额外的commit来“总结”一下，然后完成最终的合并

```bash
# dev分支的修改合并到master主分支步骤
$ git checkout master    #切换到master主分支
$ git pull origin master #更新代码
$ git merge dev --no-ff  #dev合并到主分支
$ git push origin master #提交到主分支
$ git branch -d dev      #删除本地dev分支
$ git push origin --delete dev  #git branch -a 查看远程分支，删除远程dev分支

```

## git push

`git push <远程主机名> <本地分支名>:<远程分支名>` : 将本地分支的更新，推送到远程主机



## git reset
`git reset HEAD` : 取消之前 git add 添加，但不希望包含在下一提交快照中的缓存
`git reset --mixed` : 头指针恢复，add的缓存也会丢失掉，工作空间的代码不变
`git reset --soft` : 头指针恢复，add的缓存不变，工作空间的代码不变。如果还要提交，直接commit即可
`git reset --hard` : 头指针恢复，aad的缓存消失，本地的源码也会变为上一个版本的内容，彻底回退到某个版本


## git stash

`git stash` : 会把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录。

`git stash save "test-cmd-stash"` : 实际应用中推荐给每个stash 加个msg

`git stash list` : 查看现有stash

`git stash drop stash@{0}` : 移除stash@{0}

举个🌰：
```bash
$ git status
On branch develop
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   package.json

no changes added to commit (use "git add" and/or "git commit -a")

$ git stash save "暂时保存"
Saved working directory and index state On develop: 暂时保存

$ git status
On branch develop
nothing to commit, working tree clean

$ git stash list
stash@{0}: On develop: 暂时保存

$ git stash apply
On branch develop
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   package.json

no changes added to commit (use "git add" and/or "git commit -a")
```
https://blog.csdn.net/flysqrlboy/article/details/79250150


## 常见问题解决

* 修改本地和远程分支的名称
```bash
#本地分支重命名(还没有推送到远程)
$ git branch -m oldName newName  
#远程分支重命名 (已经推送远程-假设本地分支和远程对应分支名称相同)
$ git branch -m oldName newName #重命名本地分支
$ git push --delete origin oldName #删除远程分支
$ git push --set-upstream origin newName #把修改后的本地分支与远程分支关联
```

* 撤销add
```bash
$ git reset HEAD . #撤销所有add文件 
$ git reset HEAD -filename #撤销单个add文件 
```


* 撤销commit 未push
```bash
$ git log #找到想要撤销的id 
$ git reset --soft <commit版本号> #撤销，但不对代码修改撤销，可通过git commit 重新提交对本地代码的修改
```


* 撤销commit 已经push
```bash
$ git log
$ git reset --hard <commit版本号> #完成撤销,同时将代码恢复到前一commit_id 对应的版本 
$ git push <远程主机名> <本地分支名>:<远程分支名> --force
#要加上force 不然会提示
#error: failed to push some refs to '地址'
#hint: Updates were rejected because the tip of your current branch is behind
```


* Git也允许手动建立追踪关系
```bash
#指定dev分支追踪origin/master分支
$ git branch --set-upstream dev origin/master 
#当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名
$ git pull origin
```
	

* 忽略某个被追踪的文件的修改
```bash
#如果文件已经被跟踪，再放到.gitinore可能会失效，用以下命令来忽略
$ git update-index --assume-unchanged your_file_path 
#撤销用：
$ git update-index --no-assume-unchanged your_file_path 
```


* 把指定的dist文件提交到gh-pages分支上
```bash
$ git subtree push --prefix=dist origin gh-pages 
```


* git log 如何退出操作

	按 `Q` 键


* Git Push 避免用户名和密码方法
http://www.cnblogs.com/ballwql/p/3462104.html
```bash
#全局配置
$ git config --global user.name "username" 
$ git config --global user.email "email"
$ git config --global credential.helper store
#输入这个命令后,只需要输入一次用户名密码
```

* 设置只有自己需要忽略的文件

	修改.git/info/exclude文件


* 常用命令图
（右键新标签页打开，放大查看，图转自文章 http://www.cnblogs.com/1-2-3/archive/2010/07/18/git-commands.html ）

{% qnimg ef906deb-1.png %}



相关链接：
[Git简明指南](http://www.runoob.com/manual/git-guide/)
Git 完整命令手册地址：http://git-scm.com/docs
PDF 版命令手册：http://www.runoob.com/manual/github-git-cheat-sheet.pdf
https://git-scm.com/book/zh/v2
http://www.runoob.com/git/git-basic-operations.html
http://www.ruanyifeng.com/blog/2014/06/git_remote.html
https://www.cnblogs.com/cheneasternsun/p/5952830.html
