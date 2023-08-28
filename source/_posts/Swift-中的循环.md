---
title: Swift 中的循环
abbrlink: 73894dfa
date: 2023-08-27 10:19:37
tags: [Swift, 循环]
categories: [Swift 基础语法]
---

Swift提供了多样化的控制流语句。包括**while**循环；**for-in**循环；`if`，`guard`和`switch`语句用来基于特定的条件执行不同的代码分支。

<!--more-->

#### for-in 循环

使用**for-in**循环来遍历数组，指定范围内的数字或者字符串中的字符。

```swift
let names = ["Felix","Rudolf","Zora"]
for name in names {
    print("你好，\(name)！")
}
```

输出结果：

```swift
你好，Felix！
你好，Rudolf！
你好，Zora！
```

我们也可以使用**for-in**循环来遍历字典，遍历字典时我们可以同时遍历出字典中的键和值。

```swift
let persons = ["Felix":23,"Rudolf":34,"Zora":33]
for (name,age) in persons {
    print("\(name) 今年\(age)岁！")
}
```

输出结果：

```swift
Rudolf 今年34岁！
Felix 今年23岁！
Zora 今年33岁！
```

循环打印出指定范围内的数字：

```swift
//循环打印出1到5的整数
for i in 1...5{
    print(i)
}
```

> 在上面的数字循环中我们通过**区间运算符**来辅助实现数字的循环。

另外，我们在循环时for后面跟着的是一个被隐式声明的常量，我们不再需要使用let关键字再次进行声明。

在某些情况下，当我们只需要指定循环的次数，并不需要用到隐式声明的常量时，我们可以使用通配符“_”来省略声明的常量。

```swift
//i自加5次
var i = 0
for _ in 0...5{
    i += 1
}
print(i)
```

#### while循环

**while**循环通过判断条件执行分支代码。当条件为true时，执行循环内的代码，否则不行。

```swift
let a = 1
let b = 2
while a < b {
    print("a小于b")
}
```

输出结果：

```
a小于b
```

##### repeat-while

**repeat-while**是while循环的另一种形式，它相当与其他编程语言中的**do-while**循环，在判断循环条件之前它会先去执行一次循环代码块，然后重复循环直到判断条件为false。

```swift
let a = 1
let b = 2
repeat {
    print("a小于b")
}
while a < b
```
