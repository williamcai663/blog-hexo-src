---
title: compiler error 解决办法
date: 2018-04-24 12:22:17
tags: 随笔
---
    编译项目，报Error:java: Compilation failed: internal java compiler error。
    原因是项目中jdk版本配置不一致，配成一致即可解决问题。

- 查看项目的jdk（Ctrl+Alt+shift+S）

	File ->Project Structure->Project Settings ->Project 
	
	![project_jdk](/assets/blogImg/project_jdk.JPG)

- 查看工程的jdk（Ctrl+Alt+shift+S）

	File ->Project Structure->Project Settings ->module 
	
	![module_jdk](/assets/blogImg/module_jdk.JPG)

- 查看idea中java配置（Ctrl+Alt+S）

	File ->Settings>Build,Execution,Deployment>Compiler>Java Compiler 
	
	![idea_jdk](/assets/blogImg/idea_jdk.jpg)

    报错的原因就在这里，idea软件环境中Java版本的配置和之前项目的配置不一样，设置成一样的即可解决该问题。