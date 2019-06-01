# 编码风格

## 一般规则

### 1. 集合转换

从一个集合转换到另一个集合时，尽量使用map、filter、reduce和其他一些类似函数式编程操作符组合，避免使用迭代的方式。

{% hint style="info" %}
在使用这些方法时避免使用有副作用的闭包。
{% endhint %}

```swift
// ✅推荐使用
let stringOfInts = [1, 2, 3].flatMap { String($0) }

// ❌不推荐
var stringOfInts: [String] = []
for integer in [1, 2, 3] {
    stringOfInts.append(String(integer))
}
```

```swift
// ✅推荐使用
let evenNumbers = [4, 8, 15, 16, 23, 42].filter { $0 % 2 == 0 }

// ❌不推荐
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {
    if integer % 2 == 0 {
        evenNumbers.append(integer)
    }
}
```

### 2. 代理弱引用

在`delegate`和`protocol`中，确保生命周期，属性使用`weak`。

### 3. 闭包弱引用

在闭包中避免循环引用

```swift
myFunction() { [weak self] (error) -> Void in
    // 使用可选值
    self?.doSomething()

    // 或者使用 'guard let' 解包
    guard let strongSelf = self else {
        return
    }

    strongSelf.doSomething()
}
```

### 4. 类型推断

不要使用类方法的简写，因为从类方法比枚举推断上下文通常更困难。

### 5. 方法重写

在编写方法时，需要考虑是否需要重写，如果不需要，可以使用`final`显示的标记，这样可以提高编译时间。

### 6. 类方法和类属性

在声明与类关联的函数或属性时，首选是`static` 而不是`class`。

当我们需要在子类中重写该函数或方法时才使用`class`。

{% hint style="info" %}
可以考虑使用`Protocol`实现这一点。
{% endhint %}

### 7. 计算属性

当一个函数没有参数，仅仅是返回一些对象或值，最好使用计算属性。

```swift
// ✅推荐使用
public var fullName: String {
    return "\(self.name) \(self.surname)"
}

// ❌不推荐
function fullName() -> String {
    return "\(self.name) \(self.surname)"
} 
```

## 变量

### 1. `let` 和`var` 

尽量使用`let`声明变量，只有当变量确实需要改变时，才需要声明为`var`。

### 2. 属性值

如果属性的值直到被创建时才知道，那么仍然可以使用`let`：在初始化期间分配常量属性。

### 3. 类型推导

如果常量或变量能够自动推导类型， 最好不要显示的声明类型。

### 4. 具有描述性

声明变量时，应该具有描述性，不需要太详细。

```swift
// Dictionary 是多余的
var someDictionary: Dictionary = [String: String]()
// CGPoint 重复
var somePoint: CGPoint = CGPoint(x:100, y: 200)
// b 不具有描述性
var b = Bool(false)
```

### 5. 避免重复

避免重复类型信息，一次就够了。

## 常量

### 1. 私有常量

在可能的情况下，与之相关的文件保持常量是私有的。

### 2. 定义常量

为代码中不变的数据定义常量。

例如：单元格复用标识符的字符串常量，`key`名称（KVC 或者 字典），`segue`标识符。

### 3. 静态常量

如果常量是在类或者结构体中声明的，则必须将其声明为`static`，以避免为每个实例声明一个常量。

### 4. 不必使用`K`前缀

`Swift`中常量不必使用`K`作为前缀。

### 5. 文件级常量

文件级常量必须使用`fileprivate let`声明。

## 元组

### 1. 函数返回值

如果一个函数返回多个值，可以使用命名元组。

如果返回类型很明显，可以省略命名。

如果一个元组中有3个或3个以上的项，需使用类或结构体替换。

### 2. 重定义

如果不止一次的使用某个元组，需要使用`typealias`定义。

如`public typealias Address = (name: String, number: Int)`

## 访问修饰符

### 1. 顺序

访问修饰符必须放在声明属性之前。

```swift
// ✅推荐使用
private static let myPrivateVar: Int

// ❌不推荐
static private let myPrivateVar: Int
```

### 2. 同一行内声明

访问修饰符关键字应该和其他代码放在一行。

```swift
// ✅推荐使用
open class MyClass {
    /* ... */
}

// ❌不推荐
open
class MyClass {
    /* ... */
}
```

### 3. 默认修饰符

默认修饰符为`internal`，无需显示的声明。

### 4. 单元测试

当您需要为单元测试设置一个变量为`open`时，将其标记为内部使用`@testable import Module`。

如果属性应该是私有的，但是为了单元测试的目的而将其声明为内部属性，需要添加适当的文档注释。

```swift
/**
 This property defines the pirate's name.
 - warning: Not `private` for `@testable`.
 */
let myVar = "A Value"
```

### 5. 私有

尽量使用`private` 而不是`fileprivate`。

### 6. 公开

