---
title: RealityKit之创建一个AR应用
tags:
  - RealityKit
  - Reality Composer
  - Reality Converter
categories:
  - RealityKit
abbrlink: eed84786
date: 2023-11-29 11:04:50
---

### 使用模板创建 AR 项目

使用 Xcode创建一个 AR 项目时，直接选择**Augmented Reality App**模板：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311291114380.png" style="zoom:40%"/>

接着，项目的配置选择**SwiftUI**和**RealityKit**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311291115728.png" style="zoom:40%"/>

项目创建好后，运行项目。<span style="color:red">**注意**</span>：**RealityKit**构建的项目无法直接在预览或者模拟器中查看效果，需要将项目运行在 iPad 或者 iPhone的真实设备上。

<!--more-->

 默认项目运行情况如下：

<video width="20%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311291123894.MP4" ></video>

初始默认的项目中，通过实现`UIViewRepresentable`协议来显示一个**UIKit**中的视图。这个协议必须要是实现两个方法`makeUIView`和`updateUIView`，前者用来构建和返回一个 `UIView`类型的视图；后者根据**SwiftUI**中的状态变量变化时来更新`UIView`。



### RealityKit 层次结构介绍



<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311291135843.png" style="zoom:50%"/>

上面的图为官方文档中提供的层次结构图。

在**RealityKit**中，最上层是一个**ARView**，用来向用户展示一个渲染的 3D 图形。一个**ARView**都有一个单一的**Scene**，这是一个**Entity**集合的容器。在**Scene**上，我们可以添加一个或者多个**AnchorEntity**，这个实例可以让 **ARView**知道如何将内容显示在真实的世界中。对于每一个**AnchorEntity**，我们可以在上面添加多个和多层次的**Entity**用来构建场景内容。



现在，我们来看看初始项目中的`makeUIView`方法，它的实现步骤如下：

1. 创建一个**ARView**实例：

```swift
let arView = ARView(frame: .zero)
```

2. 创建一个**ModelEntity**的实例，并添加一些基础的材质。这里是创建了一个长宽高均为 0.1m，圆角 0.005m的正方体：

```swift
let mesh = MeshResource.generateBox(size: 0.1, cornerRadius: 0.005)
let material = SimpleMaterial(color: .gray, roughness: 0.15, isMetallic: true)
let model = ModelEntity(mesh: mesh, materials: [material])
```

3. 创建一个**AnchotEntity**实例，并将上面创建**ModelEntity**添加到这个实例上：

```swift
let anchor = AnchorEntity(.plane(.horizontal, classification: .any, minimumBounds: SIMD2<Float>(0.2, 0.2)))
        anchor.children.append(model)
```

4. 将创建的**AnchorEntity**实例添加到**ARView**的**Scene**上：

```swift
arView.scene.anchors.append(anchor)
```

5. 最后就是返回**ARView**的实例。



### 场景追踪和辅助层添加

在一个 AR 的场景中，我们需要对当前的场景变化进行实时的场景追踪，例如识别到水平面后才放置 3D 对象。要想实现场景的追踪，需要用到**ARKit**，所以我们首先需要导入这个模块：

```swift
import ARKit
```

然后删除`makeUIView`中除了`ARView`实例创建的代码：

```swift
  func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero)
       
        // TODO: 待添加
        return arView
    }
```

接着就是在`TODO`部分实现场景追踪的代码：

```swift
     let session = arView.session // 获取 ARView中的 session
     let config = ARWorldTrackingConfiguration() // 世界追踪配置
     config.planeDetection = [.horizontal] // 水平面追踪
     session.run(config) // AR的session根据这个配置进行运行
```

AR 的 session 用来支持视图的渲染，它是**ARSession**的实例。在**RealityKit**中会自动创建一个默认的`session`来管理视图。

如果此时运行项目，它已经能够实时的追踪当前场景中的平面了，为了更加直观的看到这种追踪的效果，可以在**ARView**中添加一个辅助层视图。

```swift
  // 辅助视图
        let coachingOverlay = ARCoachingOverlayView()
        coachingOverlay.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        coachingOverlay.session = session
        coachingOverlay.goal = .horizontalPlane // 目标为水平
        arView.addSubview(coachingOverlay) // 将赋值视图作为子视图添加到 ARView 上
```

另外，当 AR 追踪到平面时可以让它显示特征点或者锚点：

```swift
arView.debugOptions = [.showFeaturePoints, .showAnchorOrigins, .showAnchorGeometry]
```

此时项目运行后的效果如下：

<video width="30%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311291527924.MP4"></video>

当 AR 场景中识别到水平面之后，就会显示水平面的特征点、一个锚点坐标轴锚点的几何形状。另外我们注意到AR 场景会识别到多个不同的水平面。

### 从 Reality Composer 中获取`.usdz`格式的模型

从 iPad 或者 iPhone 的 App Store 中搜索下载**Reality Composer**这个软件。**Reality Composer**可以让我们简单快速的构建、调整和模拟一个 AR 场景。

在 **Xcode 15.0**之前集成了这个工具，但是目前最新版的**Xcode** 已经移除。

除此之外，还需要下载 **Reality Converter **，它可以帮助我们在 Mac 上查看、自定义以及转换不同格式的 3D 模型。

> **下载链接**：[Creation tools for spatial apps - Augmented Reality - Apple Developer](https://developer.apple.com/augmented-reality/tools/)



### 点击放置物体对象



 <span style="color:red">**未完待续！！**</span>
