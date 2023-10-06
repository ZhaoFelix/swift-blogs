---
title: SwiftUI 控件之 Button(二)
abbrlink: c45d65f5
date: 2023-10-06 09:18:39
tags: [SwiftUI, Button, Gradient, Modifier]
categories: [SwiftUI 基础]
---

### 使用渐变色作为按钮的背景颜色

在 SwiftUI 中，我们不仅仅能使用一个明确的颜色作为按钮的背景色，也可以使用**渐变色**作为按钮的背景色。

```swift
            Button(action: {
            }, label: {
                Text("Button")
            })
            .padding()
            .foregroundStyle(.white)
            .background(.linearGradient(Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing))
            .clipShape(RoundedRectangle(cornerRadius: 10, style: .circular))
```

在上面的示例中，我们使用了`linearGradient`创建了一个**线性渐变色**作为按钮的背景颜色，`linearGradient`需要三个参数，

* 参数一是一个`Gradient`类型的对象，需要给它设置一个`Color`类型数组作为渐变的颜色；
* 参数二和参数三分别是渐变的**开始点**和**结束点**，例如示例中表示的是渐变从左到右进行。

具体的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310060933521.png"/>

<!--more-->

### 创建一个屏幕与屏幕等宽的按钮

当一个按钮越大时，越能吸引到用户的注意力。所以，在一些情况下下，我们需要创建一个和屏幕完全等宽的按钮。想要实现这样的效果，我们可以给按钮添加`frame`修饰器，然后通过设置`frame`的参数来进行设置。

在上面示例的基础上添加`frame`修饰器，如下：

```swift
.frame(minWidth: 0, maxWidth: .infinity)
```

在上面的代码中，我们给按钮设置了最小的宽度为 0，最大的宽度为**无限**，**这意味着按钮将以尽可能的宽度填充在容器视图中**。

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310060947242.png" style="zoom:20%"/>

<span style="color:red">**需要注意的一点是：**</span>在 SwiftUI 中，同时添加`padding`和`frame`修饰器时，应当将`frame`修饰器置于`padding`前面。即

```swift
.frame(minWidth: 0, maxWidth: .infinity)
.padding()
```

而不是：

```swift
.padding()
.frame(minWidth: 0, maxWidth: .infinity)
```

类似地，还有`padding`和`background`。

```swift
 .padding()
 .background(.linearGradient(Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing))
```

而不是：

```swift
 .background(.linearGradient(Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing))
 .padding()
```

### 使用 ButtonStyle 定义按钮的样式

在实际开发一个app 的过程中，我们都会让整个 app 中的按钮风格统一，这就意味这所有的按钮都会用到相同的修饰器。为了减少相同代码的冗余，我们可以通过自定义修饰器的方式来将一些需要重复使用的风格样式进行单独定义。针对**Button**而言，我们就可以通过**ButtonStyle**来重新定义一个新的按钮样式。



 首先，自定义一个结构体，这个结构体需要实现**ButtonStyle**协议，在实现这个协议之后，这个协议需要实现一个`makeBody`的协议方法。

`makeBody`有一个`configuration`的参数，它是`Configuration`类型，`configuration`包含了一个名为`label`的属性，将我们通用的样式修饰器添加到这个属性上即可修改按钮的样式。

```swift
struct GradientBackgroundStyle: ButtonStyle {
    func makeBody(configuration: Configuration) -> some View {
        configuration.label           
            .frame(minWidth: 0, maxWidth: .infinity)
            .padding()
            .foregroundStyle(.white)
            .background(.linearGradient(Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing))
            .clipShape(RoundedRectangle(cornerRadius: 20, style: .circular))
            .padding(.horizontal, 20)
            
    }
}
```

定义好这个风格样式之后，只需要在原来的**Button**之后添加`buttonStyle`修饰器，然后创建一个`GradientBackgroundStyle`的对象作为参数传进去即可。

```swift
 Button(action: {
            }, label: {
                Text("Delete")
            })
            .buttonStyle(GradientBackgroundStyle())
```

效果还是和之前一样，只是我们可以将相同的样式应用到多个按钮上。

### 使用 Button Role 

在 iOS 15 之后，SwiftUI 为 **Button**提供了一个`role`选项，设置了这个选项之后，SwiftUI 会根据设置的选项进行渲染。`role`选项提供了两种不同的类型，分别是`cancel`和`destructive`。

```swift
Button(role: .cancel) {
                } label: {
                    Text("Delete")
                }
                .buttonStyle(.borderedProminent)
                VStack(alignment: .leading) {
                    Text(".cancel")
                    Text(".borderedProminent")
                }.fontWeight(.bold)
```

不同的样式效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310061047489.png"/>
