---
title: 在SwiftUI 中创建一个内容切换效果
tags:
  - SwiftUI
  - 动画
  - SF Symbol
categories:
  - SwiftUI 进阶
abbrlink: d4605c9a
date: 2023-08-29 10:09:15
---

### 文字内容切换效果

界面布局：

```swift
struct ContentView: View {
    @State private var text: Int = 0
    var body: some View {
        VStack(spacing: 20) {
            Text("$\(text)")
                .font(.largeTitle.bold())
                .contentTransition(.numericText(value: Double(text)))
            Button {
                withAnimation(.bouncy) {
                    text = .random(in: 100...10000)
                }
            } label: {
                Text("Update")
            }

        }
        .padding()
    }
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202308291020904.mp4" style="zoom:20%"></img>



<!--more--> 

<span style="color:red">**注意**</span>：`contentTransition` 修饰器的系统要求为： `@available(iOS 16.0, macOS 13.0, tvOS 16.0, watchOS 9.0, *)`。

###  SF Symbol 切换效果

界面布局：

```swift
struct ContentView: View {
    @State private var sfImage: String = "house.fill"
    @State private var sfCount: Int = 1
    var body: some View {
        VStack(spacing: 20) {
            Image(systemName: sfImage)
                .font(.largeTitle.bold())
                .contentTransition(.symbolEffect(.automatic))
            Button {
                let images:[String] = ["suit.heart.fill", "house.fill", "gearshape", "person.circle.fill", "iphone", "macbook"]
                withAnimation(.bouncy) {
                   sfCount += 1
                    sfImage = images[sfCount % images.count]
                }
            } label: {
                Text("Update")
            }

        }
        .padding()
    }
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202308291039418.mp4"  style="zoom:20%"/>

<span style="color:red">**注意**</span>：

* `contentTransition` 修饰器的系统要求为： `@available(iOS 16.0, macOS 13.0, tvOS 16.0, watchOS 9.0, *)`;

* `symbolEffect` 修饰器的系统要求为：`@available(iOS 17.0, macOS 14.0, tvOS 17.0, watchOS 10.0, *)`。

  

