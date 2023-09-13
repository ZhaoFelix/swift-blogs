---
title: SwiftUI控件之 Label
tags:
  - SwiftUI
  - Label
categories:
  - SwiftUI 基础
abbrlink: 8bd109ea
date: 2023-09-13 13:41:11
---

在 SwiftUI 中，**Label** 用来展示一个文本和图标（icon）。

#### 使用

使用**Assets** 中的图片作为**Label** 的图标：

```swi
Label("avatar", image: "avatar")
```

使用**SF Symbols** 中的图标作为**Label** 的图标：

```swift
Label("apple", systemImage: "apple.logo")
```

#### 设置不同的**Label**样式

可以使用`labelStyle` 修饰器来定义 Label 的样式：

* `iconOnly` : 只显示图标；
* `titleAndIcon`: 显示文字和图标，默认样式；
* `titleOnly` ： 只显示文字；
* `automatic`：自动。

```swift
 Label("apple", systemImage: "apple.logo")
    .labelStyle(.automatic)
```

<!--more-->

#### 使用一个 View作为 Label 的图标

```swift
            Label {
                Text("Felix")
                    .font(.body)
                    .foregroundStyle(.primary)
                Text("Zhao")
                    .font(.subheadline)
                    .foregroundStyle(.secondary)
                
            } icon: {
                Circle()
                    .fill(.red)
                    .frame(width: 44, height: 44)
                    .overlay(Text("F"))
            }
```

在上面的Label 中，我们使用两个不同样式的`Text` 作为**Label** 要显示的文字内容；使用一个`Circle` 作为**Label**的自定义图标视图。

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309131405868.png"/>

