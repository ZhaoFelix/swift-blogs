---
title: SwiftUI之使用 Combine 和 ViewModel 构建一个注册页面
tags:
  - SwiftUI
  - Combine
  - MMVM
  - Form
categories:
  - SwiftUI 基础
abbrlink: cd699355
date: 2023-11-01 09:25:49
---

在 SwiftUI 中，**Combine** 是**声明式UI**的核心关键。接下来，我们将构建一个包含三个`TextField`的注册页面，以及实现常见表单验证来学习和掌握**Combine**的基础原理。

### 构建基础页面

首先，我们构建下面的一个基础界面：

<img src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311011348656.png" style="zoom:10%"/>

<!--more-->

`ContentView.swift`视图部分：

```swift
struct ContentView: View {
    @State var userName: String = ""
    @State var password: String = ""
    @State var confirmationPassword: String = ""
    
    var body: some View {
        VStack {
            Text("创建账号")
                .font(.system(.largeTitle, design: .rounded))
                .bold()
                .padding(.bottom, 30)
            FormTextField(fieldValue: $userName, fieldName: "用户名")
            RequirementText(text: "用户名不能少于四个字符")
                .padding()
            FormTextField(isSecure: true,fieldValue: $password, fieldName: "密码")
            RequirementText(iconName: "lock.open", text: "密码至少为8个字符",isStrikeThrough: true)
                .padding()
            
            FormTextField(isSecure: true, fieldValue: $confirmationPassword, fieldName: "二次确认密码")
            RequirementText(text: "密码前后输入必须一致",isStrikeThrough: false)
                .padding()
                .padding(.bottom, 50)
            SignInButton()
            FooterView()
            Spacer()
        }
        .padding()
    }
}
```

`FormTextField`视图：

```swift
// 表单输入框
struct FormTextField: View {
    var isSecure: Bool = false
    @Binding var fieldValue: String
    var fieldName: String = ""
    var body: some View {
        VStack {
            if isSecure {
                SecureField(fieldName, text: $fieldValue)
                    .font(.system(size: 20, weight: .semibold, design: .rounded))
                    .padding(.horizontal)
            } else {
                TextField(fieldName, text: $fieldValue)
                    .font(.system(size: 20, weight: .semibold, design: .rounded))
                    .padding(.horizontal)
            }
            Divider()
                .frame(height: 1)
                .background(Color(red: 240/255, green: 240/255, blue: 240/255))
                .padding(.horizontal)
        }
    }
}

```

`RequirementText`视图：

```swift
struct RequirementText: View {
    var iconName:String = "xmark.square"
    var iconColor: Color = Color(red: 251/255, green: 128/255, blue: 128/255)
    var text: String = ""
    var isStrikeThrough: Bool = false  //是否划线
    
    var body: some View {
        HStack {
            Image(systemName: iconName)
                .foregroundStyle(iconColor)
            Text(text)
                .font(.system(.body, design: .rounded))
                .foregroundStyle(.secondary)
                .strikethrough(isStrikeThrough)
            Spacer()
        }
    }
}
```

`SignInButton`视图：

```swift
// 登录按钮
struct SignInButton: View {
    var body: some View {
        Button(action: {
            
        }, label: {
            Text("登录")
                .font(.system(.body, design: .rounded))
                .foregroundStyle(.white)
                .bold()
                .padding()
                .frame(minWidth: 0, maxWidth: .infinity)
                .background(LinearGradient(gradient: Gradient(colors: [Color(red: 251/255, green: 128/255, blue: 128/255), Color(red: 253/255, green: 193/255, blue: 104/255)]), startPoint: .leading, endPoint: .trailing))
                .clipShape(RoundedRectangle(cornerRadius: 10))
                .padding(.horizontal)
        })
    }
}
```

`FooterView`视图：

