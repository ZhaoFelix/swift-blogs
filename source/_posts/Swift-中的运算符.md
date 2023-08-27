---
title: Swift 中的运算符
tags:
  - Swift
  - 运算符
categories:
  - Swift 基础语法
abbrlink: c2a3f429
date: 2023-08-24 18:04:41
---


### Swift 中的运算符

运算符是一个符号，用于告诉编译器执行一个数学或逻辑运算。

#### 专门用语

运算符包括一元、二元、三元：

- 一元运算符对一个目标进行操作，例如-a。
- 二元运算符对两个目标进行操作，例如a+b。
- 三元运算符操作三个目标，Swift语言有仅只有一个三元运算符（a ? b : c）。

Swift提供来以下几种运算符：

- 算术运算符
- 比较运算符
- 逻辑运算符
- 位运算符
- 赋值运算符
- 区间运算符
- 其他运算符

<!--more-->

#### 算术运算符

Swift提供来四种标准的算术运算符：

- 加（+）
- 减（-）
- 乘（*）
- 除（/）

```swift
1 + 2 //equals 3
5 - 3 //equals 2
2 * 3 //equals 6
10 / 2  //equals 5
```

加法运算符同时支持String的拼接：

```swift
"Hello ," + "World" //equals "Hello, World"
```

##### 比较运算符

Swift支持所有C的所有标准比较运算符：

- 相等 （a == b）
- 不像等（a != b）
- 大于（a > b）
- 小于（a < b）
- 大于等于（a >= b）
- 小于等于（a <= b）

每个比较运算符都会返回一个Bool来表示比较的结果是否为真：

```swift
1 == 2 //false
2 != 3 //true
2 > 1  //true
2 < 1 //false 
1 >= 1 //true
2 <= 3 //true
```

> 注意：Swift同时也提供两个等价运算符（=== 和 !== ）,你可以使用它们来判断两个对象的引用是否相同。

#### 逻辑运算符

逻辑运算符可以修改或者合并布尔逻辑值true和false。Swift提供 以下三种标准的逻辑运算符：

- 逻辑非 （ !a ）：对a的布尔值取反；
- 逻辑与 （ a&&b ）：若a和b均为true,则结果为true；若a和b中有一个或两个都为false，则结果为false。
- 逻辑或 （ a||b ）：若a和b其中一个为true，则结果为true。

```swift
var a:Bool = true 
var b:Bool = false
!a  //false
a&&b false
a||b true
```

#### 位运算符

位运算符用来对二位进制位进行操作，Swift提供以下几种位运算符：

- 取反（～）
- 按位与（&）
- 按位或（|）
- 按位异或（^）

| p    | q    | p&q  | p    | q    | p^q  |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    | 0    | 1    | 1    | 1    |
| 0    | 0    | 0    | 0    | 0    | 0    |

#### 赋值运算符

赋值运算符（a = b）可是初始化或者更新a的值为b:

```swift
let b = 10
var a = 5
a = b //此时，a的值为10
```

除了简单的赋值运算符，Swift还提供了组合赋值运算符。

```swift
let a = 10 
let b = 10
var c = 0

c += a //  c = c+a
c -= a  // c = c - a
c *= a // c = c * a
c /= a  // c = c / a
c %= a  // c = c % a
```

#### 区间运算符

Swift包含了两个区间运算符，他们表示一个范围的值的便捷方式。

##### 闭区间运算符（a...b）定义了从a到b的一组范围，并且包含了a和b。a的值不能大于b。

```swift
for i in 1...5{
    print(i)  //循环打印出1～5的整数，包括1和5
}
```

> 关于循环控制流的内容将在后续内容中出现。

##### 半开区间运算符

半开区间运算符（a..<b）定义了从a到b但不包括b的区间。

```swift
for i in 1...5{
    print(i)  //循环打印出1～4的整数,包括4，不包括5
}
```

##### 单侧区间

单侧区间主要用在数组的遍历。当我们需要遍历数组中的指定索引前或者后的元素时可以使用单侧区间。

```swift
//遍历数组items索引2后的所有元素
for item in items[2...]{
    print(item) 
}
//遍历数组items索引2前的所有元素
for item in items[...2]{
    print(item) 
}

//遍历数组items索引2前的所有元素,不包括索引2的元素
for item in items[..<2]{
    print(item) 
}
```

#### 三元运算符

三元条件运算符是一种有三部分的特殊运算符，它类似于 question ? answer1 : answer2。如果question为真，则会判断answer1并且返回它的值；否则它判断为answer2并且返回它的值。

```swift
if question {
    answer1
}
else {
    answer2
}
```
