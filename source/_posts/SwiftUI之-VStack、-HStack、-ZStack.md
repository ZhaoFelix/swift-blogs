---
title: SwiftUI之 VStack、 HStack、 ZStack
abbrlink: e7687e8f
date: 2023-09-09 09:02:53
tags: [SwiftUI, VStack, HStack, ZStack]
categories:  [SwiftUI 基础]
---

在 SwiftUI 中，我们会经常使用到 **VStack**、**HStack**、**ZStack** 来帮助我们进行布局。

#### VStack

在**VStack**中的子视图都将按照**垂直方向**进行排列。

例如：

```swift
VStack {
    Image(systemName: "globe")
    Text("Hello, World!")
        .font(.body)
}
```

此时的布局效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309090914844.png"/>

<!--more-->

#### HStack 

和**VStack** 类似，不过 **HStack**是将子视图按照**水平方向** 进行排列。

 类似地，将上面示例代码中的**VStack** 改为**HStack** :

```swift
HStack {
    Image(systemName: "globe")
    Text("Hello, World!")
        .font(.body)
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309090918393.png"/>

对于 **VStack** 和 **HStack** 而言，都可以设置它们子视图之前的间距`spacing` 和对齐方式`alignment` 。

区别在于，**VStack** 的对齐方式有：

* `center`：居中对齐；
* `leading` ：左对齐；
* `trailing` ： 右对齐。

**HStack** 的对齐方式有：

* `center` ：居中对齐；
* `top` ：顶部对齐；
* `bottom`：底部对齐。

```swift
VStack(alignment: .leading, spacing: 10) {
            Image(systemName: "globe")
            Text("Hello, World!")
                .font(.body)
}
```



```swift
HStack(alignment: .top, spacing: 10) {
            Image(systemName: "globe")
            Text("Hello, World!")
                .font(.body)
 }
```



#### ZStack 

**ZStack** 是让子视图按照**Z轴方向** 堆叠排列。

```swift
ZStack {
        Image("avatar")
            .resizable()
            .frame(width: 200, height: 200)
        Text("Hello, World!")
            .font(.body)
            .foregroundStyle(.white)
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309090942670.png"/>

**ZStack** 只能设置对齐方式`alignment`，它的对齐方式包括：

* `top`：顶部对齐；
* `bottom`：底部对齐；
* `center`：居中对齐；
* `leading`：左对齐；
* `trailing`: 右对齐；
* `topLeading`：左上角对齐；
* `topTrailing`：右上角对齐；
* `bottomLeading`：左下角对齐；
* `bottomTrailing`: 右下角对齐。



#### 布局示例

##### VStack 

```swift
struct VStackContentView: View {
    var body: some View {
        // 左对齐，上下间距为10
        VStack(alignment: .leading, spacing: 10) {
            ForEach(0...10, id: \.self) {
                Text("选项\($0)")
                Divider()
            }
        }.padding(10)
    }
}
```

##### HStack

```swift
struct HStackContentView: View {
    var title: String = "朋友圈"
    var body: some View {
        HStack(alignment: .center, spacing: 10) {
            Text(title)
            Spacer()
            Image(systemName: "chevron.right")
        }
        .padding(10)
    }
}
```

##### ZStack

```swift
struct ZStackContentView: View {
    // ZStack 实现头像置于背景之上的效果
    var body: some View {
        ZStack {
            Image("bg")
                .resizable()
                .scaledToFit()
                .frame(height: 300)
            Image("avatar")
                .resizable()
                .frame(width: 100, height: 100)
                .clipShape(Circle()) // 圆形头像设置
        }
    }
}
```

##### VStack、 HStack和 ZStack 一起使用

```swift
struct LayoutDemo: View {
    var options = ["朋友圈", "发现", "游戏", "更多"]
    var body: some View {
        VStack {
            ZStackContentView()
            VStack {
                ForEach(options, id: \.self) { title in
                    HStackContentView(title: title)
                    Divider()
                }
            }
        }
    }
}
```

##### ContentView 

```swift
struct ContentView: View {
    @State var selectedSeg:Int = 3
    private let segmentArr = ["VStack", "HStack", "ZStack", "Layout"]
    
    var body: some View {
        VStack(spacing: 20) {
            Picker(selection: $selectedSeg) {
                ForEach(0 ..< segmentArr.count) {
                    Text(segmentArr[$0]).tag($0)
                }
            } label: {
                
            }
            .pickerStyle(.segmented) //设置选择器的样式
            // 根据选择器绑定值的变化不同，显示不同的布局视图
            switch selectedSeg {
            case 0:
                VStackContentView()
            case 1:
                HStackContentView()
                Divider()
            case 2:
                ZStackContentView()
            case 3:
                LayoutDemo()
            default:
                Spacer()
            }
            Spacer()
        }
    }
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309090955427.mp4" style="zoom:20%"/>
