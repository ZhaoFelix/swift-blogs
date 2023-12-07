---
title: SwiftUI之app的生命周期
tags:
  - SiwftUI
  - scenePhase
  - AppDelegate
  - SceneDelegate
categories:
  - SwiftUI 进阶
abbrlink: 2626effb
date: 2023-12-01 16:49:31
---

在 SwiftUI之前，我们管理 app 的生命周期使用**AppDelegate**，这在使用 **UIKit** 开发时是非常常见的。在 iOS 13.0 之后，对于 SwifUI 的 app 可以使用**AppDelegate**和**SceneDelegate**来管理 app 的生命周期。

```swift
@main
struct AppLifeycleApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var delegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

<!--more-->

`AppleDelegate`:

```swift
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        print("启动")
        return true
    }
}
```

在上面的代码中使用了`@UIApplicationDelegateAdaptor`包装器。如果还需要使用`SceneDelegate`来调用更多的生命周期函数，可以先定义一个`SceneDelegate`的类：

```swift
class SceneDelegate: NSObject,UIWindowSceneDelegate {
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let _ = (scene as? UIWindowScene) else { return }
    }
    
    func sceneDidEnterBackground(_ scene: UIScene) {
        print("场景进入后台")
    }
}
```

然后在`AppDelegate`中添加下面这个函数，让它和`SceneDelegate`进行关联：

```swift
 func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        let sceneConfig: UISceneConfiguration = UISceneConfiguration(name: nil, sessionRole: connectingSceneSession.role)
        sceneConfig.delegateClass = SceneDelegate.self
        return sceneConfig
    }
```

<span style="color:red">**需要注意的是**</span>，但我们使用`AppDelegate`来管理 app 的生命周期时，需要针对不同的平台进行判断。相反，使用后面的这种方式则不用。

>**Note**
>
>If you enable scene support in your app, iOS always uses your scene delegates in iOS 13 and later. In iOS 12 and earlier, the system uses your app delegate.



在**iOS 14**之后，SwiftUI 中提供了一种新的方式来管理 app 的生命周期：

```swift
@main
struct AppLifeycleApp: App {
    @Environment(\.scenePhase) var scenePhase
    
    var body: some Scene {
        WindowGroup {
            ContentView()
                .onChange(of: scenePhase, {
                    switch scenePhase {
                    case .background:
                        print("进入后台")
                    case .inactive:
                        print("不活跃")
                    case .active:
                        print("活跃")
                    @unknown default:
                        print("默认")
                    }
                })
        }
    }
}
```

这里，我们是直接使用`Environment`中的`scenePhase`来获取当前 app 的状态。



> 参考文章： **[Managing  your app's life cycle ](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle?source=post_page-----e9be79e75fef--------------------------------)**
