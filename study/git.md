创建版本库
----

> __什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。__

_所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：_

	$ mkdir learngit
	$ cd learngit
	$ pwd
	/Users/michael/learngit

*第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：*

	$ git init
	Initialized empty Git repository in /Users/michael/learngit/.git/


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