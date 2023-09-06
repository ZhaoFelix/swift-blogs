---
title: SwiftUI实践之创建一个待办清单（一）
tags:
  - SwiftUI
  - 待办清单
categories:
  - 使用SwiftUI创建一个待办清单
abbrlink: 5b45bb70
date: 2023-09-05 13:47:17
---

# 项目创建和基础配置说明

## 创建项目

1、打开Xcode

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051352356.png" style="zoom:30%"/>

Xcode的初始界面存在三种不同的项目创建方式：

**Create a new Xcode project**: 创建一个全新的工程项目，最常用的项目创建方式；

**Cone an existing project**: 克隆一个项目，适用于已经存在并且使用Git或SVN版本管理工具管理的项目；

**Open a project or file**： 打开一个本机已经存在的项目或文件。

<!--more-->

2、选择第一个，创建一个全新的Xcode项目：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051354165.png" style="zoom:15%"/>

首先需要注意是，我们可以创建不同平台的应用项目，例如<span style="color:red"> iOS、macOS、watchOS、tvOS</span> 或者是同时运行多个平台的项目。

这里我们选择 **iOS**，因为我们的应用只需要运行在iPhone或者iPad上。

针对我们常见的APP开发，我们有两个选项，分别是 **App** 和 **App Playground。** 二者的区别在于：

- **App** 选项创建的项目是一个工程文件，包含多个文件； **App Playground** 选项创建的项目是一个扩展名为 `.swiftpm` 的文件；
- **App** 选项创建的项目**只能**通过Mac电脑上的Xcode编译运行和二次开发；**App Playground** 选项创建的项目可以使用Xcode、iPad或者Mac上的 **Swift Playgrounds** 应用程序运行和二次开发。
- **App** 选项创建的项目项目更加灵活，适用于大型项目或者多人协同开发；**App Playground** 选项创建的项目适用于个人或者小型软件开发；

细心的同学可能已经注意都一点，**App Playground** 选项创建的项目和在iPad上使用**Swift Playgrounds**创建的项目是一样的。

> 其他的一些项目创建选项主要用于一些特殊的应用开发，例如开发游戏、开发一个AR的应用以及开发软件包等。赶兴趣的同学可以自行了解。

选择 **App** 选项，点击 **Next**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051356228.png" style="zoom:30%"/>

项目的配置信息：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051357254.png" style="zoom:50%"/>

**Product Name**: 项目名称或者工程名称；

**Team**:  开发团队名称，即开发者的Apple ID；

