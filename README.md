# 代码格式

## 1. 缩进

缩进必须使用制表符 `tab` ，每个制表符是 **4** 个空格宽度

{% hint style="info" %}
Xcode设置：_Xcode &gt; Preferences &gt; Text Editing &gt; Indentation_
{% endhint %}

## 2. 单行长度

避免一行代码超过**160** 个字符

{% hint style="info" %}
Xcode设置：_Xcode &gt; Preferences &gt; Text Editing &gt; Page Guide Column_
{% endhint %}

## 3. 删除行尾空格

启用自动删除行尾空格

{% hint style="info" %}
Xcode设置：_Xcode &gt; Preferences &gt; Text Editing &gt; Automatically trim trailing whitespace & Including whitespace-only lines_
{% endhint %}

## 4. 左括号放在代码后面

禁止左括号单独一行

```swift
class MyClass {
    func myMethod() {
        if x == y {
            /* ... */
        } else if x == z {
            /* ... */
        } else {
            /* ... */
        }
    }
    /* ... */
}
```

## 5. 冒号前**不**需要空格

在属性，常量，变量，字典的`key` , 函数参数，协议， 类等类型编写时，不要在冒号前加空格

```swift
// 属性类型
let aProperty: MyPropertyClass

// 字典
let aDictionary: [String: Int] = [
  "key1": 2,
  "key2": 3
]

// 函数
func aFunction<T, U: SomeProtocol>(argA: U, argB: T) where T.RelatedType == U {
      /* ... */
}

// 函数调用
myFunction(argument: "MyArgument")

// class
class Dog: Wolf {
      /* ... */
}

// 协议
extension PirateViewController: UITableViewDataSource {
      /* ... */
}
```

## 6. 逗号后面需要空格

在数组元素， 条件语句等逗号后面需要增加空格

```swift
// 数组
let anArray = ["a", "b", "c"]

// 条件语句
if a == b, b == c {
        /* ... */
}
```

## 7. 操作符前后需要空格

在操作符 \(`+`, `-`, `==`, `>` ...\) 前后都需要有一个空格,

`(`**之前**需要有一个空格，`)`**之后**需要有一个空格

```swift
let myValue = 20 + (30 / 2) * 3
if 1 + 1 == 3 {
    /* ... */
}
```

## 8. 多行缩进

当一个函数或`if`语句跨多行时，使用`Xcode`的推荐规则进行缩进

```swift
// 当一个函数跨行时，使用 Xcode 缩进规则缩进
func myFunction(parameterOne: String,
                parameterTwo: String,
                parameterThree: String) {
    // Xcode indents to here for this kind of statement
    print("\(parameterOne) \(parameterTwo) \(parameterThree)")
}

// if 语句跨跨行时，使用 Xcode 缩进规则缩进 逻辑运算符换行放在行首
if myFirstValue > (mySecondValue + myThirdValue)
	&& mySecondValue == myThirdValue
	&& myThirdValue == (mySecondValue - myFirstValue) {
	// Xcode indents to here for this kind of statement
	print("Hello, World!")
}
```

## 9. 多个参数的函数调用

当调用一个多个参数的函数或者一行很长时，把每个参数放在单独一行

```swift
myAwesomeFunctionWithLotsOfArgs(
    firstArgument: "Hello, I am a string",
    secondArgument: "Test",
    thirdArgument: 2)
```

## 10. 数组，字典元素过多

当数组，字典包含元素太多时， 需要考虑将其拆分为多行，并将 `[`和`]`视为函数或`if`语句中的`{`和`}` 。\(闭包也是同等方式处理\)

```swift
someFunctionWithABunchOfArguments(
    someStringArgument: "hello I am a string",
    someArrayArgument: [
        "dadada daaaa daaaa dadada daaaa daaaa dadada daaaa daaaa",
        "string one is crazy - what is it thinking?"
    ],
    someDictionaryArgument: [
        "dictionary key 1": "some value 1, but also some more text here",
        "dictionary key 2": "some value 2"
    ],
    someClosure: { parameter1 in
        print(parameter1)
    })
```

## 11. 空数组和空字典

对于空数组和空字典，在声明时需要指定类型

```swift
// 指定类型 推荐方式
var names: [String] = []
var lookup: [String: Int] = [:]

// 类型推导 🙅‍♂️不推荐
var names = [String]()
var lookup = [String: Int]()
```

## 12. 使用变量拆分过多的条件判断

当`if`语句的条件过多时，使用变量拆分条件

例如 使用如下代码👇

```swift
let firstCondition = x == firstReallyReallyLongPredicateFunction()
let secondCondition = y == secondReallyReallyLongPredicateFunction()
let thirdCondition = z == thirdReallyReallyLongPredicateFunction()
if firstCondition && secondCondition && thirdCondition {
    // do something
}
```

替换下面的代码👇

```swift
if x == firstReallyReallyLongPredicateFunction()
    && y == secondReallyReallyLongPredicateFunction()
    && z == thirdReallyReallyLongPredicateFunction() {
    // do something
}
```

## 13. 控制流语句避免使用括号

在`Swift`标准中，不需要在控制流语句中使用括号

```swift
// 推荐
if x == y {
    /* ... */
}

// 🙅‍♂️不推荐
if (x == y) {
    /* ... */
}
```

## 14. 关注点分离

方法之间需要有一行空行，有助于组织和查看代码。

方法或函数里面，功能点之间需要增加一行空行。

如果方法内部有太多的空行，意味着这个函数应该重构为几个不同的函数。

## 15. 访问控制

| 关键字 | 访问权限 |
| :---: | :---: |
| private | 私有访问（作用域内有效） |
| fileprivate | 文件内私有 |
| internal | lib或项目内部访问（默认） |
| public | 公开（作为lib库时，可访问，不可继承或重写） |
| open | 开放（作为lib库时，可访问，也可继承或重写） |

使用`private`控制`extension`中非共享的实现细节

```swift
private extension OneViewController {
    /* ... */
}
```

使用`public private(set)`实现只读属性接口

```swift
public private(set) var property: String = ""
```

## 16. 自由函数

谨慎使用自由函数（不关联任何类或类型的函数），最好使用方法，这样有更好的可读性和可扩展性。

## 17. 分号

`Swift`不需要在每个代码语句后使用分号；

当一行有多条语句时，需要使用分号，但是不推荐在一行写多条语句。

## 18. 表情符号

不推荐在代码中使用表情符号`emoji`

## 19. 分隔代码

使用`// MARK:`分隔方法，属性，类