如果在模块外部可以设置子类化的内容，尽量使用`open`而不是`public`。

## 自定义操作符

### 1. 避免自定义运算符

应该避免创建自定义运算符，防止降低代码的可读性。如果需要创建自定义运算符，可以使用命名函数替换。

### 2. 重写现有操作符

 可以覆盖现有的操作符来支持新的类型。如使用`==`比较时。

### 3. 更多参考

更多参考内容[NSHipster article](http://nshipster.com/swift-operators/#guidelines-for-swift-operators).

## Switch 和 Enums

### 1. break

Switch语句中不存在隐式贯穿，因此不需要在 case 分支中显式地使用 `break` 语句。

### 2. default

当使用具有有限可能的`Switch`语句时，不要使用`default case`为未使用的`case`创建一个单独的用例。

### 3. 对齐

`case`和`switch`左侧对齐

### 4. 关联值

当为枚举定义关联值时，一定要显式地描述它。

```swift
// ✅推荐使用
case animated(duration: TimeInterval

// ❌不推荐
case animated(TimeInterval)
```

### 5. list case

优先使用`list case` （如`case 1, 2, 3:`）。

### 6. 抛出错误

当有case未执行到，需要把错误信息抛到上层而不是默认失败。

```swift
func doSomethingWithNumber(_ digit: Int) throws {
    switch digit {
    case 0, 1, 2, 3, 4, 5:
        /* ... */
    default:
        throw Error(message: "Cannot handle this value")
    }
}
```

### 7. 避免写出枚举类型

当编译器能够自动推导出枚举类型时，则不需要写出枚举类型。

```swift
// ✅推荐
.enumValue

// ❌不推荐
MyEnum.enumValue
```

## 可选值

### 1. 避免强制解包

避免使用`!`,`as!`,`try!`强制解包，当强制解包空内容时会引发崩溃。

### 2. 可选类型

当变量，函数返回值可以为`nil`时，需要声明为可选类型（`?`）。

### 3. 显示比较

当需要判断值是否为`nil`又不需要解包出来的值，则可以进行显示比较。

```swift
// ✅推荐使用
if someOptional != nil {
    /* ... */
}

// ❌不推荐
if let _ = someOptional {
    /* ... */
}
```

### 4. 避免使用**`unowned`**

避免使用隐式解包

```swift
// ✅推荐使用
weak var myViewController: UIViewController?

// ❌不推荐使用
weak var myViewController: UIViewController!
unowned var myViewController: UIViewController
```

### 5. 使用相同名称解包

可以使用相同的变量名称解包

```swift
guard let aValue = aValue else {
    /* ... */
}
```

### 6. 多个可选绑定

在同一个`if`语句中可以使用多个可选绑定，避免多重`if`嵌套。

错误使用❌

```swift
if let id = json[Constants.id] as? Int {
    if let firstName = json[Constants.firstName] as? String {
        if let lastName = json[Constants.lastName] as? String {
            if let initials = json[Constants.initials] as? String {
                /* ... */
            }
        }
    }
}
```

正确使用✅

```swift
if
    let id = json[Constants.Id] as? Int,
    let firstName = json[Constants.firstName] as? String,
    let lastName = json[Constants.lastName] as? String,
    let initials = json[Constants.initials] as? String {
        /* ... */
}
```

### 7. 多个可选绑定的格式

当使用`if`或`guard`对多个可选值进行解包时，将每个常量或变量放在一行，后面跟着一个逗号`,`，最后一行除外。

当是`guard`语句时，最后一行是变量后面跟着 `else {`,

当是`if`语句时，最后一行是变量后面跟着`{`。

```swift
if
    let constantOne = valueOne,
    let constantTwo = valueTwo,
    let constantThree = valueThree {
        // Code
}
```

## 协议

### 1. 组织代码

1. 使用`// MARK:`将协议与其他代码分开。
2. 在`class`或`struct`的实现之外创建`extension`，但在同一个文件内。

{% hint style="info" %}
\#2 子类不能覆盖扩展里的方法。
{% endhint %}

### 2. @objc

如果协议具有可选方法，则必须使用`@objc` 关键字声明它。

### 3. 多个类使用同一协议

多个类使用同一协议，请在自己的文件内声明。

### 4. 仅支持class的协议

若仅支持类类型的协议，需要增加`class`关键字。

## 属性

### 1. 只读计算属性

创建只读计算属性，不需要增加`get {}` 。

```swift
// ✅推荐使用
var diameter: Double {
    return radius * 2
}

// ❌不推荐
var diameter: Double {
    get {
        return radius * 2
    }
}
```

### 2. 代码缩进

`get {}`, `set {}`, `willSet` , `didSet` 必须使用代码缩进。

### 3. newValue，oldValue

在`set`、`get`方法中使用标准的`newValue`，`oldValue`。

### 4. 单例的实现

必须使用下面这种方式

```swift
class NetworkManager {
    private init() {}
    static let shared = NetworkManager()
    /* ... */
}
```

### 5. 懒加载

使用`lazy`关键字，实现懒加载，延迟对象的初始化，控制声明周期，在特殊情况下根据需要减少内存分配。

## 三目运算符

为了增加清晰度和代码整洁度，可适当使用三木运算符`? :` 。

三元运算符的最佳用途是在赋值变量和决定使用哪个值。

```swift
// ✅推荐使用
var value = 5
result = value != 0 ? x : y

// ❌不推荐
result = a > b ? x = c > d ? c : d : y
```

## 闭包

### 1. 省略类型

在闭包内声明参数，如果能够自动推导类型，也可以省略类型。

```swift
// 省略类型
doSomethingWithClosure() { response in
    print(response)
}

// 显示声明类型
doSomethingWithClosure() { response: NSURLResponse in
    print(response)
}
```

### 2. 行数

如果不超过一行字符的限制，则应该保持左括号和参数在同一行。

### 3. 简写参数

仅对简单的单行闭包实现使用简写参数语法。

```swift
// [4, 6, 8]
let doubled = [2, 3, 4].map { $0 * 2 } 
```

对于复杂的情况，明确定义参数。

```swift
let names = ["George Washington", "Martha Washington", "Abe Lincoln"]
let emails = names.map { fullname in
    let dottedName = fullname.replacingOccurrences(of: " ", with: ".")
    return dottedName.lowercased() + "@whitehouse.gov"
}
```

### 4. 尾随闭包

仅当参数列表末尾有单个闭包表达式参数时，才使用尾随闭包语法。 给出闭包参数描述性名称。

```swift
// ✅推荐使用
UIView.animate(withDuration: 1.0 {
    /* ... */
}
UIView.animate(withDuration: 1.0, animations: {
    /* ... */
}, completion: { finished in
    /* ... */
}

// ❌不推荐
UIView.animate(withDuration: 1.0, animations: {
    /* ... */
}, { finished in
    /* ... */
}
```

## 代理

### 1. 委托源

创建自定义代理方法时，未命名的第一个参数应该是委托源。

```swift
// ✅推荐使用
func customPickerView(_ pickerView: CustomPickerView, numberOfRows: Int)

// ❌不推荐
func customPickerView(pickerView: CustomPickerView, numberOfRows: Int)
```

## 数组

### 1. 避免下标访问

避免使用下标直接访问数组，但可以使用`.first`或`.last`等访问器，以确保项目不可用时的安全性。 如果没有访问器，请务必先进行正确的绑定检查。

### 2. 迭代

使用`for item in items` 语法，禁止使用`for i in 0 ..< items.count` 。

### 3. 连接数组

使用`.append()`或`.append(contentsOf:)`方法代替+-操作符去连接数组，因为在编译阶段，它们性能更高。

## `guard` 的使用

### 1. 尽早退出

尽早退出是对函数进行完整性检查的首选方法。也是避免嵌套`if`语句的首选方法。使用`guard`可以提高代码的可读性。

```swift
// ✅推荐使用
func doSomethingWithItem(at index: Int) {
    guard index >= 0 && index < item.count else {
        // 因为数组越界提前退出
        return
    }
    let item = item[index]
    doSomethingWithObject(item)
}

// ❌不推荐
func doSomethingWithItem(at index: Int) {
    if index >= 0 && index < item.count {
        let item = item[index]
        doSomethingWithObject(item)
    }
}
```

### 2. 完整性检查

免使用嵌套的`if`语句并减少代码中嵌套缩进的数量：使用`guard`语句而不是`if`语句进行完整性检查。

```swift
// ✅推荐使用
guard let anObject = anObject else {
    return
}
doSomething(with: anObject)
doAnotherStuff(with: anObject)

// ❌不推荐
if let anObject = anObject {
    doSomething(with: anObject)
    doAnotherStuff(with: anObject)
}

// ❌不推荐
if anObject == nil {
    return
}
doSomething(with: anObject!)
doAnotherStuff(with: anObject!)
```

### 3. 避免单行`guard`语句

避免在一行上使用`guard`语句。

```swift
// ✅推荐
guard let aValue = aValue else {
    return
}

// ❌不推荐
guard let aValue = aValue else { return }
```

### 4. 使用范围

只有在失败导致退出当前上下文时才应使用`guard`; 如果上下文需要继续则应该使用一个或多个`if`语句。 `guard`的目标是进行早期检查和返回。

### 5. 状态选择

如果在两个不同的状态之间进行选择，则使用`if`语句而不是使用`guard`语句更有意义。

```swift
// ✅推荐使用
if aCondition {
    /* code A */
} else {
    /* code B */
}

// ❌不推荐
guard aCondition else {
    /* code B */
    return
}
/* code A */
```















