安装配置
----
> **Windows安装**
	
_[安装下载](https://git-scm.com/downloads)_

_安装完成后进行以下配置_

   `git config --global user.name "Your Name"`

   `git config --global user.email "email@example.com"`
	

Git 项目/版本库管理
----
> __什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。__

> **工作区和暂存区**
	
	工作区: 是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区

	版本库:  工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，
	其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以
	及指向master的一个指针叫HEAD。

   ![Alt work_stage](../../_media/git/work_stage.jpg)

> **下载已有项目**

	git clone git@github.com:wantstudy/docs.git

> **上传本地项目到`Github`**

	git init 初始化目录为git项目
	git add <files> 添加文件到仓库,文件可多选
	git commit -m 'mark'  提交文件到本地仓库
	git push -u origin master 提交文件到远程仓库，初次提交 -u  

> **本地项目与远程项目关联**
	
	git remote add origin git@github.com:wantstudy/docs.git

Git 常用操作命令
-----

1. `git add <file>`
	要提交的所有修改放到暂存区（Stage）,文件可多选

2. `git commit -m 'message'` 
	一次性把暂存区的所有修改提交到分支,message 为备注

3. `git status` 
	查看当前仓库状态

4. `git diff` 
	比较工作区与暂存区的差异

		git fetch origin  更新本地的远程分支

		git log master..origin/master   本地与远程的差集 :（显示远程有而本地没有的commit信息）
		
		git diff <local branch> <remote>/<remote branch>	统计文件的改动

5. `git checkout -- file`

		readme.txt自修改后还没有被放到暂存区,撤销修改就回到和版本库一模一样的状态

		readme.txt已经添加到暂存区后，又作了修改，撤销修改就回到添加到暂存区后的状态



版本回退
-------
>**首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。**

1. 查看提交历史

`git log --pretty=oneline` 

`git log --graph --pretty=oneline --abbrev-commit`

2. 回退到上一版本

`git reset --hard HEAD^`

3. 回退指定版本

`git reset --hard <commit_id>`

4. 查看执行过的命令

`git reflog`



创建/切换分支
-----

   查看分支：`git branch`

   查看分支详细信息：`git branch -v`

   创建分支：`git branch <dev>`

   切换分支：`git checkout <dev>`

   创建+切换分支：`git checkout -b <name>`

   创建远程分支：`git push origin <dev>`

   合并某分支到当前分支：`git merge <dev>`

   查看合并后冲突： `git status`

   删除本地分支：`git branch -d <dev>`

   删除远程分支：`git push origin --delete <dev>`


