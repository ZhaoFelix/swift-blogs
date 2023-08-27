---
title: Swift 中的集合类型
tags:
  - Swift
  - 集合类型
  - Foundation
  - 数组
  - 集合
  - 字典
categories:
  - Swift 基础语法
abbrlink: 5f278529
date: 2023-08-25 09:17:28
---

Swift 语言提供 `Arrays`、`Sets` 和 `Dictionaries` 三种基本的集合类型用来存储集合数据。**数组（Arrays）**是有序数据的集。**集合（Sets）**是无序无重复数据的集。**字典（Dictionaries）**是无序的键值对的集。

Swift 语言中的 `Arrays`、`Sets` 和 `Dictionaries` 中存储的数据值**类型必须明确**。这意味着我们不能把错误的数据类型插入其中。

<!--more-->

#### 数组

数组使用有序列表存储同一类型的多个值。相同的值可以多次出现在一个数组的不同位置中，这意味着数组中的元素是可以重复的。

##### 创建一个空数组

```Swift
var someInts = [Int]()
var someStr:[String] = []
```

##### 创建一个带有默认值的数组

```Swift
var threeDouble = Array(repeating: 0.3, count: 3)
print(threeDouble)
```

这里我们使用`Array`默认构造方法创建了一个长度大小为3，初始元素值为0.3的双精度类型的数组。参数说明：

- repeating:初始时元素的值
- count：元素的个数或者数组的长度

此时，打印数组的显示如下：

```
[0.3, 0.3, 0.3]
```

##### 访问数组

我们可以根据数组的索引来访问数组中的元素，不过需要注意的一点是数组的索引是从0开始的。

```Swift
var nameArr = ["Felix","Zhao","Rudolf"]
print("第一个元素\(nameArr[0])")
print("第二个元素\(nameArr[1])")
print("第三个元素\(nameArr[1])")
```

##### 修改数组

###### 添加数组元素

我们可以使用`append`方法或者赋值运算符`+=`将一个元素添加到数组的末尾。

```Swift
nameArr.append("Wells")
nameArr += ["Wells"]
```

此时的数组为：

```swift
["Felix", "Zhao", "Rudolf", "Wells", "Wells"]
```

##### 循环遍历数组

我们可以使用`for-in`循环来遍历数组中的每一个元素。

```Swift
for name in nameArr {
    print(name)
}
```

如果我们同时需要每个元素的值和对应的索引，我们可以使用`enumerate()`方法来遍历数组。

```Swift
for (index,name) in nameArr.enumerated() {
    print("当前索引:\(index),当前值：\(name)")
}
```

输出结果如下：

```Swift
当前索引:0,当前值：Felix
当前索引:1,当前值：Zhao
当前索引:2,当前值：Rudolf
当前索引:3,当前值：Wells
```

##### 数组合并

当我们有两个数据类型相同数组需要合并时，可以使用`+`运算符来合并数组。

```swift
var nameArr = ["Felix","Zhao","Rudolf"]
var newNameArr = ["Lily"]
var newArr = newNameArr + nameArr
```

此时，newArr的元素为：

```swift
["Lily", "Felix", "Zhao", "Rudolf"]
```

##### 数组相关属性

```swift
print(nameArr.count) 
print(nameArr.isEmpty) 
```

`.count`用来获取数组元素的个数；`.isEmpty`判断数组是否为空，返回值为布尔值。

#### 字典

Swift中的字典是一个用来存储无序的相同类型数据的集合。字典中每个值(value)对应一个唯一的键(key)，键被作为值在这个字典里的标识符。字典里的数据是没有具体顺序的，我们只能通过值对应的键来访问到这个数据。另外，字典中的键可以是整型或者字符串类型，但是一个字典里键的类型必须是唯一的。

##### 创建字典

Swift中的字典使用`Dictionary<Key,Value>`的形式定义，其中`Key`是字典中键的数据类型，`Value`是字典中值的数据类型。我们使用这样形式来定义一个空的字典：

```swift
var emptyDcit = Dictionary<String,String>()
```

Swift中也给我们提供了一种简化的语法来定义字典：

```swift
var emptyDcit = [String:String]()
```

两种方式都是定义了一个键和值都为字符串类型的空字典，在实际使用过程中，我们更多的是使用后面简化的定义方法。

在项目开发中，如果我们只需要定义一个字典，但是不需要对它进行初始化操作的话，我们可以使用下面的这中方法：

```swift
var dic:[String:String]!
```

##### 字典的访问与修改

我们可以根据键来修改字典中对应的值，我们也可以使用一些内置的方法来添加、删除元素。

