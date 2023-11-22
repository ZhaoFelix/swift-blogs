---
title: SceneKit基础之实现一个太阳系场景(二)
tags:
  - SwiftUI
  - SCNScene
  - SCNLight
  - SCNNode
  - SceneKit
categories:
  - SceneKit 基础
abbrlink: 75fc57c3
date: 2023-11-22 10:04:32
---

在上一篇文章中，我们已经通过**Scene Editor**实现了场景的布局。接下来我们将通过代码的方式来给节点添加图片材质、场景背景以及切换相机的视角。



###  定义模型和实现切换

定义一个名为**Planet**的枚举用来管理节点数据：

```swift
enum Planet: String, CaseIterable {
    case mercury
    case venus
    case earth
    case mars
    case saturn
    
    var name:String {
        // 名字为首字母大写
        rawValue.prefix(1).capitalized + rawValue.dropFirst()
    }
}
```

<!--more-->

定义一个名**ViewModel**的类用来管理场景中选中的节点以及节点的切换事件：

````swift
class ViewModel: NSObject,ObservableObject {
    @Published var selectedPlanet: Planet?  // 当前选中的节点
    
    // 当前选中的节点名称
    var title:String {
        selectedPlanet?.name ?? ""
    }
    
    // 选择下一个
    func selectedNextPlanet() {
        changeSelection(offset: 1)
    }
    
    // 选择上一个
    func selectedPreviousPlanet() {
        changeSelection(offset: -1)
    }
    
    // 清空选择
    func clearSelection() {
        selectedPlanet = nil
    }
    
    
    // 节点改变
    private func changeSelection(offset: Int) {
        guard let selectedPlanet = selectedPlanet, let index = Planet.allCases.firstIndex(of: selectedPlanet) else {
            selectedPlanet = Planet.allCases.first
            return
        }
        let newIndex = index + offset
        if newIndex < 0 {
            self.selectedPlanet = Planet.allCases.last
        } else if newIndex < Planet.allCases.count {
            self.selectedPlanet = Planet.allCases[newIndex]
        } else {
            self.selectedPlanet = Planet.allCases.first
        }
    }
}

````

回到**ContentView**文件中，实例化一个**ViewModel**的对象：

```swift
@ObservedObject var viewModel = ViewModel()
```

然后在 `body`部分实现界面的完全布局：

```swift
 var body: some View {
        ZStack {
            SceneView(scene: scene,
                      pointOfView:setupCamera(planet: viewModel.selectedPlanet),
                      options: [.allowsCameraControl, .autoenablesDefaultLighting])
            .background(.secondary)
            
            VStack {
                Spacer()
                HStack {
                    HStack {
                        HStack {
                            Button(action: {
                                viewModel.selectedPreviousPlanet()
                            }, label: {
                                Image(systemName: "arrow.backward.circle.fill")
                            })
                            Button(action: {
                                viewModel.selectedPreviousPlanet()
                            }, label: {
                                Image(systemName: "arrow.forward.circle.fill")
                            })
                        }
                        Spacer()
                        Text(viewModel.title)
                            .foregroundStyle(.black)
                        Spacer()
                        // 不为空时显示清除按钮
                        if viewModel.selectedPlanet != nil {
                            Button(action: {
                                viewModel.clearSelection()
                            }, label: {
                                Image(systemName: "xmark.circle.fill")
                            })
                        }
                    }
                }
                .padding(8)
                .background(.white)
                .clipShape(RoundedRectangle(cornerRadius: 14))
                .padding(20)
            }
            
        }
        .ignoresSafeArea(.all)
    }
```

在上面的代码中，`SceneView`多了一个`pointOfView`参数，它是一个`SCNNode`类型，它表示的是当前场景渲染时的**视角点**。我们可以通过切换视角点来切换当前场景中可见的内容。`setupCamera`函数的实现如下：

```swift
 // 根据选中的节点，改变相机的视点
    func setupCamera(planet: Planet?) -> SCNNode? {
        // 获取场景中的相机节点
        let cameraNode = scene?.rootNode.childNode(withName: "camera", recursively: false)
        
        if let planet = planet,  let planetNode = scene?.rootNode.childNode(withName: planet.rawValue, recursively: false) {
            let constraint = SCNLookAtConstraint(target: planetNode)
            cameraNode?.constraints = [constraint]
            let globalPosition = planetNode.convertPosition(SCNVector3(50, 10, 0), to: nil)
            let move = SCNAction.move(to: globalPosition, duration: 1.0)
            cameraNode?.runAction(move)
        }
        return cameraNode
    }
```

它的实现如下：

1. 根据节点名`camera`获取相机节点`cameraNode`；
2. 根据传进来的`planet`的`rawValue`获取当前选中的节点`planetNode`；
3. 然后设置`cameraNode`的`constraints`；
4. 给`cameraNode`添加一个移动的`action`；
5. 然后新的相机节点`cameraNode`作为场景的`pointOfView`。

### 给星球节点添加图片材质

将准备好的星球图片素材拖入到**Assets**素材文件夹下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311221022305.png" style="zoom:20%"/>

同理，拖入准备好的场景背景素材添加到这个文件夹下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311221056208.png" style="zoom:20%"/>

**注意**，这里的场景背景图片需要放入六张图片。因为给 3D 场景背景添加背景图片需要同时设置 6 个方向，即 XYZ 三个轴的正负方向。



接着定义一个名为`applyTextures`的方法来添加材质和背景：

```swift
  // 给场景添加材质
    static func applyTextures(to scene: SCNScene?) {
        guard let scene = scene else { return }
        // 给每个星球添加图片材质
        for planet in Planet.allCases {
            let identifier = planet.rawValue
            let node = scene.rootNode.childNode(withName: identifier, recursively: false)
            let texture = UIImage(named: identifier)
            node?.geometry?.firstMaterial?.diffuse.contents = texture
        }
        
        // 给整个场景添加背景材质
        let skyboxImages = (1...6).map { UIImage(named: "skybox\($0)") }
        scene.background.contents = skyboxImages
    }
```

给节点添加材质通过设置节点的`geometry?.firstMaterial`或者`geometry?.materials`。前者是只添加一个材质；后者添加多个材质。

给场景添加添加背景通过设置场景的`background.contents`。

最后，别忘了在`makeScene`方法中调用这个方法。
