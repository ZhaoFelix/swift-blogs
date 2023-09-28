---
title: SwiftUI控件之 PhotosPicker
abbrlink: 3f68e07
date: 2023-09-28 09:02:09
tags: [SwiftUI, Picker, PhotosPicker]
categories: [SwiftUI 基础]
---

在 SwiftUI 中，有一个特殊的**Picker（选择器）**，叫做**PhotosPicker（相册选择器）**，使用它可以帮助快速获取手机相册中的照片。

### 创建一个 PhotosPicker

**PhotosPicker**本身并不属于 **SwiftUI**这个模块中，它属于**PhotosUI**模块，所以我们首先需要在头部导入这个模块：

```swift
import PhotosUI
```

使用 `@State` 定义一个变量用来和`PhotosPicker`进行绑定：

```swift
 @State var selectedImages: PhotosPickerItem?
```

创建`PhotoPicker`：

```swift
PhotosPicker("选择喜欢的照片", selection: $selectedImages)
```

此时的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309280923302.png" style="zoom:20%"/>

<span style="color:red">**注意**：</span>上图的效果为 iOS 17 以后的效果。

<!--more-->

因为相册里面的照片属于用户的**敏感隐私数据**，所以访问的时候会有隐私数据的提示。一般情况下，Apple 希望我们在隐私设置中添加访问敏感隐私数据的使用说明。

点击我们都项目**根目录**，在**TARGETS**中找到**Info**选项，在**Custom iOS Target Properties**中添加一个访问手机相册说明。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309280929340.png" style="zoom:20%"/>



添加的记录如下，它是以**Key-Value**的形式存在的：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309280932456.png"/>

### 使用选中后的照片

使用**PhotosPicker**选中后的照片为 `PhotosPickerItem`类型，我们要想让选中的照片使用`Image`进行显示需要使用 `loadTransferable(type:completionHandler:)` 方法，具体使用如下：

```swift
struct ContentView: View {
    @State var selectedImages: PhotosPickerItem?
    @State var avatarImage: Image?
    var body: some View {
        VStack {
            PhotosPicker("选择喜欢的照片", selection: $selectedImages)
            // 非空时显示
            if let avatarImage {
                avatarImage
                    .resizable()
                    .frame(width: 200, height: 200)
            }
        }
        // 当前 selectedImages的值发生变化时调用
        .onChange(of: selectedImages) { _ in
            // 创建一个异步任务
            Task {
                // 将选中的 PhotosPickerItem 类型转换为要显示的 Image
                if let image = try? await selectedImages?.loadTransferable(type: Image.self) {
                    avatarImage = image
                    return
                }
            }
        }
    }
}
```

在上面的代码中，实现的步骤如下：

1. 使用`@State` 定义一个 `Image`类型的变量用来接收转换后的照片；
2. 使用`onChange`修饰器监听`selectedImages`变量值的的变化，当发生变化时将新的值转换为`Image`进行显示；
3. 转换的过程中使用了`Task`创建了**异步**任务配合`loadTransferable`方法使用；
4. 当`avataImage`的值不为`nil`时显示照片。

### 配置 PhotosPicker 可选择的范围

在 iOS ，我们平时拍的照片、截图、录的视频或者下载保存的视频都是存储在相册中的，`PhotosPicker`提供了一个`matching`参数用来设置应用可以选择的相册范围。

例如，如果想要应用只能选择视频，可以像下面这样设置：

```swift
PhotosPicker("选择喜欢的照片", selection: $selectedImages, matching: .videos)
```

它来提供了很多的范围可选，例如：

* `panoramas`：全景照片；

* `images`: 所有照片；

* `videos`: 所有视频；

* `screenshots`: 截图

  ......

  

  
