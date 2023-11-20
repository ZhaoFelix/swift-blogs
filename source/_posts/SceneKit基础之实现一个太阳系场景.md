---
title: SceneKit基础之实现一个太阳系场景(一)
abbrlink: 24c195ec
date: 2023-11-16 16:00:01
tags: [SceneKit, SwiftUI, SCNScene, SCNNode, SCNLight]
categories: [SceneKit 基础]
---

在这篇博文中，我们将实现下面这样的一个效果：

<video width="20%" controls="controls" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311180936806.mp4"></video>

在上面的效果中，使用**SceneKit**创建了一个 3D 的太阳系场景。在这个场景中，有我们常见的行星，然后我们还给场景设置了一个全场景的背景。最后就是通过下面的左右切换按钮可以切换视角查看不同的行星。

<!--more-->

### 创建一个项目

在 Xcode 中，使用通用的模板创建一个 SwiftUI 的项目。然后在项目中创建一个**SceneKit Scene File**，即**SceneKit 的场景文件**。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311180957991.png" style="zoom: 40%"/>

创建完成后，文件的扩展名为`.scn`：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311181003069.png" style="zoom:20%"/>

接着，到`ContentView`文件中，定义一个名为`makeScene`的静态函数用来通过这个文件名加载场景：

```swift
static func makeScene() -> SCNScene? {
        let scene = SCNScene(named: "Solar.scn") // 通过场景文件创建一个场景
        return scene
    }
```

将这个函数返回场景赋值给到一个变量，供后续使用。

```swift
var scene: SCNScene? = makeScene()
```

在`body`部分，通过`SceneView`来展示这个场景：

```swift
SceneView(scene: scene, options: [.allowsCameraControl, .autoenablesDefaultLighting])
```

此时通过预览或者编译运行项目，就能看到场景文件所展示的内容了。因为现在我们的场景文件中除了一个背景什么都没有，所以我们只能看到一个简单的效果。

### 通过场景编辑器（Scene Editor）来编辑布局场景

回到新建的`.scn`文件中，在这个文件中，Xcode 给我们提供了一个**场景编辑器**的功能，让我们可以直接通过界面的方式编辑一个场景。

在**Scene Editor**的左边区域，可以看到整个场景中包含的**节点**，新建的场景中摸下情况下只有一个**相机（Camera）**节点。让我们先来看一下 Apple 官方对**场景（SCNScene）**的定义：

>A container for the node hierarchy and global properties that together form a displayable 3D scene.

在这个定义中，我们可以知道一个**SCNScene**是一系列节点的容器。这些节点属于**SCNNode**类型。



