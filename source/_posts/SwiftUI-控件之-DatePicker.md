---
title: SwiftUI 控件之 DatePicker
tags:
  - SwiftUI
  - DatePicker
  - Picker
categories:
  - SwiftUI 基础
abbrlink: b30b2594
date: 2023-09-25 09:09:28
---

在 SwiftUI 中，**Picker**可以帮助我们实现一些常见的选择器应用场景。除此之外，SwiftUI 还给我们提供了其他两种常见的选择器，**DatePicker（时间选择器）**和 **ColorPicker （选择器）**。



### 创建一个 DatePicker

**DatePicker**的创建和**Picker**创建一样，需要使用`@State`定义一个类型为`Date`的动态变量，然后将这个动态变量和`DatePicker`进行绑定：

```swift
 @State var selectedOptions:Date = Date()
```

```swift
DatePicker("选择日期", selection: $selectedOptions)
```

默认情况下的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309250930378.png" style="zoom:60%"/>

<!--more-->

默认情况下，我们可以选择日期和时间两种格式。我们也可以通过给`DatePicker`添加`displayedComponents`来配置可选项，例如：

```swift
 DatePicker("选择日期", selection: $selectedOptions, displayedComponents: [.date]) // 只可选择日期
```

或者

```swift
DatePicker("选择日期", selection: $selectedOptions, displayedComponents: [.hourAndMinute]) // 只可选择时间
```

以及

```swift
DatePicker("选择日期", selection: $selectedOptions, displayedComponents: [.hourAndMinute, .date]) // 默认均可选
```



不同样式的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309250936280.png" style="zoom:60%"/>

### 设置 DatePicker 的不同样式

**DatePicker**的样式可以通过`datePickerStyle`修饰器来进行设置，它提供了`compact`、`graphical`和`wheel`三种样式。

```swift
DatePicker("选择日期", selection: $selectedOptions, displayedComponents: [.hourAndMinute, .date]) // 默认均可选
                    .datePickerStyle(.graphical)
```

不同的样式如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309250948482.png" style="zoom:20%"/>

### DatePicker 本地化

默认情况下，**DatePicker** 以英文的形式进行显示，如果我们需要让它显示为中文，就需要进行**本地化**处理。本地化处理的流程就是给`DatePicker`添加一个`environment`修饰器，然后将本地的语言设置为中文即可，具体如下：

```swift
  DatePicker("选择日期", selection: $selectedOptions)
                .datePickerStyle(.graphical)
                .environment(\.locale, Locale.init(identifier: "zh"))
```

本地化前后对比：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309250959187.png" style="zoom: 20%"/>

<span style="color:red">**注意：**</span>在 **Xcode 15.0 Beta**中，并没有针对星期的显示进行本地化处理，无法确定是 **Xcode 15.0** 本身的问题还是最新的SwiftUI更新后的特性，在之前的的 **Xcode 14.0**中 本地化的处理也会对星期进行本地化，如下：



<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309251005729.png" style="zoom:20%"/>  



具体原因待后续观察！

