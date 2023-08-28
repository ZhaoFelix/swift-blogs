---
title: SwiftUI之绘制自定义形状
tags:
  - SwiftUI
  - 自定义形状
categories:
  - SwiftUI 进阶
abbrlink: 8c78197a
date: 2023-08-27 09:55:29
---

在之前一片文章中，我们介绍了 SwiftUI 中内置的一些图形形状。在一些特殊的工能需求下，我们需要自定义去绘制一些形状，例如五角星⭐️，多边形。

### 自定义路径

在 SwiftUI 中可以使用 <span style="color:red">`Shape`</span>协议自定义路径。

```swift
// 自定义一个结构体，实现 Shape 协议
struct DrawRectangleShape: Shape {
    func path(in rect: CGRect) -> Path {
        var path = Path()
        path.addRect(rect)
        return path
    }
}
```

在上面的代码中，我们自定义了一个实现 `Shape` 协议的结构体类型，在这个自定义的结构体中我们需要实现一个 `path(in:)`的协议方法，这个方法要求我们返回一个 `Path`对象，即我们要绘制的形状路径。

 <!--more-->

在上面的这个例子中，我们直接将传进来的 `rect` 添加到绘制的路径中。具体表现为下面的代码绘制的是一个红色矩形：

```swift
DrawRectangleShape()
            .fill(.red)
            .frame(width: 200, height: 200)
```

以及下面的代码绘制结果看起来像是一根红色的线，因为绘制的图像的形状高度为 1。

```swift
DrawRectangleShape()
            .fill(.red)
            .frame(width: 200, height: 1)
```

