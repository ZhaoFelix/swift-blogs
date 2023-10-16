---
title: Xcode必备技巧（一）
tags:
  - Xcode
  - 快捷键
categories:
  - Xcode 使用技巧
abbrlink: 3249ee52
date: 2023-10-15 09:58:27
---



### Xcode 中常用的快捷键

**1. Command + 0**

显示/隐藏左侧文件导航栏

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151015899.gif" style="zoom: 30%"/>

**2. Command + Shift + O** 

<!--more-->

打开有一个快捷搜索，通过关键字快速定位到对应的文件、函数或者变量。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151044891.gif" style="zoom:30%"/>

**3. Command + Shift + J**

快速定位当前文件的所在的位置。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151047955.gif" style="zoom:30%"/>

**4. Command + Shift + Y** 

快速显示/隐藏Xcode 底部的调试区域，即输出控制台部分。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151048412.gif" style="zoom:30%"/>

**5. Command + Option + [**

光标所在行的代码向上移动。

**6.Command + Option + ]**

光标所在行的代码向下移动。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151050647.gif" style="zoom:30%"/>



**7. Command + Option + 0** 

显示/隐藏右侧区域。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151058674.gif" style="zoom:30%"/>

**8. Control + I**

光标所在行的代码进行随进对齐。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151101493.gif" style="zoom:30%"/>

经常和**Command + A** 一起使用，即全选当前文件的所有代码，然后对所有的代码进行美化操作。



### Xcode 中的多光标编辑

Xcode 和很多的其他开发工具一样，也支持多光标的编辑操作。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151108330.gif" style="zoom:80%"/>

Xcode 中有多种添加多光标的方式：

**方式一：Control + Shift + 单击鼠标左键**。

**方式二： Control + Shift + 方向上下键**<span style="color:red">**(推荐方式)**</span>。

### Xcode 中开启拼写检查

在开发过程中，我们会经常遇到各种命名拼写错误的情况，为了尽量避免这个情况的出现，Xcode 给我们提供了一个**拼写检查**的功能。

开启路径如下：

Xcode 左上角的菜单栏中**Edit -> Format -> Spelling and Grammar -> Check Spelling While Typing**， 勾选住这个设置选项即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151138115.png" style="zoom:20%"/>

开启前的效果：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151145517.png" style="zoom:30%"/>

开启后的效果：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151145166.png" style="zoom: 30%"/>

当出现拼写错误的时候，可以选中这个拼写错误的单词，然后鼠标右键就可以看到 Xcode 给我们提供的修改建议和处理方法了。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310151146632.png" style="zoom:20%"/>
