---
title: Swift 中的类
tags:
  - Swift
  - 类
categories:
  - Swift 基础语法
abbrlink: 338306b2
date: 2023-08-29 09:05:07
---

在Swift中，类是一种引用类型，用于封装数据和方法。类是面向对象编程的基础，它们允许我们创建具有特定属性和方法的自定义数据类型。在这篇文章中，我们将详细介绍Swift中的类及其使用方法。

## 类的定义

在Swift中，类使用`class`关键字定义。类的定义包括一个可选的名称、一对大括号`{}`，以及一个或多个属性和方法的定义。类的属性可以是<span style="color:red">**值类型**</span>（例如`Int`、`String`等），也可以是<span style="color:red">**引用类型**</span>（例如数组、字典等）。方法则包含在一个或多个大括号内，并遵循特定的语法规则。

<!--more-->

下面是一个简单的类定义示例：

```swift
class Person {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    func sayHello() {
        print("Hello, my name is \(name) and I am \(age) years old.")
    }
}
```

在这个示例中，我们定义了一个名为`Person`的类，它有两个属性（`name`和`age`）和一个方法（`sayHello`）。`init`方法是一个特殊的方法，用于初始化类的实例。当创建一个新的`Person`实例时，我们需要提供一个名字和一个年龄，然后`init`方法会将这些值分别赋给`name`和`age`属性。

定义好一个类之后，我们可以实例化一个类的对象，然后通过**点语法**去访问类的属性和方法：

```swift
var person = Person(name: "Felix", age: 25)
person.sayHello()
person.age = 26 
```



## 类的构造器和属性访问修饰符

在Swift中，我们可以使用构造器来初始化类的实例。构造器是一个特殊的函数，它在创建类的实例时被调用。构造器的名称与类名相同，并且没有返回类型。我们可以在构造器中设置实例的属性值。

在上面的`Person`类中，我们使用了`init`方法作为构造器。当我们创建一个新的`Person`实例时，需要提供名字和年龄参数，如下所示：

```swift
let person = Person(name: "Alice", age: 30)
```

此外，我们还可以使用访问修饰符来控制属性和方法的访问权限。Swift提供了四种访问修饰符：`public`、`protected`、`internal`和默认（即不使用任何修饰符）。**默认情况下，所有属性和方法都是公开的（可以在任何地方访问）**。通过使用不同的访问修饰符，我们可以限制属性和方法的可见性，从而提高代码的安全性和可维护性。

例如，我们可以将上面的`Person`类修改为以下形式，将所有属性和方法设置为私有：

```swift
class Person {
    private var name: String
    private var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    internal func sayHello() {
        print("Hello, my name is \(name) and I am (age) years old.")
    }
}
```

当我们再想通过实例化的对象访问 `name` 和 `age` 属性时，Xcode 会报以下的错误：

 <span style="color:red">`'age' is inaccessible due to 'private' protection level `</span>



## 类的继承和多态性

在Swift中，类可以继承自其他类，从而实现代码的重用和扩展。子类可以继承父类的属性和方法，并可以根据需要添加新的属性和方法。此外，Swift还支持多态性，这意味着我们可以使用父类的引用来操作子类的对象。

下面是一个关于类继承的例子：

```swift
class Student: Person {
  // 子类独有的属性
    var school: String
    
    init(name: String, age: Int, school: String) {
        self.school = school
        super.init(name: name, age: age)
    }
    
   // 子类独有的方法
    func toSchool() {
        print("Go to school")
    }
}

```

在上面的这个例子中，我们定义了一个`Student` 子类，这个子类继承了`Person` 父类。这里的继承意味着`Student` 子类有父类的`age` 和`name`属性以及`sayHello`方法。

```swift
var student = Student(name: "Felix", age: 26, school: "SBS")
student.toSchool() // 调用字类方法
student.sayHello() // 调用父类方法
```



在子类中，我们还可以重写父类的方法，使用 `override` 关键字：

```swift
 // 重写父类的方法
override func sayHello() {
    print("Hello, my name is\(name) and I am \(age) years old. my school is \(school)")
}
```



