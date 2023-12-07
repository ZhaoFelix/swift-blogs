---
title: Reality Composer Pro之创建一个场景（一）
abbrlink: 5b8fb7ec
date: 2023-12-07 13:58:15
tags: [Reality Composer Pro, WWDC23]
categories: [Reality Composer Pro]
---

### 下载安装Reality Composer Pro 

在今年的**WWDC23**上，为了帮助开发者更好的开发**Version Pro**的应用，Apple 推出**Reality Composer Pro**。

>Apple 的官方介绍：
>
>Discover the all-new Reality Composer Pro, designed to make it easy to preview and prepare 3D content for your visionOS apps. Available with Xcode, Reality Composer Pro can help you import and organize assets, such as 3D models, materials, and sounds. Best of all, it integrates tightly with the Xcode build process to preview and optimize your visionOS assets.

截止目前，**Reality Composer Pro**已经可以通过最新的**Xcode 15.1 beta 3**中获取使用。我们只需要下载对应的Xcode 版本。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071415945.png" style="zoom:30%"/>

然后，在Xcode 的顶部菜单栏中点击**Xcode -> Open Developer Tools -> Reality Composer Pro**即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071419118.png" style="zoom:30%"/>

### 使用 Reality Composer Pro 创建项目或者场景

打开**Reality Composer Pro **后，有三个操作选项，分别是：

* **Create New Project**： 创建一个项目；
* **Create New Scene**：创建一个场景；
* **Open Existion Project or Scene**：打开一个已经存在的项目或者场景。



#### 创建项目

创建一个**项目**后，它的文件结构如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071426406.png" style="zoom:40%"/>

**Reality Composer Pro**初始项目如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071430250.png" style="zoom: 20%"/>

#### 创建一个场景

如果创建的是一个场景，那么它就是一个`.usda`的场景文件。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071434419.png" style="zoom:50%"/>

**Reality Composer Pro**的初始界面如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071436697.png" style="zoom: 20%"/>

### 开始构建一个场景

#### 素材导入

这里我们直接使用**WWDC23 **中**[认识 Reality Composer Pro](https://developer.apple.com/wwdc23/10083)**这个 Session 的素材来构建一个相同的场景。

直接到[这里]([Diorama | Apple Developer Documentation](https://developer.apple.com/documentation/visionOS/diorama))下载整个项目包，然后根据下面的目录找到对应的素材：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071447119.png" style="zoom:20%"/>

要导入素材，点击**Reality Composer Por**底部的素材导入按钮即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071451382.png" style="zoom: 50%"/>

导入后：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071456023.png" style="zoom:20%"/>

如果你想构建一个自己独特的场景，也可以使用**Reality Composer Pro**内置的**资源库**，点击右上角的➕按钮就能看到，

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071459026.png" style="zoom:20%"/>



### 构建底座

将刚刚导入的素材`Diorana_base.usdz`和`Yosemite.usdz`拖入到中间的场景区域中。注意，如果我们是分开单个拖入场景的话，需要我们分别调整它们二者的位置以达到最佳的组合效果。如果不想单独调整位置，可以按住键盘上的**Shift**键同时选中两个文件一同拖入到场景中，这样他们就会很好的组合在一起。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071522623.png" style="zoom: 20%"/>

接着我们可以给场景中的基座添加一个材质，让它看起来更加的真实。点击右上角的➕按钮，打开**资源库**，在左侧的分类选项卡中选择**Material Library(材质资源库)**，然后将我们需要材质拖入到场景中。例如这里我选择了**Smooth Concrete**材质效果。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071528860.png" style="zoom:20%"/>

接着，在左侧选中需要添加材质的节点，展开**Reality Composer Pro**右侧面板，在**Material Bindings**区域将**Binding**的材质修改为我们刚刚拖入的材质即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071531898.png" style="zoom:20%"/>

然后，是使用相同的方法，拖入三个`Location_Pin.usdz`到场景中，然后分别调整它们的位置到合适的位置。同时将它们分别重命名为`EI_Captian`、`Merced_River`和`Cathedral_Rocks`，选中三者，鼠标右键选择**Group**，将三者成组，组名为`Yosemite_Location_Pins`。最终，场景和左侧的节点层次结构如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202312071547082.png" style="zoom:30%"/>

### WWDC23 介绍视频

[认识 Reality Composer Pro](https://developer.apple.com/wwdc23/10083)

