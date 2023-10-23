---
title: SwiftUI 之 List 常见设置
tags:
  - SwiftUI
  - List
categories:
  - SwiftUI 基础
abbrlink: 4c9505b9
date: 2023-10-23 15:27:40
---

在 SwiftUI 中，**List** 是非常常用的一个组件。它可以帮助我们快速实现一个列表视图。

假设，我们现在下面这样的一个**List**示例：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310231554363.png" style="zoom:10%"/>

示例代码如下：

```swift
struct ContentView: View {
    var books: [String] = ["三国演义","水浒传", "红楼梦", "西游记"]
    var body: some View {
        VStack {
            List(books, id: \.self) { book in
                Text(book)
            }
            .listStyle(.inset)
        }
    }
}
```

针对**List**，我们可以进行很多的样式设置。

<!--more-->

### 改变 List 中<span style="color:red">分割线</span>的颜色

默认情况下，**List**中分割线的颜色默认的灰色。想改变它的默认颜色，可以使用下面的这个修饰器：

```swift
.listRowSeparatorTint(.green)
```

但是，这这里需要<span style="color:red">**注意**</span>的一点就是，这个修饰器我们需要添加到`List`里面的子视图中，在本示例中为`Text`，即

```swift
Text(book)
     .listRowSeparatorTint(.green)
```

### 隐藏分割线

隐藏`List`中的分割线可以使用`listRowSeparator`修饰器，它的参数为`hidden`或`visible`。

```swift
.listRowSeparator(.hidden)
```

### 自定义 List 滚动区域的背景

在 **iOS 16** 之后，`List`支持自定义它的背景。例如，如果我们要想给 `List`添加一个背景色：

首先，需要隐藏`List`的滚动内容背景，即

```swift
.scrollContentBackground(.hidden)
```

然后，可以使用`background`修饰器设置新的背景：

```swift
.background(
                // 使用渐变色作为背景
                LinearGradient(colors: [.red, .green], startPoint: .top, endPoint: .center)
            )
```

此时，效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310231645149.png" style="zoom:20%"/>

如果想要让`List`的显示内容也使用我们自定义的背景颜色，可以让`List`中的每一行颜色都变为透明色即可

```swift
.listRowBackground(Color.clear)
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310231652348.png" style="zoom:10%"/>

完整代码如下：

```swift
struct ContentView: View {
    var books: [String] = ["三国演义","水浒传", "红楼梦", "西游记"]
    var body: some View {
        VStack {
            List(books, id: \.self) { book in
                Text(book)
                    .listRowSeparator(.visible)
                    .listRowBackground(Color.clear)
                    .foregroundStyle(.white)
                    .bold()
                    .listRowSeparatorTint(.white.opacity(0.5))
            }
            .scrollContentBackground(.hidden)
            .background(
                // 使用渐变色作为背景
                LinearGradient(colors: [.red, .green], startPoint: .top, endPoint: .center)
            )
        }
    }
}
```

