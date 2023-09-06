---
title: 工欲善其事，必先利其器（Xcode 常见设置）
tags:
  - Xcode
categories:
  - Xcode 使用技巧
abbrlink: b2a2499d
date: 2023-09-06 17:46:07
---

# 

## Xcode 基础设置

### 设置代码编辑器的字体和整体样式

创建好项目之后，使用快捷 **`Command + ,`** 或者顶部菜单栏的 **Xcode→Settings** 打开Xcode的偏好设置，在 **Themes** 主题选项部分可以选择或者设置代码编辑器的代码风格和字体大小：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309061748945.png" style="zoom:30%"/>

<!--more-->

另外一个常用的设置是 **显示/隐藏** 文件的扩展名，也可以根据需要指定需要隐藏/显示 的文件类型：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309061749630.png" style="zoom:80%"/>

显示文件扩展名：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309061750955.png"/>

 隐藏文件扩展名：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309061751694.png"/>

### Xcode 整体布局设置

**显示/隐藏** SwiftUI项目的预览区：

在Xcode的右上角部分，点击从右往左数的第二个图标，**选中/取消选中** **Canvas  选项即可显示或关闭预览区：**

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309061752415.png" style="zoom:30%"/>

另一个比较常用的设置是代码区域的预览小地图，按照上面的方式勾选 **Minimap** 即可在代码编辑区域的右上角显示代码预览，当一个代码文件的代码行数很多时，我们可以通过快速点击预览到达指定的代码位置。

上面的设置是我们比较常用，同时也是能极大提升我们开发效率的设置，随着对Xcode越来越熟悉也可以逐渐了解Xcode中更多的使用技巧。

除了上面的一些基础设置，接着我们来介绍几个Xcode中常用的快捷键：

- `Command + B` ： 快速进行项目的编译，可以在不运行的情况下检查我们的代码编写是否存在错误；
- `Command + R` ： 在指定模拟下运行项目；
- `Command + N` ： 在项目中创建一个文件；
- `Command + Option + P` ：重新加载SwiftUI的预览界面。在默认情况下，我们在代码区域编写完成SwiftUI的代码后，右边的预览区域会自动进行加载，但是当我们的SwiftUI代码很多或者自动加载失败时就可以使用这个快捷方式重新快速加载预览而无须通过鼠标点击的方式进行。
