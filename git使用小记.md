title: Git使用小记
date: 2014-08-13 17:21:00
categories: linux
tags: [linux,git,代码管理]
desctiption: 代码管理git使用教程

---

###前言：

我们写代码写完后总要进行管理，以前写的很多代码虽然写的不是很好，但因为没有一个比较好的代码管理习惯，所以很多都遗失掉了，为此现在都还觉得很可惜，近来在学习使用git来进行代码管理，git是一个很强大的分布式版本控制系统。

![git]({{BASE_PATH}}/image/git流程.png)
###git工作流程
了解git，首先要弄清楚对象在被git管理过程中所处的4个阶段，分别是：工作目录、index(又称为暂存区)、本地仓库和远程仓库。<!--more-->从时间先后来讲，工作目录的内容是你当前看到的，也是最新的；index区标记了你当前工作目录中，哪些内容是被git管理的；而本地仓库保存了对象被提交过的各个版本，比起工作目录和暂存区的内容，它要更旧一些；远程仓库是本地仓库的异地备份，远程仓库的内容可能被分布在多个地点的处于协作关系的本地仓库修改，因此它可能与本地仓库同步，也可能不同步，但是它的内容是最旧的。任何对象都是在工作目录中诞生和被修改；任何修改都是从进入index区才开始被版本控制；只有把修改提交到本地仓库，该修改才能在仓库中留下痕迹；而要与协作者分享本地的修改，可以把它们push到远程仓库来共享。图最上方的add、commit、push等，展示了git仓库的产生过程。反过来，我们可以从远程历史仓库中获得本地仓库的最后一个版本，clone到本地，从本地检出对象的各个版本到index暂存区或工作目录中，从而实现任何对象或整个仓库的任意阶段状态的”回滚”。当正向和反向都能自由切换后，git就强大到无所不能了。

###远程仓库操作命令

* 从远程仓库克隆仓库到本地：$ git clone git://github.com/******.git

* 查看远程仓库别名以及地址：$ git remote -v

* 添加远程仓库：$ git remote add [name] [url]

* 删除远程仓库：$ git remote rm [name]

* 修改远程仓库：$ git remote set-url --push [name] [newUrl]

* 拉取远程仓库：$ git pull [remoteName] [localBranchName]

* 推送远程仓库：$ git push [remoteName] [localBranchName]

* 提交本地test分支作为远程的master分支：$git push origin test:master

* 提交本地test分支作为远程的test分支：$git push origin test:test


###分支操作命令--branch

* 查看远程和本地所有分支：$ git branch -a

* 创建本地分支：$ git branch [name]

* 切换分支：$ git checkout [name]

* 创建新分支并且切换到新的分支：$ git check -b [name]

* 复制远程的分支并且以此来创建新分支：$ git check -t /remotes/origin/branch1  //这样就能在本地创建一个复制来自远程branch1分支

* 删除分支：$ git branch -d [name]  //-d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项

* 合并分支：$git merge [name]  //将名称为[name]的分支与当前的分支合并

* 创建远程分支：(本地分支push到远程)：$ git push origin [name]

* 删除远程分支：$ git push origin :heads/[name] 或 $ git push origin :[name]


###版本(tag)操作相关命令

* 查看版本：$ git tag

* 创建版本：$ git tag [name]

* 删除版本：$ git tag -d [name]

* 查看远程版本：$ git tag -r

* 创建远程版本(本地版本push到远程)：$ git push origin [name]

* 删除远程版本：$ git push origin :refs/tags/[name]

* 合并远程仓库的tag到本地：$ git pull origin --tags

* 上传本地tag到远程仓库：$ git push origin --tags

* 创建带注释的tag：$ git tag -a [name] -m 'yourMessage'

###子模块(submodule)相关操作命令

* 添加子模块：$ git submodule add [url] [path]

* 初始化子模块：$ git submodule init ----只在首次检出仓库时运行一次就行

* 更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下

* 删除子模块：（分4步走哦）

    1. $ git rm --cached [path]

    2. 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉

    3. 编辑“ .git/config”文件，将子模块的相关配置节点删除掉

    4. 手动删除子模块残留的目录

* 忽略一些文件、文件夹不提交

    * 在仓库根目录下创建名称为“.gitignore”的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如

```shell
        target
        
        bin
        
        *.db
```

* 删除缓存区的文件

    * 不怎么进行删除操作，所以就常用这一个命令：$ git rm -r --cached .



###一般的代码管理流程：


1. 远程已经建完仓库后直接克隆到本地： $ git clone [仓库地址]

2. 查看本地和远程的branch：$ git branch -a

3. 如果远程的branch更新了的话运行：$ git fetch -p      //这样会将远程的分支列表重新拉取

4. 将远程的branch复制到本地并且切换到这个分支下面：$ git checkout -t [远程分支名字]

5. 这是看到自己已经在新建的branch下面了，然后看一下有没有文件。

6. 若没有的话试一下：$ git pull

7. 做完修改以后的话先add：$ git add .      //这样会将除了在.gitignore中写入的文件都加入进要提交的文件内

8. 然后进行提交到本地仓库：$ git commit - m "提交"

9. 可以查看状态：$ git status 或者 $git diff

10. 如果没什么问题的话就进行提交到远程库： $ git push     //如果加入-f的话是强制提交。

