---
title: SwiftUI 控件之 ScrollView(一)
tags:
  - ScrollView
  - SwiftUI
categories:
  - SwiftUI 基础
abbrlink: 35d3a19e
date: 2023-09-17 09:43:58
---

在 SwiftUI 中，**ScrollView** 是一个可滚动的视图。

#### 创建一个 ScrollView 

当我们使用`ForEach` 循环创建超出一整个屏幕的内容时：

```swift
      VStack {
            ForEach(0..<100) { index in
                Text("当前行\(index)")
            }
        }
```

在上面的代码中，我们使用`VStack`和`ForEach` 创建了 100 个`Text`，此时要显示的内容已经超出了一个屏幕所能显示的内容，我们无法看到超出屏幕范围的内容。为了解决这个问题，我们就可以在最外层嵌套一个`ScrollView`，让整个视图变成滚动显示的。

```swift
ScrollView {
            VStack {
                ForEach(0..<100) { index in
                    Text("当前行\(index)")
                }
            }
        }
```

此时，我们就可以通过上下滚动的方式查看所有的`Text` 了。

<!--more-->

#### ScrollView的相关配置

##### 控制 ScrollView 的滚动位置

可以通过`defaultScrollAnchor` 修饰器配置`ScrollView`的开始滚动的起始位置，它支持以下几个位置：

* `top` : 从顶部开始滚动；
* `bottom` : 从底部开始滚动；
* `center`： 从中间位置开始滚动；
* `leading`: 从左侧开始滚动；
* `trailing`： 从右侧开始滚动；
* `topLeading`：从左上开始滚动；
* `topTrailing`：从右上开始滚动；
* `bottomLeading`：从左下开始滚动；
* `bottomTrailing`：从右下开始滚动。

```swift
.defaultScrollAnchor(.center)
```

##### 控制 ScrollView 滚动的方向

**ScrollView** 支持水平`horizontal`和垂直`vertical`方向的滚动。

```swift
ScrollView(.vertical) {
  
}
```

##### 控制 ScrollView 滚动指示器的显示/隐藏

**ScrollView** 滚动的时候，视图边缘会有一个滚动的指示器。可以使用`scrollIndicators` 修饰器来控制它的显示或者隐藏。

```swift
.scrollIndicators(.hidden)
```

* `hidden`: 表示隐藏；
* `visible`: 表示显示；
* `never`：和`hidden` 一致；

指定水平或垂直方向上的显示或者隐藏：

```swift
.scrollIndicators(.hidden, axes: [.horizontal])
```

