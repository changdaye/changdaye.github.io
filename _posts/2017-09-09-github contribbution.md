---
layout:     post
title:      github细节
subtitle:    "\"github contribbution 没记录\""
date:       2017-09-09
author:     mrchang
header-img: img/post-bg-swift.jpg
catalog: true
tags:
    - github
    - github contribbution
    - 小绿框
   
---

## 前言
1. 长期使用GitHub托管代码的小伙伴可能有发现，有时候写了几天的代码，但是发现contribbution（小绿框并没有点亮）
2. 那么什么样的commit才会被统计到呢，[这里从github help上找到了答案，英语不错的可以看下](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)

## 中文
1. Issues 和 pull requests
	* 这个操作是在一年之内
   * 这个操作是针对一个独立的仓库，不能是fork
2. Commits
  
   当你的commits满足以下条件时，它才会被展示出来：
   * 一年之内提交的commits
	* commits使用的email地址是与你的Github账号相关联的
	* 这些commits是在一个独立的仓库而不是fork仓库（博主就是这种错误，导致半个月的commit都没记录，才发现）
	* 这些commits是在：
		* 在默认分支上（通常是master）
		* 在gh-pages分支(包含 Project Pages sites 的仓库)
	* 此外，至少满足下面条件中的一个（主要针对你Commit的仓库不是你创建的）：

		* 你是这个仓库的协作者，或者是这个版本库的拥有组织中的一员
		* 你fork过这个仓库
		* 你对这个仓库发起过pull request或者issue
		* 你对这个仓库标记了Star
		
	注意：私有库的贡献仅仅对私有库成员显示
## Contributions未被Github计入的几个常见原因
	* 进行Commits的用户没有被关联到你的Github帐号中。
	* 不是在这个版本库的默认分支进行的Commit。
	* 这个仓库是一个Fork仓库，而不是独立仓库。（博主就是这种错误）
## 如何排查
	你可以在你的本地repo里用`git log`命令查看`commit`记录上的个人邮箱是否正确，像我就是因为之前切换到Mac平台开发之后用户名没有配置，所以我之后的commit记录上的邮箱一直是mrchang，所以Github就会认为这些commits都不是你提交的！
	
## 补救措施
然而这也并不是没有补救办法的，Github官网上就有给出详细的补救过程，英语好的同学请自行移步 [Changing author info](https://help.github.com/articles/changing-author-info/)，下面是我翻译自Github Help的简要步骤：

1. 变更作者信息
	* 为改变已经存在的 commits 的用户名和/或邮箱地址，你必须重写你 Git repo 的整个历史。
		 * 警告： 这种行为对你的 repo 的历史具有破坏性。如果你的 repo 是与他人协同工作的，重写已发布的历史是一种不好的习惯。仅限紧急情况执行该操作。

	
		使用脚本改变你 repo 的 Git 历史
		我们写了一段能把 commit 作者旧的邮箱地址修改为正确用户名和邮箱的脚本。
2. 使用脚本来改变某个repo的Git历史
	* 我们已经创建了一个脚本，使用正确的姓名和电子邮件地址提交后，你以前提交的所有的commits中的作者信息及提交者字段中的旧的用户名和邮箱地址都将被更正

		* 注意： 执行这段脚本会重写 repo 所有协作者的历史。完成以下操作后，任何 fork 或 clone 的人必须获取重写后的历史并把所有本地修改 rebase 入重写后的历史中。
	* 在执行这段脚本前，你需要准备的信息：

		Mac、Linux下打开Terminal，Windows下打开命令提示符（command prompt）
		
		给你的repo创建一个全新的clone
		
		git clone --bare https://github.com/user/repo.git
		
		cd repo.git
		
		复制粘贴脚本，并根据你的信息修改以下变量：旧的Email地址，正确的用户名，正确的邮件地址
				
				`
				#!/bin/sh
				
				git filter-branch --env-filter '
				
				OLD_EMAIL="旧的Email地址"
				
				CORRECT_NAME="正确的用户名"
				
				CORRECT_EMAIL="正确的邮件地址"
				
				if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
				
				then
				
   	 				export GIT_COMMITTER_NAME="$CORRECT_NAME"
   	 				
    				export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    				
				fi
				
				if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
				
				then
				
				    export GIT_AUTHOR_NAME="$CORRECT_NAME"
				    
				    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
				    
				fi
				
				' --tag-name-filter cat -- --branches --tags`
		1. 按 Enter键 执行脚本。
		
		2. 用git log命令看看新 Git 历史有没有错误
			
		3. 把正确历史 push 到 Github
		
			git push --force --tags origin 'refs/heads/*'
			
		4. 删掉刚刚临时创建的 clone
		
			cd ..
			
			rm -rf repo.git
		
3. 如何正确设置你的 git 个人信息
	* 接下来全局设置好你的正确信息，以后就放心的用Github进行版本管理吧
	* `git config --global user.email "你的邮件地址"`
	* `git config --global user.name "你的Github用户名"`
	

## 日常晒猫

![](https://cdn-blog.oss-cn-beijing.aliyuncs.com/17-9-9/31643841.jpg)

[原文链接](https://segmentfault.com/a/1190000004318632)	