![文档示例图](https://docs-assets.developer.apple.com/published/4f035789b7/592d7da9-814c-4bbd-b1a7-0e07b3003d94.png)

每个**节点**都有很多的属性可以在**Scene Editor**中进行设置，选中任意一个节点后，展开 Xcode 右侧的**观察器**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311200948983.png" style="zoom:30%"/>

熟悉UIKit中**Storyboard**界面编辑的同学会发现，这里多了很多的**观察器**：

* **Node inspector**： 节点观察器，可以设置节点的基本属性，例如位置和大小；
* **Attributes inspetor**：属性观察器；
* **Material inspector**：材质观察器，可以设置节点的材质样式；
* **Physics inspector**： 物理观察器，给节点添加物理效果时用到；
* **Scene inspector**： 场景观察器，可以对场景进行设置。



接下来，我们首先来调整一下场景中相机节点的相关属性。选中相机节点后，点击右侧的**Node inspector**，设置下面的属性：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201006110.png" style="zoom:40%"/>

在我们的场景中，每个节点都会有这些属性，每个属性都可以进行设置：

* **Identity**： 节点名称，在代码中可以通过这个节点名获取到这个节点；
* **Position**：节点在场景中相对于父节点的位置；
* **Euler**： 节点相对于父节点的旋转；
* **Scale** ：在 x、y、z三个轴上的变换尺寸



当我们改变了场景中相机节点的相关属性之后，可能看不到这个节点，可以通过对场景的缩放来查看找到节点。



### 向场景中添加其他节点或对象

在**Scene Editor**中，点击Xcode顶部的**+**按钮：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201033512.png" style="zoom:30%"/>

然后，我们选择一个**Sphere**球体对象拖入到场景中：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201035975.png" style="zoom:30%"/>

此时，如果运行项目，我们是无法在场景中查看这个球体的，因为这个场景并不在相机的**视野**范围内容。我们可以来调整一下这个球体节点的属性。

#### 修改节点材质

选中节点后，在观察器面板选择**Materials inspector（材质观察器）**。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201405923.png" style="zoom:30%"/>

接着，点击**Diffuse**的颜色选择选择器，设置一个**Hex Color**为**#F2FF2C**，这样一来我们就给这个对象添加了一个颜色的材质效果。然后设置**Illumination**的颜色为**white**，设置这个属性可以调整光在节点上的照明度，即便这个节点被和光源隔离，它依然能收到灯光。

#### 修改节点的大小和相机节点视野

为了让这个节点显得更大一点，可以在**Attributes inspector**将**Radius**调整为`10`即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201736484.png" style="zoom:30%"/>

此时，我们会注意到这个节点依然没有在相机节点的视野内。一方面可以调整这个节点相对于相机的位置；另一方面可以直接调整相机的**Z Clipping** 属性。

这个属性有两个值，一个是**Near**，另一个是**Far**。这里我们将**Far**的值调整为`300`。此时的效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201742077.png" style="zoom:40%"/>

### 添加其他的节点

按照上面相同的方式添加其他的节点，同时设置节点的位置、大小、节点名、节点颜色材质以及节点光照效果。以下所有的节点均为**Sphere**。



##### Mercury

* **Name**：mercury
* **Position**： x:0，y:0， z:15
* **Diffuse**： #BBBBBB
* **Roughness**：节点**粗糙度**为`1`，取值范围为 0～1。0 表示光滑，反光越多；1 表示粗糙，反光越少。



##### Venus

* **Name**：venus
* **Position**： x:0，y:0， z:35
* **Diffuse**： #59B1D6
* **Radius**：2
* **Roughness**：节点**粗糙度**为`1`。



##### Earth

* **Name**：earth
* **Position**： x:0，y:0， z:50
* **Diffuse**： #2F5CD6
* **Radius**：2
* **Roughness**：节点**粗糙度**为`1`。



##### Mars

* **Name**：mars

* **Position**： x:0，y:0， z:75

* **Diffuse**： #C65B2C

* **Radius**：2

* **Roughness**：节点**粗糙度**为`1`。

  

##### Saturn

* **Name**：saturn
* **Position**： x:0，y:0， z:150
* **Diffuse**： #D69D5F
* **Radius**：5
* **Roughness**：节点**粗糙度**为`1`。



##### 给土星添加星环

众所周知，土星有一个与众不同支持在于它有一个自己独特的星环。所以我们接下来要给`saturn`节点添加一个星环的子节点。

点击 Xcode 顶部的**+**按钮，选择或者搜索一个**Tube**形状节点添加到`saturn`节点上。添加完成后，在节点管理中中的结构如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201810337.png"/>

接着，我们调整**Tube**形状的以下属性：

* **Inner raduis**： 7 
* **Outer radius**：9 
* **Height**：0.1

然后，就是让这个**Tube** 节点使用和`saturn`相同的材质效果。在**Tube** 的**Attritubes insepector**中，找到**Materials**属性，

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201815812.png" style="zoom: 40%"/>

点击左边的➕按钮，选择对应的材质即可。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201816771.png" style="zoom:20%"/>

此时的场景中效果如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311201817415.png" style="zoom: 30%"/>
