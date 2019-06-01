---
description: 1. 代码命名约定； 2. 文件命名约定
---

# 命名约定

## 代码命名约定

### 1. 命名空间

类名**不**需要使用Objective-C的规则增加前缀，Swift以模块的名称作为命名空间。

如果来自不同模块的两个名字发生冲突，可以通过在类型名称前面加上模块名称来消除歧义。

### 2. 首字母大写的驼峰命名

对于`struct`, `enum type`, `class`, `typedef`, `associatedtype`, `protocols` 使用首字母大写的驼峰命名。[Pascal Naming Convention](https://en.wikipedia.org/wiki/Naming_convention_%28programming%29#Pascal,_Modula-2_and_Oberon)

### 3. 第一个单词首字母小写的驼峰命名

对于`function`, `method`, `property`, `constant`, `variable`, `argument names`, `enum value` 使用第一个单词首字母小写的驼峰命名。[Camel Case Convention](https://en.wikipedia.org/wiki/Camel_case)

### 4. 缩略语

处理一个缩略语或其他常用大写字母的名称时，要使用使用**完全大写**的命名方式。

{% hint style="info" %}
如果这个单词需要位于以小写字母开始名称的开头，则需要对该缩写词使用**完全小写**的命名。
{% endhint %}

```swift
// "HTML" 作为常量的开始, 需要使用小写的 "html"
let htmlBodyContent: String = "<p>Hello, World!</p>"

// 推荐使用 ID；不推荐使用 Id
let profileID: Int = 1

// 推荐使用 URLFinder；不推荐使用 UrlFinder
class URLFinder { ... }
```

### 5. 常量

所有与实例无关的常量，都需要声明为`static`，以避免为每一个实例都声明一个常量。

这些常量应该放在`class`或`enum`的`// Mark:` 之后，或者放在一个不带大小写的枚举`enum`中。使用枚举的优点是不会被意外的实例化，而且可以作为命名空间使用。

```swift
class MyClassName {
        // MARK: - Constants
        static let buttonPadding: CGFloat = 20.0
        static let indianaPi = 3
        static let shared = MyClassName()
}
```

```swift
class MyClassName {
    enum Constants {
        static let buttonPadding: CGFloat = 20.0
        static let indianaPi = 3
    }
}
```

### 6. 常量范围

尽量在常量的使用范围内声明常量，不必把所有的常量都声明在共享的常量文件内。

### 7. 避免模糊名称

避免类，变量等的模糊命名，尽量做到见名知意。

例如`RoundAnimatingButton`比`CustomButton`要更容易理解

### 8. 避免缩写

避免使用单一名称或缩写。

```swift
// 推荐使用
let popUpViewController: UIViewController
// 🙅‍♂️不推荐
let popupVC: UIViewController

// 推荐使用
let animationDuration: TimeInterval
// 🙅‍♂️不推荐
let animDur: TimeInterval
```

### 9. 命名中包含类型

只有在名称不明确的情况下才需要在命名中增加类型信息。

```swift
class Person {
    // 名字明显是'String'类型，所以可以省略类型'String'
    let firstName: String
    // 当类型不够明显时，可以添加类型名称'ImageView'
    let personImageView: UIImageView
}
```

### 10. 名称之后添加类型

使用`IBOutlet`时，在名称结尾添加类型。

```swift
// 推荐使用
@IBOutlet weak var submitButton: UIButton!
@IBOutlet weak var emailTextField: UITextField!

// 🙅‍♂️不推荐
// 类型在开头且是缩写
@IBOutlet weak var btnSubmit: UIButton!
// 类型必须放在结尾
@IBOutlet weak var buttonSubmit: UIButton!
```

### 11. 函数参数命名

函数参数命名，需要确保函数易于阅读，以便理解每个参数的目的。

### 12. 协议命名

协议应该被命名为名词；如果不能，也可以在协议中添加`Protocol`后缀。

## 文件命名约定

### 1. 单一类型

只包含单一类型如`MyType`的文件命名为`MyType.Swift`。

### 2. 单一类型加一些辅助函数

包含`MyType`类型和一些上层的辅助函数（非主要实体）也可以命名为`MyType.Swift` 。

### 3. 单一扩展

为`MyType`类型扩展添加遵循了协议`MyProtocol`，这个文件可以被命名为`MyType+MyProtocol.swift` 。

### 4. 多个扩展

当为某个类型添加了多个扩展或辅助函数，可以使用`MyType+`为前缀，使文件名称具有通用性。如`MyType+Additions.swift`。

### 5. 描述性命名

当一些声明不在公共类型或命名空间下，可以使用描述性的方式命名。

如全局数学函数的集合可以命名为`Math.Swift`。







