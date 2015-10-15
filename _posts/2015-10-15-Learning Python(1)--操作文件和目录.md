---
layout: post
title: Learning Python(1)--操作文件和目录
date: 2015-10-15
categories: blog
tags: [Python]
description: Keep learning~

---

#Keynote

1. os模块的重要函数或变量：
 
 - `os.name`，操作系统类型。(linux->posix,windows->nt)
 
 - `os.uname()`，详细系统信息。(windows不支持此函数)
 - `os.environ`，环境变量；os.environ.get('key')，获得某个环境变量的值。
操作文件和目录：
 - `os.path.abspath('.')`，查看当前目录的绝对路径。
 - `os.path.join('/Users/michael', 'testdir')`，在某个目录下创建一个新目录，首先把新目录的完整路径表示出来。
 - `os.mkdir('/Users/michael/testdir')`，然后创建一个目录。
 - `os.rmdir('/Users/michael/testdir')`，删除一个目录。
 - `os.path.split('/Users/michael/testdir/file.txt')`，拆分一个路径，后一部分总是最后级别的目录或文件名。
 - `os.path.splitext('/path/to/file.txt)`，获取文件的扩展名。
 - `os.rename('tset.txt','test.py')`，对文件重命名。
 - `os.remove('test.py')`，删除文件。
 过滤文件：
 - `os.listdir('.')`，列出当前目录下的所有目录和文件。
 - `os.path.isdir(x)`，x是否为目录。
 - `os.path.isfile(x)`，x是否为文件。

2. Exercis：

编写一个程序，能在当前目录以及当前目录的所有子目录下查找文件名包含指定字符串的文件，并打印出相对路径。
	
	-*— coding:utf-8 -*-
        import os
        def search_file(filename,path='.'):
	        content=os.listdir(path)
	        file_list=list()
	        dir_list=list()
	        for x in content:
		        if os.path.isdir(os.path.join(path,x)):
			        dir_list.append(x)
		        elif os.path.isfile(os.path.join(path,x)):
			        file_list.append(x)
	        for x in file_list:
		        if filename==x:
			        print(os.path.join(path,filename))
	        for x in dir_list:
		        search_file(filename,os.path.join(path,x))

        search_file('test.txt')
        
>os.path.isdir(os.path.join(path,x))

>os.path.isfile(os.path.join(path,x))

 ***os.path.isdir(x),x必须为一个目录***

参考：[廖雪峰博客--操作文件和目录](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431925324119bac1bc7979664b4fa9843c0e5fcdcf1e000)
