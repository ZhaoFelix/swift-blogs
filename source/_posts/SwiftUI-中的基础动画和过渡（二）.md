---
title: SwiftUI 中的基础动画和过渡（二）
abbrlink: a0094105
date: 2023-10-14 16:53:53
tags: [SwiftUI, withAnimation, Animation]
categories: [SwiftUI 进阶]
---

在上一篇文章中，我们已经介绍了 SwiftUI 中的**隐式动画**和**显式动画**。

在 SwiftUI中，只需要更多的关注动画的开始和结束，动画的过程由 SwiftUI 帮我们自动完成。接着我们来实现一些常见的动画效果。

### 创建一个加载动画

```swift
struct ContentView: View {
    @State var isLoading: Bool = false
    var body: some View {
        ZStack() {
          Circle()
                .trim(from: 0, to: 0.8)
                .stroke(lineWidth: 10)
                .fill(.green)
                .rotationEffect(.degrees(isLoading ?  360 : 0))
                .animation(.default.repeatForever(autoreverses: false), value: isLoading)
        }
        .frame(width: 100, height: 100)
        .onAppear {
            isLoading = true
        }
    }
}
```

<!--more-->



在上面的代码中，

1. 使用 `Circle`创建了一个非闭合的圆环；
2. 给`Cricle`添加了`rotationEffect`修饰器实现旋转的效果，它需要一个角度作为参数；
3. 给`Circle`添加了**隐式动画**和动态变量`isLoading`进行绑定；
4. 当整个视图显式的时候，将动态变量`isLoading`的值改为`true`，开始动画；

这里的`animation`动画的效果我们使用的是默认`default`，然后链式调用了 `repeatForever`方法，这个方法中可以设置动画是否反转。

不反转：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310160942851.gif" />

反转：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310160943712.gif"/>

即设置为反转后，动画结束后会回到动画开始前的效果。



如果我们想要改变动画的速度，只需要将默认`default`动画设置为`linear`，然后修改`duration`参数即可。如下：

```swift
.animation(.linear(duration: 1).repeatForever(autoreverses: false), value: isLoading)
```

**`duration`值越大，动画速度越慢，即持续时间越长。**

另外，`onAppear`修饰器是 SwiftUI 中视图的生命周期函数，类似于 UIKit的中`viewDidAppear`方法，它的作用是当视图出现在屏幕是自动调用。

类似地，我们可以对代码稍加修改即可实习下面的这样效果：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310160958398.gif"/>

修改后的代码如下：

```swift
struct ContentView: View {
    @State var isLoading: Bool = false
    var body: some View {
        ZStack() {
            Circle()
                .stroke(lineWidth: 10)
                .fill(Color(.systemGray5))
            Circle()
                .trim(from: 0, to: 0.2)
                .stroke(lineWidth: 10)
                .fill(.green)
                .rotationEffect(.degrees(isLoading ? 360 : 0))
                .animation(.linear(duration: 1).repeatForever(autoreverses: false), value: isLoading)
        }
        .frame(width: 100, height: 100)
        .onAppear {
            isLoading = true
        }
    }
}
```



### 动画延迟

通过设置`duration`可以修改动画的速度，同样地可以通过设置`delay`来设置的延迟效果。

```swift
struct ContentView: View {
    @State var isLoading: Bool = false
    var body: some View {
        HStack() {
            ForEach(0..<4, id: \.self) { index in
                Circle()
                    .fill(.green)
                    .frame(width: 10, height: 10)
                    .scaleEffect(isLoading ? 0.3 : 1)
                    .animation(.linear(duration: 0.6).repeatForever(autoreverses: true).delay(0.2 * Double(index)), value: isLoading)
            }
        }
        .onAppear {
            isLoading = true
        }
    }
}
```

在上面的代码中，我们使用`ForEach`创建了四个相同的`Circle`，然后都给它们添加了一个大小改变的修饰器`sacleEffect`，动画效果我们设置为`linear`，链式调用了`repeatForever`和`deplay`方法。

`delay`通过`0.2*Double(index)`设置，即四个`Circle`的延迟动画时间分别为：0，0.2，0.4，0.6。通过设置延迟时间，可以让`Circle`动画的开始时间不同。 如果没有这个延迟时间，所有的`Circle`都将同时开始出现动画效果。

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310161007140.gif"/>



### 矩形变换为圆

```swift
struct ContentView: View {
    @State var recordBegin: Bool = false
    @State var recording: Bool = false
    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: recordBegin ? 30 : 5)
                .fill(recordBegin ? .red : .green)
                .frame(width: recordBegin ? 60 : 240, height: 60)
                .overlay {
                    Image(systemName: "mic.fill")
                        .font(.title)
                        .foregroundStyle(.white)
                        .scaleEffect(recording ? 0.7 : 1.0)
                }
            RoundedRectangle(cornerRadius: recordBegin ?  35 : 10)
                .trim(from: 0, to: recordBegin ? 0 : 1)
                .stroke(lineWidth: 5)
                .fill(.green)
                .frame(width: recordBegin ? 70 : 250, height: 70)
        }
        .onTapGesture {
            withAnimation(.linear(duration: 0.3)) {
                recordBegin.toggle()
            }
            
            withAnimation(.default.repeatForever().delay(0.5)) {
                recording.toggle()
            }
            
        }
    }
}
```

在上面的代码中，我们通过两个状态变量控制`RoundedRectangle`的`frame`和`cornerRadius`来实现一个圆角矩形到圆的变换效果。

效果如下：

<video width="30%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310161058603.mp4"></video>
