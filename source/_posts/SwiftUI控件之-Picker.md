---
title: SwiftUI控件之 Picker
abbrlink: bd95df5
date: 2023-09-23 10:30:27
tags: [SwiftUI, Picker]
categories: [SwiftUI 基础]
---

在我们做一些信息表单填写时，经常需要用到**选择器（Picker）**。

### 创建 Picker

```swift
        Picker("今天周几",  selection: $selectedOptions) {
            Text("星期一")
            Text("星期二")
            Text("星期三")
            Text("星期四")
            Text("星期五")
            Text("星期六")
            Text("星期天")
        }
```

使用`@State`创建一个动态响应的变量，用于和`Picker`进行绑定，当选中某一项时，这个变量对应的值也会发生变化。

```swift
@State var selectedOptions:Int =  0
```

此时默认的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309240934896.png" style="zoom:50%"/>

但是如果我们此时选择任意一个其他的选项，发现并没有什么变化，而且控制台会输入下面的信息：

<span style="color:red">**Picker: the selection "0" is invalid and does not have an associated tag, this will give undefined results.**</span>

<!--more-->

这是因为我们没有给`Picker`的每一个选项设置一个不同的`Tag`值，我们只需要做下面的修改就可以解决这个问题。

```swift
        Picker("今天周几",  selection: $selectedOptions) {
            Text("星期一")
                .tag(0)
            Text("星期二")
                .tag(1)
            Text("星期三")
                .tag(2)
            Text("星期四")
                .tag(3)
            Text("星期五")
                .tag(4)
            Text("星期六")
                .tag(5)
            Text("星期天")
                .tag(6)
        }
```

这样解释了为什么我们在声明绑定的动态变量`selectedOptions`时，将它声明为`Int`类型了。

### 使用循环遍历的方式创建选项

在上面的示例中，我们都是直接定义每个选项的内容，当我们的选项很多的时候我们可以使用`ForEach`循环创建选项：

```swift
        Picker("今天周几",  selection: $selectedOptions) {
            ForEach(0..<7) { index in
                Text("选项\(index+1)")
                    .tag(index)
            }
        }
```

这里我们直接使用每次循环的索引作为选项的`Tag`值。

### 设置 Picker的不同风格

和 SwiftUI 中很多其他的控件一样，**Picker**也提供了很多的样式，我们可以通过`pickerStyle`进行设置。

```swift
 Picker("今天周几",  selection: $selectedOptions) {
                    ForEach(0..<7) { index in
                        Text("选项\(index+1)")
                            .tag(index)
                    }
                }
                .pickerStyle(.inline)
```

我们可以设置这些风格 `inline`、`segmented`、`wheel`、`palette`、`menu`、`navigationLink`。

不同风格的样式呈现如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309241012504.png" style="zoom:20%"/>

<span style="color:red">**注意：**</span>，`navigationLinke`样式需要和 **NavigationStack** 一起配合使用，即在父视图外面嵌套一个`NavigationStack`，具体如下：

```swift
        NavigationStack {
            Picker("今天周几",  selection: $selectedOptions) {
                ForEach(0..<7) { index in
                    Text("选项\(index+1)")
                        .tag(index)
                }
            }
            .pickerStyle(.navigationLink)
        }
```

它的展示效果如下：

<video src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309241054945.mp4" width="20%" height="50%" controls="controls"> </video>



除了基础的**Picker**外，还有**DatePicker（日期选择器）** 和 **ColorPicker（颜色选择器）**，我们将在后面的内容中详细介绍它们的使用。

