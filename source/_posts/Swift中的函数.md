---
title: Swift中的函数
abbrlink: 66bf2022
date: 2023-08-28 10:30:24
tags: [Swift, 函数]
categories: [Swift 基础语法]
---

### 认识函数

函数是一段独立的代码块，用来执行一些特定的操作。我们可以通过给函数一个名字来定义它的功能，当我们需要执行这段代码块的时候通过函数的名字来进行调用。

#### 定义和调用函数

定义函数使用关键字**func**,每个函数都需要有一个函数名，它描述函数执行的任务。
下面我们定义一个函数名为`sayHi`的函数：

```swift
func sayHi(){
    print("你好")
}
```

调用函数：

```swift
sayHi()
```

输出结果：

```
你好
```

<!--more-->

#### 带参数和返回值的函数

在一些特定的情况下，我们需要给函数传入一些数据，传入的数据我们称之为**参数**。另外，我们也可能需要函数给我们返回一些代码块执行的结果，这就是**返回值**。

##### 带参数的函数

```swift
func sayHi(name:String){
    print("你好，\(name)")
}
```

在定义带参数的函数时，需要声明参数的数据类型，声明的方式类似于变量的声明，但是不需要加**var**关键字。

调用带参函数：

```swift
sayHi("Felix")
```

> 注意，在调用函数的时候，传入的参数类型要和函数定义时的数据类型一致。上面的实例中参数的数据类型是**String**类型，所以函数调用是我们只能传入一个字符串。

##### 带返回值的函数

```swift
func sayHi() -> String {
    return "你好"
}
```

定义带返回值的函数我们需要使用返回箭头`->`箭头后面跟的是返回值的数据类型。另外，在代码块执行结束后我们需要使用**return**关键字返回数据。

函数调用：

```
let result = sayHi()
```

在调用带返回值的函数时，我们一般会把返回的结果赋给一个常量，以便之后使用。当然我们也可以直接使用**print**直接将结果打印。`print(sayHi())`

##### 同时带参数和返回值的函数

```swift
func sum(a:Int,b:Int) -> Int{
    return a + b
}
```

上面的实例中我们定义来一个求和函数来计算任何两个整数的值，并将求和后的结果返回。另外，这个函数定义了两个参数，所以函数调用的时候需要传入两个参数。

函数调用：

```swift
print(sum(a: 100, b: 24))
```

##### 多返回值函数

```swift
func calculate(a:Int,b:Int) -> (sum:Int,sub:Int){
    let sum = a + b
    let sub = a - b
    return (sum,sub)
}
```

这是一个计算函数，我们分别计算传入的两个参数的和与差，然后将和与差返回。

函数调用：

```swift
let result = calculate(a: 102, b: 23)
print("两数之和为：\(result.sum)")
print("两数之差为：\(result.sub)")
```

输出结果：

```swift
两数之和为：125
两数之差为：79
```

#### 函数的实际参数标签和形式参数名

每个函数的形式参数都包含实际参数标签和形式参数名。实际参数标签在函数调用的时候使用，形式参数名在函数的内部使用。默认情况下，Swift使用形式参数名作为实际参数标签，在上面所有的函数定义中，我们都没有特别声明实际参数标签。

```swift
func calculate(first a:Int, second b:Int) -> (sum:Int,sub:Int){
    let sum = a + b
    let sub = a - b
    return (sum,sub)
}
```

在上面的实例中`second`和`first`就是实际参数标签，`a`和`b`就是形式参数名。

```swift
let result = calculate(first: 102, second: 23)
```

我们注意到，我们在进行函数调用的时候使用的实际参数标签。使用实际参数标签可以帮助我们提高代码的可读性。

##### 省略实际参数标签

在某些情况下，我们可能想要在调用函数的时候省略实际参数标签，那么我们可以借助`_`通配符实现。

```swift
func calculate(_ a:Int, _ b:Int) -> (sum:Int,sub:Int){
    let sum = a + b
    let sub = a - b
    return (sum,sub)
}
```

函数调用：

```swift
let result = calculate(102, 23)
```

可以看到，在函数调用的时候我们已经可以不用写实际参数标签了。
