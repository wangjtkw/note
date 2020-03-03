# Git命令

## git add <文件名>

添加文件

![git_add](C:\Users\TKW\Desktop\note\git_pic\git_add.png)

## git commit -m"描述"

添加描述

![git_commit](C:\Users\TKW\Desktop\note\git_pic\git_commit.png)

## git log

显示日志描述

![git_log](C:\Users\TKW\Desktop\note\git_pic\git_log.png)

## git log --pretty=oneline

简易显示描述

![git_log_pretty](C:\Users\TKW\Desktop\note\git_pic\git_log_pretty.png)

## git status

显示当前版本控制的状态

![git_status](C:\Users\TKW\Desktop\note\git_pic\git_status.png)

## git diff

显示当前文件与已commit文件的差异

![git_diff](C:\Users\TKW\Desktop\note\git_pic\git_diff.png)

## git reset --hard HEAD^

版本回退

一个^代表回退一个版本，两个^则回退两次，以此类推

可使用~<num> 来回退多个版本，例如git reset --hard HEAD~100

git reset --hard <版本号> 可跳到指定版本号的版本

## git reflog

可查看每个操作的日志

![git_reflog](C:\Users\TKW\Desktop\note\git_pic\git_reflog.png)

## git remote add origin <SSH>

关联远程库

![git_remote](C:\Users\TKW\Desktop\note\git_pic\git_remote.png)

## git push -u origin <分支名>

将本地仓库的内容传到远程仓库

![git_push](C:\Users\TKW\Desktop\note\git_pic\git_push.png)

