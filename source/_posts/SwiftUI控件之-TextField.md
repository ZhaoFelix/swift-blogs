---
title: SwiftUI控件之 TextField
tags:
  - SwiftUI
  - TextField
  - SecureField
categories:
  - SwiftUI 基础
abbrlink: edbc1019
date: 2023-09-13 14:13:47
---

在 SwiftUI 中，**TextField** 用来展示和接收用户编辑的文本内容。

#### 使用

在创建一个**TextField**之前，需要先定义一个**响应式**类型的状态变量：

```swift
@State private var userName:String = ""
```

在创建**TextField** 时，使用`$userName` 的形式绑定这个状态变量：

```swift
TextField("User Name",text: $userName)
```

当`userName` 这个变量和**TextField** 绑定后，当我们在输入框中输入或者删除内容时，`userName` 对应的值也会跟着发生变化。

<!--more-->

#### TextField 的不同风格

使用`textFieldStyle` 修饰器来定义**TextField**的不同风格：

* `roundedBorder`： 圆角边框样式；
* `plain` ： 默认的无边框样式；

```swift
        VStack(spacing: 20) {
            TextField("User Name",text: $userName)
                .textFieldStyle(.roundedBorder)
            
            TextField("User Name",text: $userName)
                .textFieldStyle(.plain)
        }
```

 效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309131432602.png"/>

#### SecureField

除了常见的**TextField** ，还可以使用**SecureField** 来获取一些隐私数据的输入，例如密码。

```swift
SecureField("Password",text: $password)
```

使用安全输入框时，当用户输入类似于密码之类的隐私数据时，会以**`····`** 形式呈现，从而起到保护隐私数据的作用。



#### 官方文档

[TextField | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/textfield)

[SecureField | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/securefield)
