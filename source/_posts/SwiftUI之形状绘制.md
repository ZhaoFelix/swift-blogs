---
title: SwiftUI之形状绘制
tags:
  - SwiftUI
  - 形状
categories:
  - SwiftUI 进阶
abbrlink: 69def74
date: 2023-08-25 12:45:27
---

在 SwiftUI 中，可以使用内置的形状或者根据路径自定义形状。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202308261003030.png" style="zoom:20%;" />

### SwiftUI 中内置的形状

SwiftUI 中给我们提供了五种常用内置形状：矩形，圆角矩形，圆形，椭圆形和胶囊形状。

<!--more-->

#### 矩形

在 SwiftUI 中使用<span style="color:red"> **`Rectangle`**</span> 类创建一个矩形。

```swift
Rectangle()
      .fill(.gray) // 矩形填充的颜色
      .frame(width: 100, height: 100)
```

#### 圆角矩形

在 SwiftUI 中使用<span style="color:red"> **`RoundedRectangle`**</span> 类创建一个圆角矩形，不过它的初始化方法给我们提供了两个属性， `cornerSize` 和 `cornerRadius` 。通过配置这两个属性我们都可以创建一个圆角矩形。

```swift
// 通过圆角尺寸创建圆角矩形
RoundedRectangle(cornerSize: CGSize(width: 8, height: 8))
          .fill(.red)
          .frame(width: 100, height: 100)

// 通过圆角半径创建圆角矩形
RoundedRectangle(cornerRadius: 8)
           .frame(width: 100, height: 100)
```

以上两中方式创建出来的圆角矩形形状是一样的。

#### 胶囊形

创建一个胶囊形状使用 <span style="color:red"> **`Capsule`** </span>类。

```swift
Capsule()
    .fill(.orange)
    .frame(width: 100, height: 60)
```

这里需要注意的一点是，当我们不添加 `frame` 修饰器时，默认情况下为一个圆形。胶囊的最终形状根据给`frame` 修饰器的`width`和`height` 两个属性决定。

#### 椭圆形

创建一个椭圆形状使用 <span style="color:red"> **`Ellipse`** </span>类。

```swift
Ellipse()
     .frame(width: 100, height: 60)
```

同样地，当 `frame` 修饰器中的`width` 和 `height` 两个属性值一样时，椭圆形状会变成一个圆形。

#### 圆形

创建一个圆形状使用 <span style="color:red"> **`Circle`** </span>类。

```swift
Circle()
      .frame(width: 100, height: 100)
```

### 官方文档 

[Shapes | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/shapes)
