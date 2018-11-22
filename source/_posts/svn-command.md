---
title: svn command
date: 2018.06.27 14:36:45
reward: false
tags: 
    - svn
---

## svn commit new project

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以下的操作都在本地项目new_project文件夹内操作  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建新的project
```
//https://127.0.0.1/svn/trunk为存放new_project项目的地方
svn mkdir https://127.0.0.1/svn/trunk/new_project  

//检查new_project
svn co https://127.0.0.1/svn/trunk/new_project

//提交new_project
svn import -m "提交new_project" https://127.0.0.1/svn/trunk/new_project

```

>svn 切换分支

```
svn cp  https://127.0.0.1/svn/branch/old_project/ https://127.0.0.1/svn/branch/new_project

```

>svn 改名

```
svn mv https://127.0.0.1/svn/branch/old_project https://127.0.0.1/svn/branch/old_new_project

```

>svn 将未提交的代码切换到分支

```
svn cp . "^branch/new_project/0.6"
```

>svn 合并分支

```
分支上合并主干代码
3192：是指创建分支时主干版本号
3225：是指当前需要合并的主干版本号-
svn merge -r 3192:3225 https://192.168.1.168/svn/Yf_Server/trunk/new_project
```

>在分支查看处理的分支是否存在没有提交的代码
```
svn status 
```

>切回到主分支
```
svn sw https://127.0.0.1/svn/trunk/new_project
```

>合并分支代码,若存在问题时，需要合并，请按提示执行
```
svn merge https://127.0.0.1/svn/branch/new_project/0.5
```

>查看当前Branch中已经有那些改动已经被合并到Trunk中
```
svn mergeinfo https://127.0.0.1/svn/branch/new_project/0.5
```

>查看Branch中那些改动还未合并。
```
svn mergeinfo https://127.0.0.1/svn/branch/new_project/0.5 show-revs eligible
```

>检查是否真的合并成功，若出现问题可还原继续重复操作
提交合并的内容
```
svn ci -m "合并"
```

>删除项目
```
svn delete  https://127.0.0.1/svn/branch/new_project/0.5 -m delete 
```

>删除项目指定的文件(进入项目的目录下)
```
svn delete file_name
svn commit -m "delete 'file_name'"
```


