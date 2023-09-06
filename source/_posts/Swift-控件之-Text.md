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
.multilineTextAlignment(.leading) // 左对齐 
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

### 官方文档

[Text | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/text)

