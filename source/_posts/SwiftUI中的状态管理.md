---
title: SwiftUI中的状态管理
abbrlink: 177cbef
date: 2023-10-31 15:51:17
tags: [SwiftUI, State, Binding, StateObject, ObserableObject, ObservedObject]
categories: [SwiftUI]
---

在 SwiftUI 中，**响应式**是区别于 UIKit 的一大特点。SwiftUI 中的**响应式**主要依赖与数据的状态来进行视图的更新和重绘。

### @State

假设我们想要实现一个简单的计数功能，即点击➕按钮实现次数的加一。如下的代码:

```swift
struct ContentView: View {
    var count: Int = 0
    var body: some View {
        VStack(spacing: 10) {
            Text("当前次数\(count)")
            Button(action: {
                
            }, label: {
                Image(systemName: "plus")
                    .font(.title)
            })
            
        }
    }
}
```

在上面的代码中，我们简单地在`Button`的`action`部分添加`count += 1`，如果我们这样做了，程序会报以下错误：

<span style="color:red">**Left side of mutating operator isn't mutable: 'self' is immutable**</span>

<!--more-->

这是因为在 Swift 中**结构体**是**值类型**，它不能让我们直接修改它的成员属性。如果要想解决这个问题，只需要给`count`添加一个`@State` 的属性包装器，即

```swift
@State var count: Int = 0 
```

当添加了这个属性包装器之后，SwiftUI 会自动去管理`count`这个变量。

### @Binding 

在 SwiftUI 中，很多的控件需要一个变量值进行**双向绑定**。例如，**TextField**

```swift
TextField(text: <Binding<String>>, label: <() -> View>)
```

上面是`TextField`的一个基础构造方法，它的第一个参数`Text`要求的就是一个`Binding<String>`类型的值，这里要求的就是一个**双向绑定**，即当输入框的值发现变化时，变量的值同步发生改变；或者变量值发生变化时，输入框显示的内容也同步发生变化。

具体的用法如下：

```swift
@State var inputText:String = "" // 声明一个状态变量
```

将这个状态变量和`TextField`控件使用`$`语法建立双向绑定：

```swift
 TextField(text: $inputText) {
                Text("输入")
  }
```

在另外的一种情况下，如果我们想让父视图和子视图建立类似的双向状态值绑定，需要在子视图中使用`@Binding`属性包装器来定义一个变量接受父视图传过来的状态值。

```swift
// 子视图
struct SubContentView: View {
    @Binding var inputText: String
    var body: some View {
        TextField(text: $inputText) {
            Text("输入")
        }
    }
}
```

父视图中调用这个子视图：

```swift
struct ContentView: View {
    @State var inputText:String = "" // 声明一个状态变量
    var body: some View {
        VStack(spacing: 10) {
           SubContentView(inputText: $inputText)
        }
    }
}
```

这样一来，父视图和子视图中的值将同步发生变化，进而更新视图。

### ObservableObject 和 ObservedObject 

上面的`@State`和`@Binding`我们常用于基础的状态管理，在实际的开发应用中，我们更多的是通过一个数据模型的方式来管理视图的状态。

例如，我们先声明一个`class` 的数据模型，同时让它实现`ObservableObject`协议，

```swift
class DataManager: ObservableObject {
    @Published var dataList: [String] = ["西游记", "水浒传"]
}
```

在这个`class`模型中，会存在多个属性。如果我们需要指定属性，当这个属性的值发生变化是同步更新相关的视图，那么我们需要使用`@Published`属性包装器来修饰这个变量。

在使用时，需要使用`@ObservedObject`属性包装器来修饰这个模型类型的变量，以达到”订阅“的作用。

```swift
struct ContentView: View {
    @ObservedObject var dataManager: DataManager = DataManager()
    var body: some View {
        VStack(spacing: 10) {
            ForEach(dataManager.dataList,id:\.self) { data in
                Text(data)
            }
            Button(action: {
                dataManager.dataList.append("红楼梦")
            }, label: {
                Text("添加")
            })
        }
    }
}
```

还有一个和`@ObservedObject`类似的属性包装器`@StateObejct`，二者的作用类似，后面我们会专门讲解二者的区别。

### EnvironmentObject

在 SwiftUI 中，View 提供了 `environmentObject(_)` 方法，来把某个 ObservableObject 的值注入到当前 View 层级及其子层级中去。在这个 View 的子层级中，可以使用 `@EnvironmentObject` 来直接获取这个绑定的环境值。



使用`EnvironmentObject`可以帮助我们实现将数据值进行多层视图的注入。例如在`ContentView`根视图中注入一个值，然后在多层子视图中获取和更新对应的值。

`ContentView`中注入数据值：

```swift
struct ContentView: View {
    @ObservedObject var dataManager: DataManager = DataManager()
    var body: some View {
        NavigationStack {
            ChildContentView()
        }.environmentObject(dataManager)
    }
}
```

第一层的子视图：

```swift
// 第一层子视图
struct ChildContentView: View {
    var body: some View {
        SubChildContentView()
    }
}
```

第二层子视图：

```swift
// 第二层子视图
struct SubChildContentView: View {
    @EnvironmentObject var dataManager: DataManager
    var body: some View {
        List(dataManager.dataList, id: \.self) { data in
            Text(data)
        }
    }
}
```

然后，我们在创建一个和第一层子视图平行的视图，在这个视图里面更新值：

```swift
// 平行子视图
struct BalanceChildContentView: View {
    @EnvironmentObject var dataManager: DataManager
    var body: some View {
        Button(action: {
            dataManager.dataList.append("红楼梦")
        }, label: {
            Text("添加")
        })
    }
}
```

将这个视图放到`ContentView`的`NavigationStack`中，使用一个`VStack`进行管理，即

```swift
 VStack {
          ChildContentView()
          BalanceChildContentView()
 }
```

此时，我们就实现了多个子视图间的数据同步更新了。

