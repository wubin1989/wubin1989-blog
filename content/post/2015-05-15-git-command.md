---
title:       "git命令笔记"
subtitle:    ""
description: ""
date:        2015-05-15
author:      "wubin1989"
image:       ""
tags:        ["frontend"]
categories:  ["Tech" ]
archives:    "2015"
---

* 查看分支：git branch  
* 创建分支：git branch <name>  
* 切换分支：git checkout <name>  
* 创建+切换分支：git checkout -b <name>  
* 合并某分支到当前分支：git merge <name>  
* 删除分支：git branch -d <name>  
* 如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。  
* 查看远程库信息，使用git remote -v；  
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；  
* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；  
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；  
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；  
* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。  
* 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；  
* git tag -a <tagname> -m "blablabla..."可以指定标签信息；  
* git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；  
* 命令git tag可以查看所有标签。  
* 命令git push origin <tagname>可以推送一个本地标签；   
* 命令git push origin --tags可以推送全部未推送过的本地标签；  
* 命令git tag -d <tagname>可以删除一个本地标签；  
* 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。  
* git push origin :branch-name 冒号前面的空格不能少，原理是把一个空分支push到server上，相当于删除该分支。  
* git config --global alias.co checkout //设置checkout 命令别名    
* git config --global alias.br branch //设置branch 命令别名   
* git config --global alias.ci commit //设置commit 命令别名   
* git config --global alias.st status //设置status 命令别名   
* git config --global alias.last 'log -1 HEAD' //查看最后一次提交信息   
* git config --global alias.visual "!gitk" //启动gitk。运行外部命令，只需在命令前加上  