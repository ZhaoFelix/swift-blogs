---
title: Swift 中的枚举
tags:
  - Swift
  - 枚举
categories:
  - Swift 基础语法
abbrlink: 251de72f
date: 2023-09-02 12:40:13
---

### Swift 中的枚举

在计算机编程中，枚举是一种数据类型，用于定义一定范围内的有名称的值。Swift语言中的枚举是强大且灵活的工具，我们可以在多种场景中使用它们，包括但不限于处理特定类型的数据、创建自定义的错误类型以及实现特定的设计模式。

#### 定义和基本用法

在Swift中，我们使用`enum`关键字来定义枚举。下面是一个简单的例子，展示了如何定义和使用一个名为`Weekday`的枚举，表示一周中的每一天：

```swift
enum Weekday {  
    case Monday  
    case Tuesday  
    case Wednesday  
    case Thursday  
    case Friday  
    case Saturday  
    case Sunday  
}
```

<!--more-->

我们可以使用这个枚举来处理与一周中的某天相关的数据。例如，我们可以创建一个函数，根据给定的`Weekday`枚举值返回一周中的相应天数：

```swift
func dayOfWeek(_ weekday: Weekday) -> String {  
    switch weekday {  
    case .Monday:  
        return "Monday"  
    case .Tuesday:  
        return "Tuesday"  
    case .Wednesday:  
        return "Wednesday"  
    case .Thursday:  
        return "Thursday"  
    case .Friday:  
        return "Friday"  
    case .Saturday:  
        return "Saturday"  
    case .Sunday:  
        return "Sunday"  
    }  
}
```

然后我们可以像下面这样使用这个函数：

```swift
let monday = Weekday.Monday  
print(dayOfWeek(monday)) // 输出：Monday
```

#### 关联值

Swift的枚举还可以定义关联值，这使得枚举能够更丰富地表达信息。例如，我们创建一个枚举表示用户的登录状态，并为其关联一个错误消息：

```swift
enum UserLoginStatus {  
    case success(String)  
    case failure(String)  
}
```

在这个例子中，`UserLoginStatus`枚举有两个cases：`success`和`failure`。这两个cases都关联了一个String类型的值，表示成功或失败的状态信息。我们可以像下面这样使用这个枚举：

```swift
let successfulLogin = UserLoginStatus.success("登录成功")  
let failedLogin = UserLoginStatus.failure("登录失败，请检查用户名和密码")  
print(successfulLogin) // 输出：登录成功  
print(failedLogin) // 输出：登录失败，请检查用户名和密码
```

#### 在switch语句中使用枚举

由于Swift的枚举是全特性的，所以在switch语句中可以使用枚举的所有case值。这对于处理多种可能的枚举值非常有用：

```swift
let someWeekday: Weekday = .Wednesday  
switch someWeekday {  
case .Monday:  
    print("今天是星期一")  
case .Tuesday:  
    print("今天是星期二")  
case .Wednesday:  
    print("今天是星期三")  
case .Thursday:  
    print("今天是星期四")  
case .Friday:  
    print("今天是星期五")  
case .Saturday:  
    print("今天是星期六")  
case .Sunday:  
    print("今天是星期日")  
}
```

#### 使用原始值和匿名枚举

有时候，我们可能不需要为枚举的每个case关联一个特定的值，或者我们希望将所有值都关联到一个单一的原始值。在这种情况下，我们可以使用原始值和匿名枚举。例如：

```swift
enum乏味的颜色: Int {  
    case red = 1, green = 2, blue = 3, yellow = 4, orange = 5,  // 可以根据需要添加更多颜色
}
```
