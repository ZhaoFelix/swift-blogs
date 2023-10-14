---
title: SwiftUI 中的基础动画和过渡（一）
abbrlink: b2c22197
date: 2023-10-13 09:47:49
tags: [SwiftUI, withAnimation, Animation]
categories: [SwiftUI 进阶]
---

SwiftUI 给我们提供了两种动画的实现方式： **隐式(implicit)动画** 和**显式(explicit)动画**。这两种动画方式都可以让我们给视图添加动画和过渡。**隐式动画**通过修饰器`animation`给视图添加动画效果，**显式动画**则是使用`withAnimation`代码块的方式添加到视图。

### 隐式动画

```swift
struct ContentView: View {
    @State var circleColor: Bool = false
    @State var heartColor: Bool = false
    @State var heartSize: Bool = false
    var body: some View {
        ZStack() {
            Circle()
                .frame(width: 150, height: 150)
                .foregroundStyle(circleColor ?  Color(.systemGray5) : .red)
            Image(systemName: "heart.fill")
                .foregroundStyle(heartColor ?  .red : .white)
                .font(.system(size: 80))
                .scaleEffect(heartSize ? 1.0 : 0.5)
        }
        .onTapGesture {
            // 每次点击之后，三个状态值都进行取反操作
            circleColor.toggle()
            heartColor.toggle()
            heartSize.toggle()
        }
    }
}
```

在上面的代码中，我们实现了：

1. 使用`@State`定义了三个状态变量，用来控制圆的颜色、❤️的颜色以及大小；
2. 使用`Circle`绘制了一个圆，然后在这个圆显示一个系统的图标，将二者使用`ZStack`进行组合；
3. 给`ZStack`添加 一个点击手势`onTapGesture`，每次点击都对三个状态值进行取反操作，即`toggle`；
4. 运用**三目运算符** 根据不同的状态值显式不同的颜色和大小。

<!--more-->

具体效果如下：

<video width="20%" height="50%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310141544030.mp4"> </video>



通过上面的效果我们会发现，每次点击之后对应视图的颜色和大小都会发生变化，但是这个变化是比较生硬的，这是因为我们没有给视图添加动画的原因。接着我们就分别给`Circle`和`Image`添加一个隐式的动画效果，即给每一个视图添加一个`animation`修饰器。

`Circle`的动画如下 ：

```swift
.animation(.easeInOut, value: circleColor)
```

`Image`的动画如下：

```swift
.animation(.easeInOut, value: heartColor)
```

这里我们使用的`animation`修饰器需要两个参数，一个是动画类型，另一个是对应的一个动态变量的值。它的含义是当对应状态变量的值发生变化时，实现相关的动画效果。

此时的点击切换效果如下：

<video width="20%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310141554539.mp4"></video>

SwiftUI 中给我们提供了很多种的动画类型，包括各种的`ease`、`linear`、`spring`等。关于不同的`ease`动画，可以到[Easing Functions Cheat Sheet (easings.net)](https://easings.net/) 查看。

### 显式动画

上面的动画效果，我们同样可以使用**显式动画**来实现，在`onTapGesture`中将三个状态值的取反操作包裹在一个`withAnimation`代码块中，即：

```swift
withAnimation {
                circleColor.toggle()
                heartColor.toggle()
                heartSize.toggle()
}
```

此时点击，视图已经有了一个默认的动画效果。当然，我们也可以去定义动画的类型：

```swift
 withAnimation(.spring(duration: 1.2, bounce: 1, blendDuration: 3) {
                circleColor.toggle()
                heartColor.toggle()
                heartSize.toggle()
 }
```

效果如下：

<video width="20%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310141628100.mp4"> </video>

在显式动画中，我们能很好的控制视图的动画效果。例如，如果我们不希望给❤️图标的大小变化添加任何的动画效果，只需要把`heartSize.toggle()`这行代码移到`withAnimation`代码块外面即可。如下：

```swift
withAnimation(.spring(duration: 1.2, bounce: 1, blendDuration: 3) {
                circleColor.toggle()
                heartColor.toggle()
            }
heartSize.toggle()
```

如果我们想要在**隐式动画** 中实现和上面同样的效果，我们可以通过重新排序修饰器的方式来进行。

```swift
struct ContentView: View {
    @State var circleColor: Bool = false
    @State var heartColor: Bool = false
    @State var heartSize: Bool = false
    var body: some View {
        ZStack() {
            Circle()
                .frame(width: 150, height: 150)
                .foregroundStyle(circleColor ?  Color(.systemGray5) : .red)
                .animation(.spring(duration: 1.0, bounce: 1,blendDuration: 3), value: circleColor)
          
            Image(systemName: "heart.fill")
                .foregroundStyle(heartColor ?  .red : .white)
                .font(.system(size: 80))
                .animation(.spring(duration: 1.0, bounce: 1,blendDuration: 3), value: heartColor)
                .scaleEffect(heartSize ? 0.7 : 0.5)
        }
        .onTapGesture {
            circleColor.toggle()
            heartColor.toggle()
            heartSize.toggle()
        }
    }
}
```

在上面的代码中，针对`Image`的动画，我们将`animation`修饰器放在了`scaleEffect` 前面，那么当对应的状态值`heartColor`发生变化时这个动画效果就不会作用在`scaleEffect`上，即❤️的图标不会有尺寸变化的效果。

