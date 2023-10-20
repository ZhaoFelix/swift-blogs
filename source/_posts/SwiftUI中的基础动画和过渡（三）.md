---
title: SwiftUI中的基础动画和过渡（三）
abbrlink: 9d6e228c
date: 2023-10-16 11:03:43
tags: [SwiftUI, Animation, withAnimation]
categories: [SwiftUI 进阶]
---

在之前关于**隐式动画**和**显式动画**的文章中，我们都是将动画添加给一个已经存在的视图。在 SwiftUI，它允许我们去定义一个视图的出现和移除，这被叫做**过渡（Transitions）**。

 默认情况下，SwiftUI 中的视图的出现和移除使用的**fade-in**和**fade-out**过渡效果。除此之外，SwiftUI 也内置了**slide**、**move**、**opacity**等过渡效果。

### 构建一个简单的过渡效果

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            RoundedRectangle(cornerRadius: 10)
                .frame(width: 300, height: 300)
                .foregroundStyle(.green)
                .overlay {
                    Text("显示详情")
                        .font(.system(.largeTitle, design: .rounded))
                        .bold()
                        .foregroundStyle(.white)
                }
            
            RoundedRectangle(cornerRadius: 10)
                .frame(width: 300, height: 300)
                .foregroundStyle(.purple)
                .overlay {
                    Text("哇哦，详情信息")
                        .font(.system(.largeTitle, design: .rounded))
                        .bold()
                        .foregroundStyle(.white)
                }
        }
    }
}
```

<!--more-->

在上面的代码中，我们使用`VStack`布局管理了两个`RoundedRectangle`。接着，我们先让第二个紫色的`RoundedRectangle`隐藏，当我们点击第一个绿色的`RoundedRectangle`时在显示。

这里，我们先来定义一个状态变量，用来控制第二个紫色`RoundedRectangle`的显示或隐藏。

```swi
@State var isShow: Bool = false
```

然后将第二个`RoundedRectangle`使用`if`语句进行包裹：

```swift
  if isShow {
                RoundedRectangle(cornerRadius: 10)
                    .frame(width: 300, height: 300)
                    .foregroundStyle(.purple)
                    .overlay {
                        Text("哇哦，详情信息")
                            .font(.system(.largeTitle, design: .rounded))
                            .bold()
                            .foregroundStyle(.white)
                    }
            }
```

此时，因为`isShow`值为`false`，所以第二个`RoundedRectangle`是不会显示的。

接着，我们给绿色的`RoundedRectangle`添加`onTaGesture`修饰器去识别点击手势，当点击手势发生后，对`isShow`的状态值为取反，并使用`withAnimation`添加一个默认的过渡动画。

```swift
 .onTapGesture {
                    withAnimation {
                        isShow.toggle()
                    }
                }
```

默认的过渡动画如下：

<video controls="controls" width="20%" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310201524703.mp4"></video>

如果想要改变过渡的动画效果，可以给第二个紫色的`RoundedRectangle`添加`transition`修饰器，这个修饰器需要一个动画效果的参数，例如：

```swift
.transition(.scale(scale: 0, anchor: .center))
```

`scale`是一个缩放的效果，它需要两个参数，一个是开始时的比例；另一个是缩放开始的锚点。除了`scale`效果，还有`slide`、`offset`、`move`、`opaque`等。

### 组合过渡效果

如果我们想要要视图呈现多个过渡效果，可以使用`combined`方法来组合多个动画效果，例如

```swift
 .transition(.offset(x: -600, y:0).combined(with: .scale(scale: 0, anchor: .leading)))
```

当然我们也可以组合多个：

```swift
.transition(.offset(x: -600, y:0).combined(with: .scale(scale: 0, anchor: .leading)).combined(with: .opacity))
```

### 定义重复使用的动画

有时候，我们可能想要重复使用一个动画效果。这种情况下，我们可以通用定义一个`AnyTransition`的扩展来实现，例如：

```swift
extension AnyTransition {
    static var offsetScaleOpacity: AnyTransition {
        AnyTransition.offset(x: -600, y:0).combined(with: .scale).combined(with: .opacity)
    }
}
```

使用方式：

```swift
.transition(.offsetScaleOpacity)
```

### 动画练习

#### 练习一

使用动画和过渡创建下面的效果：

<video width="20%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310201704934.mp4"></video>

##### 完整代码和注释

```swift
struct ContentView: View {
    @State private var processing = false // 是否正在处理中
    @State private var completed = false // 是否已处理完成
    @State private var loading = false // 是否正在加载中
    
    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: 30)
                .frame(width: processing ? 250 : 200, height: 60)
                .foregroundStyle(completed ? .red : .green)
            // 如果不是在处理过程中
            if !processing {
                Text("Submit")
                    .font(.system(.title, design: .rounded))
                    .bold()
                    .foregroundStyle(.white)
                    .transition(.move(edge: .top))
            }
            // 正在处理并且处于未完成状态
            if processing && !completed {
                HStack {
                    Circle()
                        .trim(from: 0, to: 0.7)
                        .stroke(Color.white, lineWidth: 3)
                        .frame(width: 20, height: 30)
                        .rotationEffect(.degrees(loading ? 360 : 0 ))
                        .animation(.easeInOut.repeatForever(autoreverses: false), value: loading)
                    
                    Text("Processing")
                        .font(.system(.title, design: .rounded))
                        .bold()
                        .foregroundStyle(.white)
                }
                .transition(.opacity)
                .onAppear {
                    // 页面出现时调用
                    startProcessing()
                }
            }
            // 完成后显示
            if completed {
                Text("Done")
                    .font(.system(.title, design: .rounded))
                    .bold()
                    .foregroundStyle(.white)
                    .onAppear {
                        self.endProcessing()
                    }
            }
        }
        .animation(.spring, value: loading)
        .onTapGesture {
            if !loading {
                processing.toggle()
            }
        }
    }
    
    // 开始处理
    private func startProcessing() {
        self.loading = true
        // 模拟处理过程，4s 后更新为处理完成状态
        DispatchQueue.main.asyncAfter(deadline: .now() + 4) {
            self.completed = true
        }
    }
    
    // 结束处理
    private func endProcessing() {
        // 3s 后重置按钮的所有状态
        DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
            self.processing = false
            self.completed = false
            self.loading = false
        }
    }
}

```

