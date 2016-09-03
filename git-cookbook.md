# Git Cookbook

## 获取分支
```sh
$ git checkout -b dev.0.1 origin/dev.0.1 
$ git checkout dev.0.1
```

## 日志输出更友好
```bash
$ git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
$ git config --global alias.lgl "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ci) %C(bold blue)<%an>%Creset' --abbrev-commit"
$ git config --global --unset alias.lg
$ git config --global --unset alias.lgl
```

## 怎样移除远程的提交日志
##### 示例数据如下
	```bash
	$ git lg -5
	* 728e3ed - (HEAD -> master, origin/master, origin/HEAD) test4 (26 minutes ago) <ximing>
	* c8f834f - (mywork) test3 (19 hours ago) <ximing>
	* 3992e9c - test2 (19 hours ago) <ximing>
	* abbe339 - test1 (19 hours ago) <ximing>
	* 2936159 - add bookmark (21 hours ago) <ximing>
	```
##### 方法1 

移除c8f834f之后的提交历史

	```bash
	$ git reset --hard 3992e9c
	$ git push origin -f  master
	```

##### 方法2  
1. 仅移除abbe339的提交，其他保留

	```bash
	$ git rebase -i 2936159
	error: could not apply 3992e9c... test2

	When you have resolved this problem, run "git rebase --continue".
	If you prefer to skip this patch, run "git rebase --skip" instead.
	To check out the original branch and stop rebasing, run "git rebase --abort".
	Could not apply 3992e9c9ac3a6e6bb64e0876dace165980990b45... test2
	```
2. 此时应当解决冲突，然后多次执行以下命令直至没有error: could not apply 3992e9c... test2此类错误为止

	```bash
	$ git status
	$ git add -A 
	$ git rebase --continue
	```
3. 提交到远程

	```bash
	$git push origin +master
	```

## 清除远程上某个文件的所有提交日志

``` bash
$ cd current directory
$ git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch 文件名' --prune-empty --tag-name-filter cat -- --all
$ git push origin master --force
$ rm -rf .git/refs/original/
$ git reflog expire --expire=now --all
$ git gc --prune=now
$ git gc --aggressive --prune=now
```

## 增加所有更改并提交

``` bash
$ git commit -a -m "提交备注"
```

## 取消远程某次或某几次提交(远程日志保留)

``` bash
$ git revert --no-commit b49eb8e 1d8b062
## 解决冲突后提交
$ git commit -a -m "Revert commits b49eb8e and 1d8b062"
```

## 取消本地文件修改
``` bash
$ git checkout file1.js file2.js
```


