---
title: SwiftUI 控件之 Image
tags:
  - SwiftUI
  - Xcode
  - Image
  - SF Symbol
categories:
  - SwiftUI 基础语法
abbrlink: 6b844059
date: 2023-09-08 09:52:11
---

在 SwiftUI 中，我们可以使用**Image**控件来展示一张图片。

### 基础使用

在 SwiftUI中，如果我们需要使用**Image**显示一张图片，首先我们需要先将图片资源拖到蓝色的<span style="color:blue">**Assets**</span>文件夹中，然后使用图片的文件名加载显示。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309081000150.png"/>

```swift
Image("avatar")
```

在最新的<span style="color:red"> **Xcode 15.0**</span>中，增加了使用下面的方式加载资源文件夹中的图片的特性：

```swift
Image(.avatar)
```

上面两种方式的效果是一样的。

<!--more-->

### 常用的修饰器

#### 宽高

设置图片显示的`frame`：

```swift
Image(.avatar)
	.frame(width: 100, height: 200)
```

当我们设置**Image**的`frame`后，我们发现它并没有按照我们所给的宽高进行显示，这是因为我们需要先添加 `resizzble` 修饰器。

```swift
Image(.avatar)
	.resizable()
	.frame(width: 100, height: 200)
```

####  填充

但是此时我们可能会发现，我们的图片显示**变形** 了，这主要是因为我们的图片宽高比例和所给的`frame` 的宽高比例并不一致。我们可以通过设置`scaledToFill`或者`scaledToFit` 修饰器来修饰图片的填充样式。

```swift
struct ContentView: View {
    var body: some View {
        VStack(spacing: 20) {
            VStack {
                Image(.avatar)
                    .resizable()
                    .scaledToFit()
                    .frame(width: 100, height: 200)
                    .border(.red)
                Text("scaledToFit")
            }
            Divider()
            VStack {
                Image(.avatar)
                    .resizable()
                    .aspectRatio(contentMode: .fill)
                    .scaledToFill()
                    .frame(width: 100, height: 200)
                    .border(.red)
                Text("scaledToFill")
            }
        }
    }
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309081112939.png" style="zoom:20%"/>

在预览图中，<span style="color:red">**红色**</span> 的边框表示我们实际设置的`frame` 范围。在同一张图片和同样的`frame`设置下，我们可以很清楚的看到`scaledToFit`和`scaledToFill`的区别：

* `scaledToFit` 会优先按照给定的`frame` **宽度（width）**进行填充；
* `scaledToFill` 会优先按照给定的`frame` **高度（height）**进行填充。

 #### 对齐方式

我们可以在通过`frame`设置宽高的时候来设置图片的对齐方式：

```swift
.frame(width: 100, height: 200,alignment: .center)
```

默认情况下为居中（center）对齐方式.

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309081120845.png"/>

#### 图片裁切

当图片超出定义的`frame`范围时，可以使用`clipped` 修饰器让图片显示的范围和我们定义的`frame` 一致：

```swift
Image(.avatar)
     .resizable()
     .scaledToFill()
     .frame(width: 100, height: 200,alignment: .top)
     .border(.red)
     .clipped()
```



效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309081148253.png" style="zoom:20%"/>

将图片按照形状进行裁切：

```swift
.clipShape(Circle())
```

