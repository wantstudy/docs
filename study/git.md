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
	**添加文件到仓库,文件可多选**

2. `git commit -m <message>` 
	**提交文件到仓库,message 为备注**

3. `git status` 
	**查看当前仓库状态**

4. `git diff <file>` 
	**查看文件修改的内容**



版本回退
-------
>**首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。**

1. 查看提交历史

`git log --pretty=oneline` 

2. 回退到上一版本

`git reset --hard HEAD^`

3. 回退指定版本

`git reset --hard <commit_id>`

4. 查看执行过的命令

`git reflog`