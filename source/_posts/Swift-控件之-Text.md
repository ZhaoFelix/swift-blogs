---
title: Swift 控件之 Text
tags:
  - SwiftUI
  - Text
categories:
  - SwiftUI 基础
abbrlink: c90f7d11
date: 2023-09-04 14:01:33
---

### 介绍

在 SwiftUI 中，如果我们想要显示文本内容，可以使用 **Text** 控件。

### 使用方式

```swift
Text("Hello, world!")
```

### 常用的属性设置

#### 设置文字的颜色

通过`foregroundColor` 设置文字的颜色：

```swift
Text("Hello, world!")
     .foregroundColor(.black)
```

**<span style="color:red">注意</span>**，在 <span style="color:red"> Xcode 15.0</span>中，Apple 更推荐我们使用下面的方式设置文字的颜色，而且上面的修饰器将会在不久后被移除：

```swift
Text("Hello, world!")
    .foregroundStyle(.black)
```

<!--more-->

#### 设置字体的大小

通过 `font` 设置字体的大小：

```swift
Text("Hello, world!")
    .foregroundStyle(.black)
    .font(.title)
```

在上面代码中，我们直接使用了Apple 已经设计好的 **Dynamic Type sizes** 字体大小，详细介绍可以在 [《Human Interface Guidelines》]()中找到  [iOS, iPadOS Dynamic Type sizes](https://developer.apple.com/design/human-interface-guidelines/typography#iOS-iPadOS-Dynamic-Type-sizes)。

除了使用 Apple 设计的动态尺寸大小的字体，我们也可以使用下面的方式定义字体的大小：

```swift
.font(.system(size: 12))
```

#### 设置字重

设置字重可以使用 `fontWeight` :

```swift
.fontWeight(.bold)
```

如果只是对字体进行**加粗**操作，可以直接使用下面的**修饰器** ：

```swift
.bold()
```

**<span style="color:red">注意</span>** ，上面的两种设置字重的方式均只能在 <span style="color:red">iOS 16.0</span> 之后的系统中可用。

#### 设置字体的对齐方式

```swift
.multilineTextAlignment(.leading) // 多行文本左对齐 
```

* `.center`： 居中对齐；
* `.leading` ： 左对齐；
* `.trailing`： 右对齐。

#### 限制文本显示的行数

默认的情况下， **Text** 会完整的显示我们提供的字符串内容，在一些情况下我们需要限定最多所能显示的行数，可以使用`lineLimit`。

```swift
.lineLimit(2) // 最多只能显示两行
```

限制文本显示的行数后，多余的文本将会被隐藏，然后以 **...** 的方式结尾。

#### 设置内边距和行间距

当`Text` 中的文本内容超过多行的时候，我们可以使用`padding`和`lineSpacing`修饰器来设置文本的行间距和内边距。

```swift
 Text("Swift is a powerful and intuitive programming language for all Apple platforms. It’s easy to get started using Swift, with a concise-yet-expressive syntax and modern features you’ll love. Swift code is safe by design and produces software that runs lightning-fast.")
                .padding()
                .lineSpacing(10)
                .fontWeight(.bold)
```

#### 旋转字体

 SwiftUI 给我们提供了一个`rotateEffec` 修饰器来帮助我们简单的实现文本的旋转。

```swift
.rotationEffect(.degrees(45)) // 顺时针旋转 45°
```

默认情况下，将按照文本的中心作为锚点进行旋转，我们也可以指定旋转的锚点，例如：

```swift
.rotationEffect(.degrees(45),anchor: .topLeading) //以左上角的点作为锚点顺时针旋转 45°
```

`rotationEffect` 实现的是 2D 的旋转效果，SwiftUI 还提供了 `rotation3DEffect`修饰器来实现一个 3D 的旋转效果。

```swift
.rotation3DEffect(.degrees(60),axis: (x:1.0, y:0, z:0)) // 绕着 X 轴旋转 60°
```

#### 给文字添加阴影

如果想要给文字添加阴影，可以使用`shadow`修饰器。

```swift
.shadow(color:.gray, radius: 2,x: 0, y: 15)
```

`shadow`可以设置颜色、圆角、x轴和 y 轴多个参数。

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309301052661.png" style="zoom:10%"/>

#### 使用自定义的字体

在一下 app 中，我们可能想要使用自定义的字体而不是系统的字体。例如，我们想要使用https://fonts.google.com/specimen/Nunito 字体作为我们 app 的字体。那么我们就需要自定义 app 使用的字体。

首先，我们需要下载我们要使用的字体。一般情况下，字体都会提供多种样式，例如常规（regular）、加粗（bold）或者黑体（black）。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309301101260.png" style="zoom:20%"/>

我们可以根据自己的需要将指定样式的字体加入的到我们的app 项目中。例如，我们这里只需要使用常规（regular）字体，那么我将 **Nunito-Regular.ttf**这个文件拖入到项目中即可。拖入时，注意勾选下面的选项：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309301106752.png" style="zoom:30%"/>

然后在项目的**Info**配置选项的**Custom iOS Target Properties**中添加字体的配置项，如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309301110667.png" style="zoom:30%"/>

接着就是使用我们添加的字体：

```swift
.font(.custom("Nunito", size: 20))
```

此时我们的字体效果就出现了。但是需要注意的一点就是，因为我们只添加了常规样式的字体到我们的 app 中，所以即使我们给文本添加了`.fontWeight(.bold)` 加粗的修饰器，文本依然只显示常规样式，除非我们把加粗的字体也添加到app 中。

#### 展示 Markdown 格式的文本内容

Markdown 是一种轻量级的标记语言，被开发者们广泛使用。在 SwiftUI 中，**Text** 也支持将这个标记语言编辑的文本格式化为我们常见的文本内容。例如，

```swift
 Text("**这是一段使用markdown编辑的加粗文本内容**。\n 下面这是一个链接，如果使用模拟器运行点击后可以直接使用Safari浏览器打开。[Swift 博文](https://zhaofelix.github.io/swift-blogs)")
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309301122891.png" style="zoom:50%"/>

### 官方文档

[Text | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/text)

