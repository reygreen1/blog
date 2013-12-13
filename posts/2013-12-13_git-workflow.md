##Git工作流程
1. 远程克隆最新版本库。

	命令：`git clone 远程地址`

1.  尽量避免在master上修改代码，保证主干永远没有污染。
1.  新建分支。

	命令：`git checkout -b 分支名称`
	
1.  编码过程1，完成后提交代码到本地分支。

	命令：`git commit -a`

	`git commit -a` = `git add `+`git commit`（注意：不包含新增文件）
	
1.  编码过程2，完成后提交代码到本地分支。

	命令：`git commit -a`
	
1.  编码过程3，完成后提交代码到本地分支。

	命令：`git commit -a`
	
1.  可能需要合并多次提交的日志（同一任务只产生一条日志），合并日志。

	命令：`git rebase -i HEAD~3(提交次数)`
	
1.  分支上开发完成，push代码
	2. 更新master。
	
		命令：`git checkout master`+`git pull`
		
	2. 更新分支代码。
		
		命令：`git checkout 分支名称`+`git rebase master`
		
	2. 合并分支并push代码。
	
		命令：`git checkout master`+`git merge 分支名称`+`git push`
		
1. 提交完毕，删除分支。

	命令：`git branch -d 分支名称`