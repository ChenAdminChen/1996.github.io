---
title: svn command
date: 2018.06.27 14:36:45
reward: false
tags: 
    - svn
---

## svn commint new project

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以下的操作都在本地项目new_project文件夹内操作  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建新的project
>svn mkdir https://127.0.0.1/svn/new_project  //https://127.0.0.1/svn/为存放new_project项目的地方

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;检查new_project
>svn co https://127.0.0.1/svn/new_project

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;提交new_project
>svn import -m "提交new_project" https://127.0.0.1/svn/new_project

## svn 切换分支

>svn mkdir https://127.0.0.1/svn/new_project/0.5
>svn sw https://127.0.0.1/svn/new_project/0.5

## svn 合并分支

 