```swift
var countryDic:[String:String] = ["JP":"日本","CN":"中国","UK":"英国","FR":"法国"]
print(countryDic["CN"])
print(countryDic["JP"])
```

在上面的实例代码中，我们定义了一个键为国家英文缩写，值为国家的中文简写。我们分别使用键`CN`和`JP`访问字典。

当我们需要字典中添加一个新的元素时候，可以使用以下方法：

```swift
countryDic["IN"] = "印度"
```

我们需要修改字典中的某个元素的值，使用以下方法：

```swift
countryDic["CN"] = "中华人民共和国"
```

我们发现，我们添加新的元素和修改原有元素的值的方法是一样的。这是因为当我们通过键去修改字典的时候，会根据键名先去查询字典中是否包含对应的键名，如果包含的话就修改对应的值，如果不包含就添加新的元素。

经过上面的添加和修改之后，此时的字典元素如下：

```swift
["UK": "英国", "JP": "日本", "FR": "法国", "CN": "中华人民共和国", "IN": "印度"]
```

我们经常会遇到根据键名去删除字典中对应的元素，这时，我们可以使用以下方法：

```swift
countryDic.removeValue(forKey: "CN")
```

我们经常会使用到的方法还有：

```swift
let index = countryDic.index(countryDic.startIndex, offsetBy: 1)
countryDic.remove(at: index)  //根据指定的索引删除元素
countryDic.removeAll() //清空字典
```

##### 字典的遍历

在之前的学习中，我们使用了`for-in`循环来遍历数组，同样的我们也可以使用`for-in`来遍历字典，不同的是遍历字典返回的是每个元素对应的键和值。

```swift
for (key,value) in countryDic {
    print("键：\(key),值：\(value)")
}
```

当然，我们也可以值循环遍历出字典的键或者值。

```swift
for key in countryDic.keys {
    print(key)
}
for value in  countryDic.values {
    print(value)
}
```

另外，在学习数组的时候，我们使用`isEmpty`属性来判断一个数组是否为空，我们同样可以使用这个方法来判断字典是否为空。

#### 集合

Swift 中的集合（Set）是一种无序的、不重复的数据结构，它允许你存储不同类型的元素。集合的主要用途是检查元素是否存在于集合中，因为集合中的**元素是唯一**的，所以当你尝试添加一个已经存在的元素时，集合不会发生变化。

##### 创建一个集合

创建集合的常见方法是使用字面量语法或者初始化器（initializer）。下面是一些关于如何创建和使用集合的示例：

1. 使用字面量语法创建集合：

```swift
let numbers = Set<Int>() // 创建一个空的整数集合
let fruits = Set<String>("apple", "banana", "orange") // 创建一个包含三个字符串元素的集合
```

2. 使用初始化器创建集合：

```swift
var emptySet: Set<Int> = [] // 创建一个空的整数集合
var fruitsSet = Set(["apple", "banana", "orange"]) // 创建一个包含三个字符串元素的集合
var mixedSet: Set<Any> = [1, "apple", 3.14] // 创建一个包含整数和浮点数的混合类型集合
```

##### 集合中元素的访问、修改和删除

向集合中添加新的元素使用`insert` 方法：

```swift
numbers.insert(1) // 添加一个元素到集合中
fruits.insert("grape") // 向集合中添加一个元素，如果元素已经存在，则不会改变集合
```

使用`remove` 方法移除集合中的元素：

```swift
numbers.remove(1) // 从集合中移除一个元素，如果元素不存在，则不会改变集合
fruits.remove("banana") // 从集合中移除一个元素，如果元素不存在，则不会改变集合
```

##### 集合的遍历

集合的遍历和数组的遍历类似，使用 `for-in`循环语句。

```swift
for number in numbers {
    print(number) // 输出集合中的每个元素
}

for fruit in fruits {
    print(fruit) // 输出集合中的每个元素
}
```

##### 检查元素是否存在于集合中

判断一个元素是否存在于集合中可以使用`contains` 方法。

```swift
if fruits.contains("apple") {
    print("苹果存在于集合中") // 如果集合包含指定的元素，则输出相应的信息
} else {
    print("苹果不存在于集合中") // 如果集合不包含指定的元素，则输出相应的信息
}
```

##### 获取集合的大小

获取集合的大小或元素的数量使用`count` 属性。

```swift
let size = fruits.count // 获取集合中的元素个数
```

##### 判断两个集合是否相等（即它们包含相同的元素）

判断两个集合是否相等可以使用`==` 运算符。

```swift
let setA = Set(["apple", "banana", "orange"])
let setB = Set(["apple", "banana", "orange"])
let isEqual = setA == setB // 判断两个集合是否相等，如果相等则返回 true，否则返回 false
```