**Organization Identifier**: 组织ID，一般为公司域名倒过来。例如正式域名为 [www.qingmaoedu.com](http://www.qingmaoedu.com) 倒过来就是 `com.qingmaoedu.www `；

**Bundle Identifier**: Xcode 使用组织名+项目名自动为应用生成的一个标识ID，这个ID在进行应用上架时会非常关键，也可以后续在Xcode中自行修改。需要注意的一点是，这个ID在App Store 中相当于应用的身份证号，且是唯一存在的；

**Interface**: 应用开发要使用的技术框架，存在两个选项 **SwiftUI** 和 **Storyboard，**前者使用SwiftUI 作为主要的技术框架，后者使用UIKit作为主要的技术框架。当然这二者在后续的项目中可以混合使用。UIKit 是SwiftUI推出之前，开发iOS应用的唯一开发框架，用来实现界面的构建；

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051359529.png" style="zoom:100%"/>

**Language** : 开发语言，有 **Swift** 和 **Objective-C** 两种开发语言；

**Use Core Data** : 项目是否使用 Core Data 框架；Core Data 是Apple提供的数据存储框架，可以实现本地存储和云存储；

**Include Tests** : 是否包含测试到项目中，项目开发完成后进行功能和UI测试的时候会用到来编写测试用例。

最终项目配置如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051401234.png" style="zoom:70%"/>

点击 **Next** 选择一个项目文件的存储位置，

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051401477.png" style="zoom:40%"/>

<span style="color:red">**注意**</span>，这里有一个是否使用 Git 来对项目做一个版本管理的选项，如果我们熟悉Git和GitHub的使用或者这个项目需要多个人共同开发可以进行勾选。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051403037.png"/>



## 项目结构介绍和基础配置

1、项目结构

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051403741.png"/>

项目创建后，默认下的两个 Swift文件和我们之前使用 **Swift Playgrounds** 创建的一致。我们需要注意的是一个蓝色资源文件夹 **Assets** ，这个文件用存放我们项目中所有的素材 资源，例如图片、音视频、颜色数值等。

2、基础配置

选中项目的根目录，在 **TARGETS→General** 中可以找到项目的一些通用的基础配置：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051403718.png" style="zoom:30%"/>

在这个基础配置页面，我们比较常用的配置包括：

**Supported Destinations** : 项目支持的硬件平台。我们可以在这里使用应用支持运行的平台，例如iPhone、iPad、Mac 或者Apple TV。

**Minimum Deployments** : 项目最低支持的运行系统版本。例如这里显示的iOS 16.4 意味着如果我们不进行修改，那么我们开发完成的项目将无法在低于 iOS 16.4 的系统上运行。

**Identify** ： 应用的标识。

- **App Category** : 使用SwiftUI创建的项目可以在这里设置一个应用的默认图标；

- **Display Name** : 应用展示在桌面的名称，例如我们手机上常见的“淘宝”、“抖音”、“支付宝”等等，如果不进行设置默认为我们创建时设置的项目包名；

- **Bundle Identifier** : 应用的标识ID，默认自动生成，我们也可以进行修改，需要注意的是应用上架审核时这个ID必须是唯一存在的；

- **Version** : 应用版本号；

- **Build**: 应用编译版本号。

  版本号和编译版本后在应用上架审核时用的比较多，平时基本不需要进行修改。

**Deployment Info**: 配置应用在设备上运行时可以支持的设备方向和状态栏默认的样式。

关于设备的方向：

- **Portrait :**  设备与地面垂直，Home 键位于底部或者面容识别位于顶部；
- **Upside Down** ：设备与地面垂直，Home 键位于顶部或者面容识别位于底部；
- **Landscape Left ：**设备的面容识别位于右侧或者Home 键位于左侧；
- **Landscape Right ：**设备的面容识别位于左侧或者Home 键位于右侧；

> API开发文档：[UIDeviceOrientation | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uideviceorientation)



打开 `ContentView.swift` 文件，同时打开预览界面，点击预览界面从左往右数的第三个选项按钮，然后打开 **Orientation** 的选项开即可调整预览界面中设备的不同方向。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051404012.png" style="zoom:40%"/>



在刚刚的**Deployment Info** 配置中，除了可以设置应用运行后的方向，也可以设置应用中顶部状态栏的样式:

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051405925.png"/>

- **Default** ：默认样式，根据页面的样式进行选择；
- **Dark Content** : 状态栏背景为白色，文字和图标为黑色，即白底黑字；
- **Light Content** :  状态栏背景为黑色，文字和图标为白色，即黑底白字；



## 配置项目和演示

在 **Xcode**的顶部选择一个我们指定运行的模拟器：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051406328.png" style="zoom:40%"/>

如果没有想要的模拟器，可以点击底部的 **Manage Run Destinations** 选项去添加不同系统版本和型号的模拟器：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051406713.png" style="zoom:30%"/>

在后面的课程中，我们也将使用WiFi或者数据线的方式将应用运行在我们的手机上，即真机测试。

选中模拟器后，点击底部类似于播放▶️的按钮运行我们的项目，习惯使用快捷键的同学也可以使用 **`Commond + R`** 快捷键运行项目**。**

不修改任何配置的情况下，运行的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051407884.png" style="zoom:50%"/>

点击模拟器顶部的 **Home** 键回到桌面，我们可以看到应用使用了一个默认的白色线框图标以及应用的名称为我们的项目名称。 

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051407389.png"/>

需要注意的一点是，当我们通过上面介绍的这些设置去配置项目时，会发现存在以下的一些问题：

- 设置 **App Category** 和 **Status Bar Style** 都没有出现我们预期的效果。但是我们可以使用其他的方式去设置这个选项，例如在资源文件中添加我们自己的应用图标到 **AppIcon** 里面可以设置应用的图标，后续我们可以使用代码的方式去设置状态栏的样式。
- 当我们只给应用设置一个方向时，无论后续如何旋转我们的设备，界面布局都不会发生变化。即我们可以通过指定应用只能竖屏或者横屏的模式下运行，这样一来我们在开发的使用就不需要考虑不同屏幕方向下的界面布局了。
