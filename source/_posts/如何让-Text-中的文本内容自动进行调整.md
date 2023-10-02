---
title: 如何让 Text 中的文本内容自动进行调整
tags:
  - SwiftUI
  - Text
categories:
  - SwiftUI 技巧
abbrlink: 585c3fa7
date: 2023-10-02 10:28:16
---

在 SwiftUI 中，使用**Text** 添加了`frame`修饰器后文本内容经常出现显示不完整的情况，例如下面的场景：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021032152.png"/>

如果我们希望在指定控件的范围内让文本内容尽可能地显示完整，我们可以给`Text`添加 `minimumScaleFactor`修饰器，即设置**Text**的**最小比例因子**。它的作用就是指定文本内容能按照设定的`font`原始尺寸缩小的最小比例，从而让文本内容尽可能的显示完整。例如：

```swift
   VStack(spacing:20) {
            Text("Swift is a powerful and intuitive programming language for all Apple platforms. It’s easy to get started using Swift, with a concise-yet-expressive syntax and modern features you’ll love. Swift code is safe by design and produces software that runs lightning-fast.")
                .font(.headline)
            
            Text("Swift is a powerful and intuitive programming language for all Apple platforms. It’s easy to get started using Swift, with a concise-yet-expressive syntax and modern features you’ll love. Swift code is safe by design and produces software that runs lightning-fast.")
                .font(.headline)
                .minimumScaleFactor(0.5)
        }
        .padding()
        .frame(height: 200)
```

因为我们给`HStack`设置了`frame`，并把高度限制为`200`，所以在这个高度范围内是无法完整显示两个`Text`内容的。如果不给最后一个`Text`设置`minimumScaleFator`，它展示的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021040860.png"/>

设置后的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021041405.png"/>

通过前后两种效果的比较我们可以发现，在设置了`minimumScaleFactor`为 `0.5`后，后一个`Text`的字体大小在原来设置的`font(.headline)`的基础上进行缩小操作。

<!--more-->

注意，**它并不是缩小为原来尺寸的`0.5`，它只是在能显示完整内容的基础上进行了缩小，只是我们设定了最小能缩小的值为原来的`0.5`。**



当我们把`HStack`的`frame`的`height`设置能完整显示两个`Text`时，字体就不会进行缩放了。

```swift
.frame(height: 500)
```

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021053426.png"/>



同样地，如果把`height`设置的很小，后一个文本字体最小能缩放的范围也只是原来的`0.5`.

```swift
.frame(height: 100)
```

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021055994.png"/>

如果把`minimumScaleFactor`设置为 `0.1`，它就可以缩的更小。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202310021056910.png"/>

