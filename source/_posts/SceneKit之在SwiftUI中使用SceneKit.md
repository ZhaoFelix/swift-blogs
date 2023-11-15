---
title: SceneKit之在SwiftUI中使用SceneKit
abbrlink: 54d45a15
date: 2023-11-15 11:01:15
tags: [SwiftUI, SceneKit, Reality Converter, USDZ]
categories: [SceneKit 基础]
---

>SceneKit is a high-level 3D graphics framework that helps you create 3D animated scenes and effects in your apps. It incorporates a physics engine, a particle generator, and easy ways to script the actions of 3D objects so you can describe your scene in terms of its content — geometry, materials, lights, and cameras — then animate it by describing changes to those objects.

[SceneKit - Apple Developer](https://developer.apple.com/scenekit/)

来自 Apple 的官方文档的说法，SceneKit 是一个面向高层级 3D 图形框架，它可以帮助我们在app中创建一个3D 可变的场景和效果。它包括了物理引擎、粒子生成器以及更简单的给 3D 对象添加动作。

<!--more-->

### 创建 SwiftUI项目并布局

首先，我们现创建一个简单SwiftUI项目，初始的布局代码如下：

```swift
struct ContentView: View {
    var body: some View {
        VStack(spacing: 30) {
            // 使用 SceneKit显示的3D场景部分
            Color.gray
                .frame(height: UIScreen.main.bounds.height/2)
            // 左右的切换按钮
            HStack(content: {
                Image(systemName: "chevron.left.circle")
                Spacer()
                Text("地球")
                    .bold()
                Spacer()
                Image(systemName: "chevron.right.circle")
            }).font(.title)
            
            VStack(alignment: .leading, spacing: 10) {
                Text("简介")
                    .font(.title2)
                    .bold()
                Text("地球（英文名：Earth；拉丁文：Terra）是距离太阳的第三颗行星，也是人类已知的唯一孕育和支持生命的天体。地球的表面大约 29.2% 是由大陆和岛屿组成的陆地。剩余的 70.8% 被水覆盖，大部分被海洋、海湾和其他咸水体覆盖，也被湖泊、冰川、河流和其他淡水体覆盖着，尤其冰川覆盖最多,它们共同构成了水圈。")
                    .font(.body)
            }
            
        }.padding()
    }
}
```

效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311151159305.png" style="zoom:10%"/>

接下来，我们使用**SceneKit**创建一个 3D场景用来显示地球、月球或者火星等。

### 准备好要在 app 中显示或者加载的 3D 模型

如果要使用**SceneKit**显示一些 3D 的场景，我们可以使用内置的几何体对象进行创建，也可直接使用其他的软件创建的 3D 模型，例如 Maya、3DS Max、Blender 等。

不同的 3D 软件创建出来的模型文件格式各有不同，在 **SceneKit**中，Apple 更推荐我们**USDZ**的文件格式。为此，Apple 还提供了用来进行格式转换的**Reality Converter**工具和更专业的**USDZ Tools**，前者直接通过界面操作的方式进行格式转换，后者基于 Python 命令行的方式实现，可操作性更高。

二者均可在该处[Creation tools for spatial apps - Augmented Reality - Apple Developer](https://developer.apple.com/augmented-reality/tools/)下载使用。



这里，我们直接使用一些网上现成提供的模型：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311151214374.png"/>

在 Mac上，每个这样的模型都可以通过内置的**预览**进行查看，然后通过鼠标或者触控板手势与模型进行交互。

接着就是将这些模型直接**拖入**到我们的项目中即可。

拖入到项目中后，每个模型也可以在 Xcode 中进行查看。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311151217244.png" style="zoom:20%"/>

### 使用 SceneKit 展示 3D模型

首先，在头部引入**SceneKit**模块：

```swift
import SceneKit
```

然后，使用`SceneView`创建一个展示 3D 场景的视图：

```swift
SceneView(scene: SCNScene(named: "Earth.usdz"), options: [.allowsCameraControl, .autoenablesDefaultLighting])
 .frame(height: 300)
```

在上面的代码中，我们配置了`SceneView`的两个参数，一个是`scene`场景，另一个是`options`场景可选配置。前者用来展示一个场景，它是`SCNScene`类型，后者是`SceneView.Options`类型。

我们之前拖入的`.usdz`格式的 3D 模型在 Xcode 中本身就是一个`scene`，所以我们这里可以直接加载这个场景即可。对于可选的配置选项`options`，`allowsCameraControl`允许场景中的相机可以自由进行切换，即我们可以上下左右旋转查看模型；`autoenablesDefaultLighting`给场景添加默认的灯光效果。

此时的效果如下：

<video controls="controls" width="20%" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311151414390.mp4"></video>

### 场景和内容切换

接下来，我们来实现让中间的两个按钮可以自由的左右切换不同的场景。

定义一个结构体类型的模型对象：

```swift
struct SceneModel {
    var name: String // 星球名称
    var sceneName: String // 星球模型场景
    var introduce:String //星球简介
}
```

定义一个模型数据数组：

```swift
  var planetModels:[SceneModel] = [
        .init(name: "地球", sceneName: "Earth.usdz", introduce: "地球（英文名：Earth；拉丁文：Terra）是距离太阳的第三颗行星，也是人类已知的唯一孕育和支持生命的天体。地球的表面大约 29.2% 是由大陆和岛屿组成的陆地。剩余的 70.8% 被水覆盖，大部分被海洋、海湾和其他咸水体覆盖，也被湖泊、冰川、河流和其他淡水体覆盖着，尤其冰川覆盖最多,它们共同构成了水圈。"),
        .init(name: "木星", sceneName: "Jupiter.usdz", introduce: "木星为太阳系最大的行星. 木星大到什麽程度呢? 做个比较，如果木星是个中空的球体，那麽其内部大约可以放入1300个地球，可见这颗行星有多巨大. 不过由於木星的密度较地球低，其质量仅为地球的317倍. 左边的图片为木星与地球依照比例呈现的图片。"),
        .init(name: "火星", sceneName: "Mars.usdz", introduce: "太阳系八大行星的第四颗行星，介於地球与小行星群之间，距离太阳约1.52AU，体积大小仅为地球的1/6，而重量为地球的1/10. 为类地行星中距离太阳最远的. 火星古代又被称为荧惑，而英文Mars的意义为战神的意思."),
        .init(name: "冥王星", sceneName: "Pluto.usdz", introduce: "冥王星是太阳系中最後一个较大的行星 ，2006年以前与其他的八大行星并称九大行星，但2006年的天文大会已经将他降级成矮行星。"),
        .init(name: "金星", sceneName: "Venus.usdz", introduce: "金星是太阳系八大行星的第二颗行星，距离太阳约0.72天文单位，轨道在水星与地球之间。金星的一天相当於地球的２３０天，磁场强度只有地球的 10 万分之一左右。此外金星的自转方向与地球以及其他行星的自转方向相反，十分奇特。")
    ]
```

使用`@State`定义一个响应式变量，用来追踪当前展示的场景索引:

```swift
@State var index: Int = 0 
```

修改`SceneView`的代码如下：

```swift
SceneView(scene: SCNScene(named: planetModels[index].sceneName), options: [.allowsCameraControl, .autoenablesDefaultLighting])
```

名称和简介的`Text`分别为：

```swift
Text(planetModels[index].name)
```

和

```swift
Text(planetModels[index].introduce)
```

最后就是给中间的两个`Image`添加一个点击切换的手势和事件，

切换到上一个：

```swift
    Image(systemName: "chevron.left.circle")
                    .onTapGesture {
                        withAnimation() {
                            index = index == 0 ? planetModels.count - 1 : index - 1
                        }
                    }
```

切换到下一个：

```swift
 Image(systemName: "chevron.right.circle")
                    .onTapGesture {
                        withAnimation {
                            index = index == planetModels.count - 1 ? 0 : index + 1
                        }
                        
                    }
```

<span style="color:red">**注意**</span>，在通过数组索引访问数组元素时，一定要注意**数组索引越界**的问题。

此时的效果如下：

<video controls="controls" width="20%" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311151447381.mp4"> </video>

至此，这个简单的项目就完成了。后续我们将逐步深入了解**SceneKit**中的基础元素，包括**场景**、**节点**、**相机**、**灯光**、**物理引擎**、**粒子系统**等。

