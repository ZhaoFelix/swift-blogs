---
title: SwiftUI控件NavigationStack进阶
abbrlink: 307cc34b
date: 2023-10-25 09:27:01
tags: [SwiftUI, NavigationStack, NavigationView]
categories: [SwiftUI 基础]
---

在SwiftUI中，如果我们想要给app添加一个到导航栏，可以使用**NavigationStack**控件，需要注意的是**NavigationStack是iOS 16之后增加的，旨在替代原来的NavigationView，所以如果你的app 运行在iOS 16之前的版本需要使用NavigationView**。

在之前的文章中，我们已经接受了**NavigationStack**的基本使用，接下来我们将主要介绍**NavigationStack**自定义，即自定义导航栏样式。

初始项目代码如下：

```swift
struct ContentView: View {
    var books: [String] = ["三国演义","水浒传", "红楼梦", "西游记"]
    var body: some View {
        NavigationStack {
            List(books, id: \.self) { book in
                NavigationLink {
                    DetailContentView(book: book)
                } label: {
                    Text(book)
                        .bold()
                }
            }
            .navigationTitle("四大名著")
        }
    }
}

struct DetailContentView: View {
    var book: String
    var body: some View {
        Text(book)
    }
}
```

<!--more-->

### 自定义导航栏字体和文字颜色

目前，SwiftUI中并没有提供可以定义导航栏样式的修饰器，这里我们需要使用UIKit提供的`UINavigationBarAppearance` API 来实现。

例如，如果我们想要自定义导航栏上的标题字体和颜色，可以使用下面的方式：

```swift
  init() {
        let navigationBarAppearance = UINavigationBarAppearance()
        // 大标题字体样式
        navigationBarAppearance.largeTitleTextAttributes = [.foregroundColor: UIColor.red, .font: UIFont(name: "Nunito-Regular", size: 30)!]
        // 小标题字体样式
        navigationBarAppearance.titleTextAttributes = [.foregroundColor: UIColor.red, .font: UIFont(name: "Nunito-Regular", size: 20)!]
        
        // 标准样式
        UINavigationBar.appearance().standardAppearance = navigationBarAppearance
        UINavigationBar.appearance().scrollEdgeAppearance = navigationBarAppearance
        UINavigationBar.appearance().compactAppearance = navigationBarAppearance
    }
```

这里我们是在`ContetnView`进行初始化的时候就来设置导航栏的标题，即

```swift
struct ContentView: View {
    var body: some View {
   		
    }
    
    // ContentView的初始化方法
    init() {
  
    }
}
```

### 自定义返回按钮的图片和颜色

如果我们只是想要改变默认的返回按钮的图片和颜色，那么我们只需修改`UINavigationBarAppearance`实例对象的属性即可，即

```swift
 navigationBarAppearance.setBackIndicatorImage(UIImage(systemName: "arrow.turn.up.left"), transitionMaskImage: UIImage(systemName: "arrow.turn.up.left"))
```

如果是改变返回按钮字体的颜色，只需要修改`NavigationStack`强调色或者说添加`tint`修饰器，即

```swift
NavigationStack {
  
}
.tint(.black)
```



但是，如果我们想要完全自定义个返回按钮，就需要先隐藏`NavigationStack`原来默认的按钮，使用SwiftUI 提供的`navigationBarBackButtonHidden`修饰器。

需要注意的是，这个修饰器需要添加到跳转后的子视图上面，在这个例子中就是我们的`DetailContentView`视图，即

```swift
DetailContentView(book: book)
                        .navigationBarBackButtonHidden(true)
```

此时，当我们再点击跳转过去的时候，下一个页面已经没有默认的返回按钮了。



接着，我们可以使用`toolBar`给`DetailContentView`自定义一个返回的按钮，具体实现的代码如下：

```swift
struct DetailContentView: View {
    var book: String
    @Environment(\.dismiss) var dismiss // 通过dismiss 关键字获取dismiss方法，实现返回上一页
    var body: some View {
        Text(book)
            .toolbar {
                ToolbarItem(placement: .cancellationAction) {
                    Button(action: {
                        dismiss()
                    }, label: {
                        Text("\(Image(systemName: "chevron.left")) \(book)")
                            .foregroundStyle(.blue)
                    })
                }
            }
            // 导航栏调整为小标题样式
            .navigationBarTitleDisplayMode(.inline)
    }
}
```

在上面的代码中，我们使用`toolBar`修饰器和`ToolBarItem`控件去定义了一个位于左上角的返回按钮，然后使用`Environment`获取到了`dismiss`方法来帮助我们返回上一页。



需要<span style="color:red">**注意**</span>的一点是，在**iOS 15.0**之前的系统中，想要实现返回上一页需要使用下面的这种方法：

```swift
@Environment(\.presentationMode) var presentationMode
```

然后在按钮的`action`部分调用下面的代码：

```swift
presentationMode.wrappedValue.dismiss()
```

