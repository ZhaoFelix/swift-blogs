---
title: Swift中的条件语句
abbrlink: 964c101d
date: 2023-08-27 10:15:16
tags: [Swift, 条件语句]
categories: [Swift 基础]
---

Swift提供了**if**和**switch**两种条件语句，`if`语句用来判断简单的条件，`switch`语句适合复杂的条件。

<!--more-->

#### if语句

当只有一个单一的条件时：

```swift
let a = 8
if(a<10){
    print("a小于10")
}
```

**if**语句也提供else分句，当条件为false时使用：

```swift
let a = 8
if(a<10){
    print("a小于10")
}
else {
    print("a大于或等于10")
}
```

当需要判断多个条件时，可以使用**else-if**配合if语句使用：

```swift
let a = 8
if(a<10){
    print("a小于10")
}
else if(a>10){
    print("a大于10")
}
else {
    print("等于10")
}
```

#### switch语句

每一个 `switch` 语句都由多个可能的情况组成，每一个情况都以 case 关键字开始。

```swift
let A = "a"
switch A {
case "q":
    print("常量A等于字符串q")
case  "a":
    print("常量A等于字符串a")
default:
    print("其他字符")
}
```

输出结果：

```
常量A等于字符串a
```

`switch`语句要求仅可能的提供所有可能的值，但是当我们无法对所有可能的情况进行判断时，我们可以时关键字**default**进行标记，这表示其他没有提供的情况执行这部分的代码块。

##### 多条件的匹配：

```swift
let A = "b"
switch A {
case "q":
    print("常量A等于字符串q")
case  "a","b":
    print("常量A等于字符串a或b")
default:
    print("其他字符")
}
```

在上面的代码中，我们在`case`后面添加了一个新的条件，条件之间使用逗号隔开。当常量A等于"a"或"b"时执行的结果都是：

```
常量A等于字符串a或b
```

##### 区间匹配：

`switch`语句也可以配个区间运算符一起使用。

```swift
let a = 23
switch a {
case ..<0:
    print("a小于0")
case 0..<10:
    print("a大于等于0小于10")
case 10..<100:
    print("a大于等于10小于100")
default:
    print("a大于或等于100")
}
```

输出结果：

```swift
a大于等于10小于100
```
