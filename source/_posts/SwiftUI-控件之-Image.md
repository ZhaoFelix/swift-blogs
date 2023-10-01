---
title: SwiftUI 控件之 Image
tags:
  - SwiftUI
  - Xcode
  - Image
  - SF Symbols
  - Overlay
categories:
  - SwiftUI 基础
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

#### 给图片添加一个 Overlay 

如果我们想要给`Image`添加一个遮罩层可以使用`overlay`修饰器，它的参数是一个`View`类型，这意味这我们可以将任意一个`View`类型的视图作为遮罩层。

```swift
  .overlay {
                    Image(systemName: "heart.fill")
                        .foregroundStyle(.white)
                        .font(.system(size: 40))
                        .opacity(0.8)
                }
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310010946106.png" style="zoom:50%"/>

`overlay`也可以通过设置`alignment`参数来设置遮罩层的对齐方式，例如：

```swift
     .overlay(alignment: .center, content: {
                    Text("这是用AI生成的一个头像")
                        .foregroundStyle(.black)
                        .font(.title)
                        .fontWeight(.bold)
                        .padding()
                        .background(.white)
                        .clipShape(RoundedRectangle(cornerRadius: 8)) // 添加圆角
                        .opacity(0.8) // 透明度
                })
```

#### Image 加载显示 SF Symbols

**SF Symbols** 是 Apple 提供的一套系统内置的常用图标集。在最新的**SF Symbols 5** 中已经提供了总共**5296**个各种各样的图标。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310011000319.png" style="zoom:20%"/>

在使用这些系统图标时，只需要使用`Image(systemName:String)`这个方法即可，`systemName`就是图标的名称。

```swift
Image(systemName: "heart.circle")
```

需要注意的一点是，<span style="color:red">**SF Symbols** 本质上还是属于字体的一部分，所以很多适用于字体设置的修饰器同样也适用于它。</span>例如，

```swift
   Image(systemName: "heart.circle")
                .font(.title)
                .fontWeight(.bold)
                .foregroundStyle(.red)
```

**SF Symbols**支持多种颜色和多种渲染模式。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310011015286.png" style="zoom:30%"/>

通过代码可以这么进行设置：

```swift
     Image(systemName: "pencil.circle.fill")
                .font(.largeTitle)
                .fontWeight(.bold)
                .symbolRenderingMode(.palette) // 渲染模式
                .foregroundStyle(.yellow, .tint, .red) // 颜色设置
```

对于一些特殊的**Symbols**，它可以设置一个可变的值，根据这个实现不同的样式。例如：

```swift
Image(systemName: "slowmo", variableValue: 0.2)
                .font(.system(size: 50))
                .foregroundStyle(.indigo)
```

不同值的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310011023505.png"/>

**在最新的 SwiftUI 和 SF Symbols中还支持添加动画效果，我们将在后续动画部分进行介绍。**

