---
title: SwiftUI控件之 List
tags:
  - SwiftUI
  - List
  - Identifier
  - ForEach
categories:
  - SwiftUI 基础
abbrlink: 57a1224a
date: 2023-09-12 09:17:29
---

在很多 app 中，我们经常能看到上下滚动的列表，在 SwiftUI 中，我们可以使用**List** 实现这样的功能。

#### 创建一个列表

要实现一个列表，我们需要先创建一个**数组**类型的数据源：

```swift
var books: [String] = ["西游记", "红楼梦", "三国演义", "水浒传"]
```

使用 `ForEach` 遍历数组，将元素显示到 `List`中：

```swift
List {
    ForEach(books, id: \.self) { book in
         Text("《\(book)》")
    }
}
```

 除了上面的这种方式，我们还可以直接将数据元给到`List`，然它进行循环创建列表：

```swift
List(books, id: \.self) { book in
            Text("《\(book)》")
}
```

这里我们注意到，不管是我们使用哪种方式，我们都需要设置一个`id`的参数，这是因为`List` 要求每一行的元素都要有一个<span style="color:red">**唯一**</span>的id，以便我们后续对**List** 进行编辑操作。这里我们是直接将循环出来的元素对象本身作为id 给它。

<span style="color:red">**注意，这在数据源中元素不会出现重复的情况下是可行，但是如果存在重复相同的元素，就会出现编辑异常的情况。**</span>

<!--more-->

#### 不同的 `listStyle` 

我们可以通过 `listStyle` 修饰器来定义**List** 不同的风格样式，`insetGrouped`、`inset`、`grouped`、`plain`、`sidebar`、`automatic`。

注意，在不同的系统中样式显示也不一致。例如`sidebar` 样式在 iPadOS 中显示为侧边栏样式，在 iOS 中则为默认的样式。

```swift
.listStyle(.automatic)
```

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309121039437.png"/>

#### List 分组和设置  footer 、 header

如果我们想要实现 List 分组的效果，可以使用 **Section** ，然后**Section** 的构造方法分别给每个分组设置**header** 和**footer**。

```swift
        List {
            Section {
                ForEach(books, id: \.self) { book in
                    Text("《\(book)》")
                }
            } header: {
                Text("四大名著")
                    .bold()
            } footer: {
                Text("推荐阅读书籍")
            }
            Section {
                ForEach(books, id: \.self) { book in
                    Text("《\(book)》")
                }
            } header: {
                Text("四大名著")
                    .bold()
            } footer: {
                Text("推荐阅读书籍")
            }
        }
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309121055074.png" style="zoom:40%"/>

