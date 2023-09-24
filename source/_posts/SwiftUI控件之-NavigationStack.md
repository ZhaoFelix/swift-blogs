---
title: SwiftUI控件之 NavigationStack
tags:
  - SwiftUI
  - NavigationStack
  - NavigationLink
categories:
  - SwiftUI 基础
abbrlink: 80d98b55
date: 2023-09-20 13:09:09
---

在 SwiftUI 中，**NavigationStack** 用来作为一个根视图（root view）使得我们能够将一个新的视图展示在这个根视图上。

### 创建一个带导航栏和标题的app

```swift
struct ContentView: View {
    var body: some View {
        NavigationStack {
            List {
                ForEach(0..<10,id: \.self) { index in
                    Text("当前行\(index)")
                }
            }
            .navigationTitle("导航栏标题")
        }
    }
}
```

<!--more-->

在上面的代码中，

1. 将一个`NavigationStack` 作为整个页面的根视图；
2. 给 `NavigationStack` 的第一个子视图`List`添加`navigationTitle`修饰器，<span style="color:red">**注意，**</span>这个修饰器的作为就是给导航栏添加标题。
3. 使用`ForEach`创建`List`的显示内容。

此时，效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309201401648.png" style="zoom:20%"/>

### 导航栏风格

在默认情况下，iOS 中的导航栏为大标题的样式。可以通过`navigationBarTitleDisplayMode`修改标题的样式：

* `inline`：小标题样式，之前 UIKit中常用的样式；
* `larget`：大标题样式，SwiftUI app 中的默认样式。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309201404954.png" style="zoom:20%"/>

不过，即使在大标题的样式下，当我们向下滑动时，当标题样式也会变为小标题样式，向上滑动到顶部时又会重新变为原来的样式。

###  使用NavigationLink 实现页面跳转和展示

文章开头我们说过，使用`NavigationStack`作为根视图来实现页面的展示。要想实现其他视图的展示或者常见的导航栏页面跳转需要和`NavigationLink`一起使用。

```swift
                    NavigationLink {
                        // 要展示的视图
                        Text("当前页面\(index)")
                    } label: {
                        Text("当前行\(index)")
                    }
```

这里，我们使用的是` NavigationLink(destination: () -> View, label: () -> View)`函数，`destination`参数表示的展示的视图，`label`表示通过那个视图来触发展示的内容，即被点击的视图。

当我们添加了`NavigationLink`之后，`List`中的每一行视图都会变成可点击的样式，然后我们点击任意一行之后都将跳转或者展示对应的页面。具体效果如下：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309201433629.mp4" width="30%" height="50%" controls="controls"></video>

在上面的演示效果中，我们会发现，当我们进入展示的目标视图页面，目标视图页面本身也会带上导航栏并且自带返回功能。

<span style="color:red">**注意，**</span>在一些旧版本的 SwiftUI 代码中，使用的是`NavigationView`实现相同的功能，目前`NavigationView`已被逐步放弃，官方更推荐我们使用`NavigationLink`。

