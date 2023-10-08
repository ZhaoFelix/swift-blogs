---
title: SwiftUI中的状态管理和绑定
abbrlink: 72b6a6d3
date: 2023-10-08 09:37:10
tags: [SwiftUI, State,  Binding]
categories: [SwiftUI 基础]
---

在 SwiftUI 中，**状态管理**是一个非常重要的概念。假设我们有一个播放音乐的 app，当我们点击播放按钮▶️后，按钮的状态从暂停状态⏸️切换到了播放状态。这里我们可以理解为播放的状态从**一个状态**转移到了**另一个状态**，当然，在实际的应用中，我们可能存在**很多个状态**，当**状态发生变化**时，app 需要根据状态切换到对应用户界面，即 UI。

SwiftUI内置了状态管理的方式，它使用`@State`**属性包装器**去修饰一个变量，这个状态变量会被 SwiftUI 自动存储到应用内，当这个变量的值发生变化时，SwiftUI 会自动重新计算和更新视图，即更新 UI。

### @State的基本使用

首先，我们来看一下下面的代码：

```swift
struct ContentView: View {
    @State var playStatus: Bool = false // 当为 true 表示正在播放
    var body: some View {
        VStack {
            Button(action: {
                playStatus.toggle() // 布尔值取反
            }, label: {
                Image(systemName: playStatus ? "stop.circle.fill" : "play.circle.fill" )
                    .resizable()
                    .frame(width: 80, height: 80)
                    .foregroundStyle(playStatus ? .red : .green)
            })
        }
    }
}
```

<!--more-->

上面的代码中有以下的几个步骤：

1. 使用`@State`修饰了 一个名为`playStatus`的变量，这个变量用来管理后面视图中的状态，这个状态变量是一个`Bool`类型，所以这个变量只有`true`和`false`两种状态；
2. 在视图部分，有一个`Button`，按钮根据`playStatus`状态值的不同显示不用的颜色和按钮图片；
3. 在点击按钮的时候，切换`playStatus`的状态，即有 **true->false或者 false->true**。

效果如下：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310081036991.mp4" width="20%"></video>

### 状态值和视图进行双向绑定

现在，我们想创建一个计数的按钮，每当按钮被点击一次，按钮上的文字次数都加一。

```swift
struct ContentView: View {
    @State var counter: Int = 0
    var body: some View {
        VStack {
            Button(action: {
                counter += 1 // 每点击一次，counter + 1
            }, label: {
                Circle()
                    .fill(.red)
                    .frame(width: 120,height: 120)
                    .shadow(radius: 8)
                    .overlay {
                        Text("\(counter)")
                            .font(.largeTitle)
                            .foregroundStyle(.white)
                    }
                
            })
        }
    }
}
```

效果如下：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310081050156.mp4" width="20%"></video>



这里我们设想一下，如果我们需要创建多个相同的`Button`，那么我们应该如何操作呢？在这种情况下，我们都会专门定义一个**子视图**，如果是定义一个子视图，那么就需要父视图的状态值传递给到子视图，这是就需要使用到`@Binding`属性包装器。

```swift
struct CounterButton: View {
    @Binding var counter: Int
    var body: some View {
        Button(action: {
            counter += 1 // 每点击一次，counter + 1
        }, label: {
            Circle()
                .fill(.red)
                .frame(width: 120,height: 120)
                .shadow(radius: 8)
                .overlay {
                    Text("\(counter)")
                        .font(.largeTitle)
                        .foregroundStyle(.white)
                }
            
        })
    }
}
```

在上面的代码中，同样地需要在子视图中定义一个变量，用来接收从父视图传过来的，因为我们这里要接收的是从父视图传过来的使用`@State`定义的变量，所以子视图中的变量就需要使用`@Binding`进行修饰。

接着就是在父视图中使用这个子视图：

```swift
CounterButton(counter: $counter)
```

此时的效果和之前的是一样的，但是这里如果我们想要重复定义多个类似的`Button`只需要使用`CounterButton`这个子视图就好。

另外就是，这里的<span style="color:red">**$**</span>符号是 SwiftUI 的一种语法格式，它表示**让使用@State定义的变量`counter`和子视图`CounterButton`进行一个双向绑定。即，当这个值发生变化时，子视图同步重新进行计算和渲染。**

