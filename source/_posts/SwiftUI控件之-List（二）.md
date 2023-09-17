---
title: SwiftUI控件之 List（二）
abbrlink: 37193cc1
date: 2023-09-16 08:37:34
tags: [SwiftUI, List, Identifier, NavigationStack, EditButton]
categories: [SwiftUI 基础]
---

#### 创建 List

在之前的**List** 使用中，我们是直接使用`ForEach` 循环遍历一个数组来实现的，在遍历的时候我们把`self` 作为**List**中必须的**ID**。接下来，我们通过自定义一个数据对象同时让它实现`Identifiable`。

```swift
struct Ocean: Identifiable {
    let name:  String
    let id: String = UUID().uuidString
}
```

在上面的代码中，我们自定义了一个结构体类型的对象`Ocean` ，同时让它实现了`Identifiable` 协议。在实现这个协议之后，它需要结构体的有一个名为`id`的成员属性，然后这个属性的值我们直接通过`UUID().uuisString` 创建一个字符串类型的值给它，这样可以实现之后创建的每一个`Ocean` 实例对象都有一个**唯一**的**ID**，便于我们在**List** 遍历时使用。

<!--more-->

同样地，使用数组的方式创建一些数据源。

```swift
private var oceans = [
    Ocean(name: "Pacific"),
    Ocean(name: "Atlantic"),
    Ocean(name: "Indian"),
    Ocean(name: "Southern"),
    Ocean(name: "Arctic")
]
```

使用`List` 和 `ForEach`创建一个**List**：

```swift
        List {
            ForEach(oceans) { ocean in
                Text(ocean.name)
            }
        }
```

**<span style="color:red">注意：</span>在上面的代码中，由于`ForEach`遍历的对象实现了`Identifiable`协议，所以我们就不再需要去单独设置`id`。**

或者可以直接讲数据源给到`List`来创建：

```swift
        List(oceans) { ocean in
            Text(ocean.name)
        }
```

上面的代码也可以通过语法进行简写：

```swift
List(oceans) {
            Text($0.name)
        }
```

简写前和简写后的效果是一样的，简写代码中的`$0`表示遍历的第一个参数，还可以有类似于`$1`、`$2` ...等这样的形式。

#### List 中的多选

首先将`List`嵌套到`NavigationStack`中实现一个带顶部导航栏的样式：

```swift
        NavigationStack {
            List(oceans) {
                Text($0.name)
            }
            .navigationTitle("Oceans") // 导航栏的标题
        }
```

同时，通过`toolBar`添加一个`EditButton` 按钮：

```swift
.toolbar {
                EditButton()
  }
```

完整代码如下：

```swift
struct ContentView: View {
    private var oceans = [
        Ocean(name: "Pacific"),
        Ocean(name: "Atlantic"),
        Ocean(name: "Indian"),
        Ocean(name: "Southern"),
        Ocean(name: "Arctic")
    ]
    @State private var multiSelection = Set<String>()
    var body: some View {
        NavigationStack {
            List(oceans, selection: $multiSelection) {
                Text($0.name)
            }
            .navigationTitle("Oceans") // 导航栏的标题
            .toolbar {
                EditButton()
            }
            Text("当前选中：\(multiSelection.count)")
        }
    }
}
```

在上面的代码中，实现了一下几步：

1. 通过`NavigationStack` 实现了导航栏；
2. 在`toolBar`上添加了一个`EditButton`，当点击 **EditButton**后**List** 将进入**可编辑模式**；
3. 使用`@State` 定义了一个元素类型和`Ocean` 中`id` 类型（**String**）一致的动态元祖变量，然后将这个变量和**List**中的`selection`绑定，当 **List** 进入编辑模式之后我们就可以获取到被选中的**List**中**唯一**的**ID**。

效果如下：

<video width="30%" height="50%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309161014389.mp4"> </video>



#### List 的下拉刷新加载

首先将数据源`oceans` 使用`@State` 声明为动态的响应式类型：

```swift
 @State private var oceans = [
        Ocean(name: "Pacific"),
        Ocean(name: "Atlantic"),
        Ocean(name: "Indian"),
        Ocean(name: "Southern"),
        Ocean(name: "Arctic")
    ]
```

然后给`List`添加一个`refreshable `修饰器：

```swift
.refreshable {
                // 每次刷新向 oceans 数组中添加一个新的元素
                oceans.append(.init(name: "Other"))
 }
```

此时，当程序运行后，每次下拉刷新**List** 中都将添加一个新的名为`Other` 的元素。

