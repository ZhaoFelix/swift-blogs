---
title: SwiftUI实践之创建一个待办清单（二）
tags:
  - SwiftUI
  - 待办清单
categories:
  - 使用SwiftUI创建一个待办清单
abbrlink: 6b5357a8
---

# 首页界面开发与点击交互

项目创建和基础配置介绍完成后，接着我们来完成待办清单的首页开发。

## 创建文件和文件夹

在Xcode左侧项目导航栏中，选择二级目录下的项目名，鼠标右键点击 **New Group** 创建一个名为 **View** 的文件夹：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051422276.png" style="zoom:70%"/>

创建后的项目结构如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051423576.png"/>

这个文件夹将来存储我们之后创建的所有视图代码文件。

<!--more-->

在我们新建的 **View** 文件夹下新建一个名为 **HomeView** 的 SwiftUI文件：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051423667.png" style="zoom:70%"/>

此时的项目结构如下：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051423797.png"/>

## 创建导航栏

对初始文件 `ContentView.swift` 进行修改，修改后的代码如下：

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        NavigationView {
            HomeView()
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

`NavigationView` 是一个导航视图控件，用来配置导航栏文字和按钮。

在 `HomeView.swift` 文件中开始编写如下代码：

```swift
struct HomeView: View {
    var body: some View {
        // 添加一个ZStack布局
        ZStack {
				// TODO: 待创建内容
        }
        .navigationTitle("待办清单") // 导航栏标题
        .navigationBarItems(
            // 导航栏左侧按钮
            leading: EditButton(),
            // 导航栏右侧按钮
            trailing: VStack {
                Image(systemName: "plus")
                    .foregroundColor(.blue) // 图标颜色
                Text("添加")
                    .font(.subheadline) // 字体大小
                    .foregroundColor(.blue) // 字体颜色
            })
    }
}
```

- `ZStack` : Z轴层叠布局；
- `VStack` : 垂直布局；
- `HStack` : 水平布局；
- `Image` : 显示图片控件，经常和 Apple 提供的 **SF Symbols** 配合使用；
- `EditButton` : 编辑按钮；

**此时，预览效果如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051424443.png" style="zoom:60%"/>

> SF Symbols 介绍和下载地址：[SF Symbols - Apple Developer](https://developer.apple.com/sf-symbols/)



## 创建待办列表

在 `ZStack` 的内容部分 `// TODO: 待创建内容` 注释后面使用 `List`  创建一个待办列表：

```swift
List {
                HStack {
                    Image(systemName: "checkmark.circle")
                    Text("Swift学习")
                    Spacer()
                }
                .font(.title2)
                .padding(12) // 内边距
                
                HStack {
                    Image(systemName: "checkmark.circle")
                    Text("健身")
                    Spacer()
                }
                .font(.title2)
                .padding(12) // 内边距
                HStack {
                    Image(systemName: "checkmark.circle")
                    Text("阅读")
                    Spacer()
                }
                .font(.title2)
                .padding(12) // 内边距
                HStack {
                    Image(systemName: "checkmark.circle")
                    Text("聚餐")
                    Spacer()
                }
                .font(.title2)
                .padding(12) // 内边距
 }
```

**此时，预览效果如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051426713.png" style="zoom:60%"/>

在上面的代码中，我们使用复制粘贴的方式使用了大量冗余的代码，接着我们通过自定义视图的方式实现视图的复用，从而减少冗余代码的使用。

在 **View** 文件夹下新建一个名为 `TodoItemView.swift` 的文件，在文件中添加编写下面的代码：

```swift
struct TodoItemView: View {
    var todo:String = "Swift学习" // 待办事项
    var  isChecked: Bool = false // 是否已完成
    var body: some View {
        HStack {
            Image(systemName: isChecked ?  "checkmark.circle" : "circle") // 根据是否完成显示不同的图标
            Text(todo)
            Spacer()
        }
        .font(.title2)
        .padding(12) // 内边距
    }
}
```

**此时，`TodoItemView`的预览效果如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051428146.png" style="zoom: 60%"/>

回到 `HomeView.swift` 视图页面，重新编辑 `List` 的内容：

```swift
List {
     TodoItemView() // 使用默认值
     TodoItemView(todo: "健身", isChecked: true) // 通过传值的方式自定义显示内容和状态
     TodoItemView(todo: "健身", isChecked: true) // 通过传值的方式自定义显示内容和状态
     TodoItemView(todo: "阅读", isChecked: true) // 通过传值的方式自定义显示内容和状态
     TodoItemView(todo: "聚餐", isChecked: true) // 通过传值的方式自定义显示内容和状态
}
```

**此时，预览效果如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051428398.png" style="zoom:60%"/>

可以发现和之前效果是一样的，但是我们的代码变得更加简洁了。实际上，我们还可以使用数组和 `ForEach` 来进一步让我们的代码更加简洁。

在 `body` 上面定义一个字符串类型的数组：

```swift
var todos: [String] = ["SwiftUI 学习", "健身", "阅读", "聚餐"]
```

使用 `ForEach` 循环数据创建列表：

```swift
List {
       ForEach(todos, id: \.self) { todo in
            TodoItemView(todo: todo)
        }
}
```

到这里为止，我们只是简单的创建了一个字符串类型的数组，但是我们会发现所有的代码都是同一个状态，所以接下来我们需要创建一个数据模型来定义待办事项。

新建一个名为 `Model` 的文件夹，在这个文件夹先新一个名为 `Todo.swift` 的 Swift 文件，<span style="color:red">**注意**</span>这里不是SwiftUI文件。

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051429117.png"  style="zoom:30%"/>

**此时的文件结构如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051430250.png" style="zoom:70%"/>

在 `Todo.swift` 文件中定义一个结构体类型数据模型：

```swift
import Foundation
struct Todo: Identifiable {
    let id: String // 唯一ID，ForEach 的时候需要用到
    let title: String  // 待办事项
    let isChecked: Bool // 待办状态
    
    // 初始化方法
    init(id: String = UUID().uuidString, title: String, isChecked: Bool) {
        self.id = id
        self.title = title
        self.isChecked = isChecked
    }
    
}
```

然后，将我们之前字符串数组进行修改：

```swift
var todos: [Todo] = [
        Todo(title: "SwiftUI学习", isChecked: false),
        Todo(title: "健身", isChecked: false),
        Todo(title: "阅读", isChecked: true),
        Todo(title: "聚餐", isChecked: false),
    ]
```

修改 `List` 部分的代码：

```swift
List {
       ForEach(todos) { todo in
          TodoItemView(todo: todo.title, isChecked: todo.isChecked)
        }
}
```

这里我们发现，我们在使用 `ForEach` 循环的时候，不再需要传递 `id` 参数，这是因为我们在定义模型的时候让 `Todo` 继承了 `Identifiable` 协议，并且声明了一个 `id` 成员，同时初始化的时候使用方法 `UUID().uuidString` 让每个元素都自动生成了一个独一无二的ID。

## 数据响应与交互

接下来，我们要实现点击选中和编辑功能。

在SwiftUI中，响应式的数据交互是它的另外一个主要特点。接下来我们使用通过继承 `ObservableObject` 协议来创建一个响应式的数据模型。

在 **Model**  文件夹下新建一个名为 `ListViewModel.swift` 的文件，在这个文件中编辑下面的代码：

```swift
import SwiftUI // 由于我们定义的是一个响应式的数据，所以这里需要引入SwiftUI框架
class ListViewModel: ObservableObject {
    @Published var todos:[Todo] = [
        Todo(title: "SwiftUI学习", isChecked: false),
        Todo(title: "健身", isChecked: false),
        Todo(title: "阅读", isChecked: true),
        Todo(title: "聚餐", isChecked: false),
    ]
}
```

在定义一个响应式数据对象时，我们定义的是一个类即关键字使用的是 `class` ， 然后自定义的这个类需要继承自 `ObservableObject` 协议。对于类中的成员变量，如果需要进行响应式交互，我们需要使用关键字 `@Published` 进行修饰。

接着做以下的几处修改：

1、`TodoItemView.swift` 文件中：

```swift
struct TodoItemView: View {
    var todo: Todo  // 修改1
    var body: some View {
        HStack {
						// 修改2
            Image(systemName: todo.isChecked ?  "checkmark.circle" : "circle") // 根据是否完成显示不同的图标
            Text(todo.title)
            Spacer()
        }
        .font(.title2)
        .padding(12) // 内边距
    }
}
```

对预览文件进行修改，如果不需要预览也可以直接删除：

```swift
struct TodoItemView_Previews: PreviewProvider {
    static var previews: some View {
        TodoItemView(todo: .init(title: "健身", isChecked: false))
    }
}
```

2、 `HomeView.swift` 文件中：

```swift
struct HomeView: View {
    var listModel: ListViewModel = ListViewModel()  // 修改1
    var body: some View {
        // 添加一个ZStack布局
        ZStack {
            List {
							// 修改2
                ForEach(listModel.todos) { todo in
                    TodoItemView(todo: todo)
                }
            }
        }
        .navigationTitle("待办清单") // 导航栏标题
        .navigationBarItems(
            // 导航栏左侧按钮
            leading: EditButton(),
            // 导航栏右侧按钮
            trailing: VStack {
                Image(systemName: "plus")
                    .foregroundColor(.blue) // 图标颜色
                Text("添加")
                    .font(.subheadline) // 字体大小
                    .foregroundColor(.blue) // 字体颜色
            })
    }
}
```

此时的预览或者运行效果和之前的一样。

接下来，我们需要给空间添加交互，例如点击或者滑动。

在 `ListViewMode.swift` 文件中添加一个状态更新函数：

```swift
// 更新完成状态
    func updateTodoStatus(todo: Todo) {
        // 查询ID相同的第一个元素索引，可能为空，所以使用 if-let 进行解析
        if let index = todos.firstIndex(where: {$0.id == todo.id}) {
            todos[index].isChecked = !todos[index].isChecked // 完成状态取反
            print(todo)
            print(todos)
        }
    }
```

给 `TodoItemView` 添加一个 `onTapGesture` 点击手势交互修饰器，在里面实现点击后状态更新的逻辑：

```swift
ForEach(listModel.todos) {  todo in
    TodoItemView(todo: todo).onTapGesture {
           listModel.updateTodoStatus(todo: todo)
       }
 }
```

将项目运行在模拟器上或者在预览视图中直接运行，我们会发现点击后控价并没有发生任何的变化。

这是因为我们在声明一个继承自 `ObservableObject` 协议的类对象时，需要使用`@ObservedObject` 关键字进行修饰，即修改下面的代码：

```swift
@ObservedObject var listModel: ListViewModel = ListViewModel()
```

**此时，再运行我们的项目，点击任意一个待办事项，效果如下**：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202309051431958.mp4" style="zoom:30%" />
