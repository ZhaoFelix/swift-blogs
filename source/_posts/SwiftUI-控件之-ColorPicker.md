---
title: SwiftUI 控件之 ColorPicker
abbrlink: d1bc9d76
date: 2023-09-26 14:26:51
tags: [SwiftUI, ColorPicker, Picker]
categories: [SwiftUI 基础]
---

在 SwiftUI 中，除了常见的**Picker**和**DatePicker** ，还有一个**ColorPicker（颜色选择器）**，它在我们需要进行颜色选择时非常有用。

### 创建一个颜色选择器

首先，使用`@State`创建一个`Color`类型的变量作为`ColorPicker`选中绑定值：

```swift
@State var selectedOptions:Color = .red
```

创建`ColorPicker`：

```swift
 ColorPicker(selection: $selectedOptions, label: {
             Text("选择你最喜欢的颜色")
            })
```

运行项目，点击的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309261444511.png" style="zoom:20%"/>

<!--more-->

默认情况下，**ColorPicker**也支持颜色的透明度选择，如果不想用户选择颜色的透明度，可以将`ColorPicker`的参数`supportsOpacity`设置为`false`即可。如：

```swift
 ColorPicker(selection: $selectedOptions, supportsOpacity: false, label: {
                Text("选择你最喜欢的颜色")
            })
```

此时的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309261450028.png" style="zoom:20%"/>

**ColorPicker**并不支持我们过多的进行自定义，很多场景下，我们使用它的默认样式即可。

