---
title: SwiftUI控件之 ScrollView(二)
abbrlink: ff84e235
date: 2023-09-18 09:22:33
tags: [SwiftUI, ScrollView, ScrollViewReader]
categories: [SwiftUI 基础]
---

### 实现快速滚动到顶部或底部的效果

在很多的**ScrollView** 的应用中，我们经常见到点击“回到顶部”按钮实现快速回到顶部的效果。

在 SwiftUI中，如果我们想要实现这样的效果，我们可以使用**ScrollViewRader**和**ScrollView**来实现。**ScrollViewRader**可以让我们通过编程的方式实现滚动到一个已知的子视图的位置。

```swift

struct ContentView: View {
    @Namespace var topID
    @Namespace var bottomID
    
    var body: some View {
        ScrollViewReader { proxy in
            ScrollView {
                // 顶部的按钮
                Button {
                    withAnimation {
                        proxy.scrollTo(bottomID)
                    }
                } label: {
                    Text("滚动到底部")
                }
                .id(topID)
                
                // 滚动视图
                VStack(spacing: 0.1) {
                    ForEach(0..<100) {i in
                        color(fraction: Double(i) / 100)
                            .frame(height: 32)
                    }
                }
                // 顶部的按钮
                Button(action: {
                    withAnimation {
                        proxy.scrollTo(topID)
                    }
                }, label: {
                    Text("滚动到顶部")
                    
                })
                .id(bottomID)
            }
        }
    }
    
    // 创建颜色
    func color(fraction: Double) -> Color {
        Color(red: fraction, green: 1 - fraction, blue: 0.5)
    }
}
```

<!--more-->

在上面的示例代码中，实现了一下几个步骤：

1. 在`ScrollView`外面嵌套了一层`ScrollViewReader`；
2. `ScrollView`的子视图包括顶部按钮、 100 个渐变颜色子视图和底部按钮是三个部分；
3. 使用`@NameSpace`创建了`topID` 和`bottomID` 两个变量，然后将它们分别给到顶部和底部按钮；
4. 利用`ScrollViewRader` 提供的`proxy` 在点击两个按钮的时候滚动到指定的 ID 的子视图位置。

效果如下：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309180948602.mp4" width="30%" height="50%" controls="controls"></video>

### 实现滚动到指定索引的位置

创建一个数据结构：

```swift
struct NameModel: Identifiable {
    let name:String
    let id: String = UUID().uuidString
    let index: Int
}
```

生成一些测试使用的数据源：

```swift
    // 数据源
    var nameDatas:[NameModel] {
        get {
            (0..<100).map {
                NameModel(name: "我是-\($0)", index: $0)
            }
        }
    }
```

界面布局：

```swift
 var body: some View {
        ScrollViewReader { proxy in
            ScrollView {
                Button(action: {
                    withAnimation {
                        proxy.scrollTo(nameDatas[scrollToIndex].index)
                    }
                }, label: {
                    Text("滚动到索引为\(scrollToIndex)的地方")
                })
                ForEach(nameDatas) { data in
                    Text(data.name)
                        .padding(4)
                        .id(data.index)
                }
                
            }
        }
    }
```

此时的效果为当我们点击按钮后，**ScrollView**将滚动到子视图 ID为 70 的位置：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309181236721.mp4" width="30%" height="50%" controls="controls"></video>

<span style="color:red">**注意：**</span>这里目前依然存在一个问题，当我们使用数据模型的`id`属性作为**ScrollView**子视图的ID 时，即 `.id(data.id)`，无法实现类似的效果。