```swift
// 底部视图
struct FooterView: View {
    var body: some View {
        HStack {
            Text("已有账号?")
                .font(.system(.body, design: .rounded))
                .bold()
                .padding()
            Button(action: {
                
            }, label: {
                Text("注册")
                    .font(.system(.body, design: .rounded))
                    .bold()
                    .foregroundStyle(Color(red: 251/255, green: 128/255, blue: 128/255))
            })
        }
        .padding(.top, 50)
    }
}
```

###  初识Combine 

>The Combine framework provides a declarative Swift API for processing values over time. These values can represent many kinds of asynchronous events. Combine declares *publishers* to expose values that can change over time, and *subscribers* to receive those values from the publishers.
>
>- The [`Publisher`](https://developer.apple.com/documentation/combine/publisher) protocol declares a type that can deliver a sequence of values over time. Publishers have *operators* to act on the values received from upstream publishers and republish them.
>- At the end of a chain of publishers, a [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) acts on elements as it receives them. Publishers only emit values when explicitly requested to do so by subscribers. This puts your subscriber code in control of how fast it receives events from the publishers it’s connected to.
>
>Several Foundation types expose their functionality through publishers, including [`Timer`](https://developer.apple.com/documentation/foundation/timer), [`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter), and [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession). Combine also provides a built-in publisher for any property that’s compliant with Key-Value Observing.
>
>You can combine the output of multiple publishers and coordinate their interaction. For example, you can subscribe to updates from a text field’s publisher, and use the text to perform URL requests. You can then use another publisher to process the responses and use them to update your app.
>
>By adopting Combine, you’ll make your code easier to read and maintain, by centralizing your event-processing code and eliminating troublesome techniques like nested closures and convention-based callbacks.

以上是 Apple 关于**Combine**的介绍，根据这个介绍，我们可以知道**Publisher** 和**Subscriber**是**Combine**中两个重要的元素。

在**Combine**中，**Publisher(发布者)**发送事件，**Subscriber(订阅者)**订阅后接受来自**Publisher**的值。以`TextField`为例，当使用**Combine**时，每一次的键盘输入都会触发一个值变化的事件，然后**Subscriber**会去监听这些值的的变化，接收到这些值的变化后，就可以做进一步的操作，例如验证。

接着，我们来写一个验证器来对整个注册的表单进行验证。

```swift
class FormValidator: ObservableObject {
    @Published var isReadySubmit: Bool = false
}
```

### Combine 和 MVVM

**MVVM**的全称是**Model-View-ViewModel**，它是一种开发中常用的设计模式。在实际的 app 开发中，随着项目的越来越复杂，我们不会推荐把所有的内容都放在一个单一的视图中。一般情况下，我们会将视图拆分为**View**和**View Model**两个部分。

* **View**主要负责界面的布局；
* **View Model**负责持有视图的状态和数据。

在我们即将实现的这个例子中，**View Model**持有的数据是：

* UserName 
* Password
* Password confirm 

以及一些状态：

* 用户名不能少于 4 个字符；
* 密码不能少于 8 个字符；
* 密码只能包含小写字母；
* 前后两次密码的输入必须一致

综上所述，这里的**View Model**将有 7 个属性，并且每一个属性发生变化是都会通知它的订阅者。

```swift
class FormValidator: ObservableObject {
    // 输入
    @Published var username: String = ""
    @Published var password: String = ""
    @Published var passwordConfirm: String = ""
    
    //输出
    @Published var isUsernameLengthValid: Bool = false
    @Published var isPasswordLengthValid: Bool = false
    @Published var isPasswordCapitalLetter: Bool = false
    @Published var isPasswordConfirmValid: Bool = false
}
```

### 表单验证

给`FormValidator`添加一个无参的初始化方法，在这个方法里面对各个属性进行验证和赋值。

`username`的验证：

```swift
     $username
            .receive(on: RunLoop.main)
            .map { username in
                return username.count >= 4
            }
            .assign(to: \.isUsernameLengthValid, on: self)
```

在上面的代码中， `$username`是我们想要监听的值，因为我们是正在订阅一个 UI 的变化，所以我们调用了`receive(on:)`方法来确保订阅者能接收到来自主线程的值，即`RunLoop.main`。

接着，我们使用`map`这个高阶函数来判断字符串的长度是否满足要求，如果满足则返回`true`，否则返回`false`。

最后就是使用**Combine**提供的内置订阅者`assign`将判断的结果转换给到我们的`isUsernameLengthValid`属性。

同理，针对`password`和`passwordConfirm`的验证如下：

```swift
  // 验证长度是否满足
        $password
            .receive(on: RunLoop.main)
            .map{ password in
                return password.count >= 8
            }
            .assign(to: \.isPasswordLengthValid, on: self)
        
        $password
            .receive(on: RunLoop.main)
            .map { password in
                // 使用正则判断
                let pattern = "[A-Z]"
                if let _ = password.range(of: pattern, options: .regularExpression) {
                    return true
                } else {
                    return false
                }
            }
            .assign(to: \.isPasswordCapitalLetter, on: self)
```

针对密码的二次验证，使用下面的方式：

```swift
Publishers.CombineLatest($password, $passwordConfirm)
            .receive(on: RunLoop.main)
            .map{ (password, passwordConfirm) in
                return !passwordConfirm.isEmpty && (passwordConfirm == password)
            }
            .assign(to: \.isPasswordConfirmValid, on: self)
```

另外，对于`assign`方法，它返回的是一个`cancellable`的实例对象，我们能够使用这个对象在合适的时候需要这个订阅。这里，我们还需要将返回的每一个实例对象进行存储，即调用`store`方法。

```swift
private var cancellableSet: Set<AnyCancellable> = []
```

将每一个实例对象存储到这个集合中：

````swift
.store(in: &cancellableSet)
````



### 应用定义好的 ViewModel 管理状态

回到我们的视图部分，将之前使用`@State`定义的状态值替换为使用`ViewModel`来管理。

修改`ContentView.swift`中的代码：

```swift
struct ContentView: View {
    @ObservedObject private var formValidator: FormValidator = FormValidator()
    
    var body: some View {
        VStack {
            Text("创建账号")
                .font(.system(.largeTitle, design: .rounded))
                .bold()
                .padding(.bottom, 30)
            FormTextField(fieldValue: $formValidator.username, fieldName: "用户名")
            RequirementText(iconColor:formValidator.isUsernameLengthValid ? Color.secondary : Color(red: 251/255, green: 128/255, blue: 128/255) , text: "用户名不能少于四个字符", isStrikeThrough: formValidator.isUsernameLengthValid)
                .padding()
            FormTextField( isSecure: true,fieldValue: $formValidator.password, fieldName: "密码")
            VStack {
                RequirementText(iconName: "lock.open", iconColor: formValidator.isPasswordLengthValid ? Color.secondary : Color(red: 251/255, green: 128/255, blue: 128/255), text: "密码至少为8个字符",isStrikeThrough: formValidator.isPasswordLengthValid)
                RequirementText(iconName: "lock.open", iconColor: formValidator.isPasswordLengthValid ? Color.secondary : Color(red: 251/255, green: 128/255, blue: 128/255), text: "密码至少包含一个大写字母",isStrikeThrough: formValidator.isPasswordCapitalLetter)
            }
            .padding()
            FormTextField(isSecure: true, fieldValue: $formValidator.passwordConfirm, fieldName: "二次确认密码")
            RequirementText( iconColor: formValidator.isPasswordConfirmValid ? Color.secondary : Color(red: 251/255, green: 128/255, blue: 128/255) ,text: "密码前后输入必须一致",isStrikeThrough: formValidator.isPasswordConfirmValid)
                .padding()
                .padding(.bottom, 50)
            SignInButton()
            FooterView()
            Spacer()
        }
        .padding()
    }
}
```

此时，效果如下：

<video controls="controls" width="20%" src="https://swift-blogs.oss-cn-shanghai.aliyuncs.com/202311011639411.mp4"></video>

