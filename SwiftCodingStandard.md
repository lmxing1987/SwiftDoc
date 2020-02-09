## 目录 {#index}

[TOC]

## 1.1 使用4个空格进行缩进

## 1.2 二元运算符(+, ==, 或->)的前后都需要添加空格

```
推荐
```

```swift
let testValue = 1 + 2
                    
if testValue == 1 {
    /* ... */
}
                    
func testFunction(with testValue: TestClass) -> returnValue {
    /* ... */
}
```

## 1.3 一般情况下，在逗号后面加一个空格

```swift
// 推荐
let testArray = [1, 2, 3, 4, 5]
```

```swift
// 不推荐
let testArray = [1,2,3,4,5]
```

## 1.4 每个文件结尾留一行空行

## 1.5 代码结尾不要使用分号`;`

```swift
// 推荐
print("Hello World")
```

```swift
// 不推荐
print("Hello World");
```

## 1.6 左大括号不要另起一行

```swift
// 推荐
// 1. Class Define
class TestClass {
    /* ... */
}

// 2. if
if testValue == 1 {
    /* ... */
}

// 3. if else
if testValue == 1 {
    /* ... */
} else {
    /* ... */
}

// 4. while
while isTrue {
    /* ... */
}

// 5. guard
guard let testValue = testValue else  {
    /* ... */
}

```

```swift
// 不推荐
// 1. Class Define
class TestClass 
{
    /* ... */
}

// 2. if
if testValue == 1
{
    /* ... */
}

// 3. if else
if testValue == 1
{
    /* ... */
}
else
{
    /* ... */
}

// 4. while
while isTrue 
{
    /* ... */
}

// 5. guard
guard let testValue = testValue else 
{
    /* ... */
}
```

## 1.7 判断语句不用括号

```swift
// 推荐
if testValue == 1 {
    /* ... */
}

if testValue == 1 && testString == "test" {
    /* ... */
}
```

```cpp
// 不推荐
if (testValue == 1) {
   /* ... */
}

if ((testValue == 1) && (testString == "test")) {
   /* ... */
}
```

## 1.8 不建议使用`self.` ，除非方法参数与属性同名

```swift
// 使用self.情况
func setText(text: String) {
    self.text = test
}
```

## 1.9 枚举类型的访问使用更简洁的点语法

```swift
// 推荐
enum CompassPoint {
    case north
    case south
    case east
    case west
}
let directionToHead = .west
```

```swift
// 不推荐
let directionToHead = CompassPoint.west
```

## 1.10 添加有必要的注释，尽可能使用Xcode注释快捷键（⌘⌥/）

```swift
// 推荐
/// <#Description#>
///
/// - Parameter testString: <#testString description#>
/// - Returns: <#return value description#>
func testFunction(testString: String?) -> String? {
    /* ... */
}
```

```swift
// 不推荐
// Comment
func testFunction(testString: String?) -> String? {
    /* ... */
}
```

## 1.11 注释符`//`后加空格，如果`//`跟在代码后面，前面也加一个空格

```csharp
// 推荐
// 注释
let aString = "xxx" // 注释
```

## 1.12 使用`// MARK: -`，按功能和协议/代理分组

```cpp
/// MARK顺序没有强制要求，但System API & Public API一般分别放在第一块和第二块。

// MARK: - Public

// MARK: - Request

// MARK: - Action

// MARK: - Private

// MARK: - xxxDelegate
```

## 1.13 对外接口不兼容时，使用@available(iOS x.0, *)标明接口适配起始系统版本号

```swift
// 推荐
@available(iOS x.0, *)
class myClass {
    
}

@available(iOS x.0, *)
func myFunction() {
    
}
```

------

# 2. 命名规范

## 2.1 建议不要使用前缀

```swift
// 推荐
HomeViewController
Bundle
```

```swift
// 不推荐
NEHomeViewController
NSBundle
```

## 2.2 不要缩写、简写、单个字母来命名

```swift
// 推荐
let viewFrame = view.frame
```

```swift
// 不推荐
let r = view.frame
```

## 2.3 常量命名：以小写字母`k` 开头

## 2.4 变量命名

##### 2.4.1 使用小驼峰，首字母小写

##### 2.4.2 变量命名应该能推断出该变量类型，如果不能推断，则需要以变量类型结尾

```swift
// 推荐
class TestClass: class {
    // UIKit的子类，后缀最好加上类型信息
    let coverImageView: UIImageView
    @IBOutlet weak var usernameTextField: UITextField!

    // 作为属性名的firstName，明显是字符串类型，所以不用在命名里不用包含String
    let firstName: String

    // UIViewContrller以ViewController结尾
    let fromViewController: UIViewController
}
```

```tsx
// 不推荐
class TestClass: class {
    // image不是UIImageView类型
    let coverImage: UIImageView
    // or cover不能表明其是UIImageView类型
    var cover: UIImageView

    // String后缀多余
    let firstNameString: String

    // UIViewContrller不要缩写
    let fromVC: UIViewController
}
```

## 2.5 类型命名：使用大驼峰表示法，首字母大写

## 2.6 方法命名：使用参数标签让方法语义更清楚, 参数标签和参数需要表达正确的语义(Public接口及基础组件必须遵循)

参数标签规则(from Swift API Design Guidelines)

##### 2.6.1 省略所有的冗余的参数标签

```swift
// 推荐
func min(_ number1: int, _ number2: int) {
    /* ... */
}

min(1, 2)
```

##### 2.6.2 进行安全值类型转换的构造方法可以省略参数标签，非安全类型转换则需要添加参数标签以表示类型转换方法

```swift
// 推荐
extension UInt32 {
  /// 安全值类型转换，16位转32位，可省略参数标签
  init(_ value: Int16)
  
  /// 非安全类型转换，64位转32位，不可省略参数标签
  /// 截断显示
  init(truncating source: UInt64)
  
  /// 非安全类型转换，64位转32位，不可省略参数标签
  /// 显示最接近的近似值
  init(saturating valueToApproximate: UInt64)
}
```

##### 2.6.3 当第一个参数构成整个语句的介词时（如，at, by, for, in, to, with 等），为第一个参数添加介词参数标签

```go
// 推荐
// 添加介词标签havingLength
func removeBoxes(havingLength length: int) {
    /* ... */
}

x.removeBoxes(havingLength: 12)
```

例外情况是，当后面所有参数构成独立短语时，则介词提前。

```swift
// 推荐
// 介词To提前
a.moveTo(x: b, y: c)

// 介词From提前
a.fadeFrom(red: b, green: c, blue: d)
```



```swift
// 不推荐
a.move(toX: b, y: c)

a.fade(fromRed: b, green: c, blue: d)
```

##### 2.6.4 当第一个参数构成整个语句一部分时，省略第一个参数标签，否则需要添加第一个参数标签

```cpp
// 推荐
// 参数构成语句一部分，省略第一个参数标签
x.addSubview(y)

// 参数不构成语句一部分，不省略第一个参数标签
view.dismiss(animated: false)
```

##### 2.6.5 其余情况下，给除第一个参数外的参数都添加标签

## 2.7 方法命名：不要使用冗余的单词，特别是与参数及参数标签重复

```swift
// 推荐
func remove(_ member: Element) -> Element?
```



```swift
// 不推荐
func removeElement(_ member: Element) -> Element?
```

## 2.8 命名出现缩写词，缩写词要么全部大写，要么全部小写，以首字母大小写为准

```swift
// 推荐
let urlRouterString = "https://xxxxx"

let htmlString = "xxxx"

class HTMLModel {
    /* ... */
}

struct URLRouter {
    /* ... */
}

```



```swift
// 不推荐
let uRLRouterString = "https://xxxxx"

let hTMLString = "xxxx"

class HtmlModel {
    /* ... */
}

struct UrlRouter {
    /* ... */
}
```

## 2.9 Bool类型命名：用is最为前缀

```csharp
// 推荐
var isString: Bool = true
```

## 2.10 枚举定义尽量简写，不要包括类型前缀

```swift
// 推荐
public enum UITableViewRowAnimation : Int {
    case fade

    case right // slide in from right (or out to right)

    case left

    case top

    case bottom

    case none // available in iOS 3.0

    case middle // available in iOS 3.2.  attempts to keep cell centered in the space it will/did occupy

    case automatic // available in iOS 5.0.  chooses an appropriate animation style for you
}
```

## 2.11 协议命名

协议规则(from Swift API Design Guidelines)

##### 2.11.1 如果协议描述的是协议做的事应该命名为名词(eg. Collection)

```swift
// 推荐
protocol TableViewSectionProvider {
    func rowHeight(at row: Int) -> CGFloat
    var numberOfRows: Int { get }
    /* ... */
}
```

##### 2.11.2 如果协议描述的是能力，需添加后缀`able`或 ing (eg. Equatable、 ProgressReporting)

```swift
// 推荐
protocol Loggable {
    func logCurrentState()
    /* ... */
}

protocol Equatable {
    
    func ==(lhs: Self, rhs: Self) -> bool {
        /* ... */
    }
}
```

##### 2.11.3 如果已经定义类，需要给类定义相关协议，则添加`Protocol`后缀

```swift
// 推荐
protocol InputTextViewProtocol {
    func sendTrackingEvent()
    func inputText() -> String
    /* ... */
}
```

------

# 3. 语法规范

## 3.1 多使用let，少使用var

## 3.2 少用`!`去强制解包

## 3.3 可选类型拆包取值时，使用`if let`判断

```csharp
// 推荐
if let optionalValue = optionalValue {
    /* ... */
}

```

```csharp
// 不推荐
if  optionalValue != nil {
    let value = optionalValue!
    /* ... */
}
```

## 3.4 多个可选类型拆包取值时，将多个`if let`合并

```swift
// 推荐
var subview: UIView?
var volume: Double?

if let subview = subview, let volume = volume {
    /* ... */
}

```

```swift
// 不推荐
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    /* ... */
  }
}
```

## 3.5 不要使用 as! 或 try!

```csharp
// 推荐
// 使用if let as？判断
if let text = text as? String {
    /* ... */
}

// 使用if let try 或者 try?
if let test = try aTryFuncton() {
    /* ... */
}
```

## 3.6 数组和字典变量定义，定义时需要标明泛型类型，并使用更简洁的语法

```swift
// 推荐
var names: [String] = []
var lookup: [String: Int] = [:]

```



```jsx
// 不推荐
var names = [String]()
var names: Array<String> = [String]() / 不够简洁

var lookup = [String: Int]()
var lookup: Dictionary<String, Int> = [String: Int]() // 不够简洁
```

## 3.7 数组访问尽可能使用 .first 或 .last, 推荐使用 for item in items 而不是 for i in 0..

## 3.8 如果变量能够推断出类型，则不建议声明变量时指明类型

```swift
// 推荐
let value = 1 

let text = "xxxx"

```

```swift
// 不推荐
let value: int = 1 

let text: String = "xxxx"
```

## 3.9 判等

##### 3.9.1 使用`==`和`!=`判断`内容`上是否一致

```swift
// 推荐
// String类型没有-isEqualToString方法，用==判断是否相等
let str1 = "netease"
let str2 = "netease"
if str1 == str2 {
    // is true
    /* ... */
}

// 对于自定义类型，判断内容是否一致，需要实现Equatable接口
class BookItem {
    let bookId: String
    let title: String
    
    init (bookdId: String, title: String) {
        self.bookId = bookId
        self.title = title
    }
}

extension BookItem: Equatable {
}

func ==(lhs: bookItem, rhs: book: BookItem) -> bool {
    return lhs.bookId == rhs.bookId
}
```

##### 3.9.2 使用`===`和`!==`判断class类型对象是否同一个引用，而不是用 `==`和`!=`

```swift
// 推荐
if tenEighty === alsoTenEighty {
    /* ... */
}

if tenEighty !== notTenEighty {
    /* ... */
}

```



```swift
// 不推荐
/// 错误写法！
if tenEighty == alsoTenEighty {
    /* ... */
}

/// 错误写法！
if tenEighty != notTenEighty {
    /* ... */
}
```

## 3.10 常量定义，建议尽可能定义在Type类型里面，避免污染全局命名空间

```csharp
// 推荐
class TestTabelViewCell: UITableViewCell {
    static let kCellHeight = 80.0
    
    /* ... */
}

// uses
let cellHeight = TestTabelViewCell.kCellHeight

```



```swift
// 不推荐
let kCellHeight = 80.0

class TestTabelViewCell: UITableViewCell {
    /* ... */
}

// uses
let cellHeight = kCellHeight
```

## 3.11 协议

##### 3.11.1 当实现protocol时，如果确定protocol的实现不会被重写，建议用extension将protocol实现分离

```swift
// 推荐
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource

extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate

extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}

```

```swift
// 不推荐
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

##### 3.11.2 当实现protocol时，如果确定protocol的实现会被重写，则可将protocol的实现放在一起

##### 3.11.3 当实现protocol时，建议用extension将protocol实现分离

## 3.12 当方法最后一个参数是Closure类型，调用时建议使用尾随闭包语法

```swift
// 推荐
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

```



```swift
// 不推荐
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})
```

## 3.13 switch case选项不需要使用`break`关键词

```swift
// 推荐
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}
// Prints "The last letter of the alphabet"
```

## 3.14 访问控制

##### 3.14.1 对于私有访问，如果在文件内不能被修改，则标记为`private`；如果在文件内可修改，则标记为`fileprivate`

```swift
// 推荐
class User {
    fileprivate var name = "private"
}

extension User {
    var accessPrivate: String {
        // 同一文件内，可访问fileprivate类型属性，但不可访问private类型属性
        return name
    }
}
```

#### 3.14.2 对于公有访问，如果不希望在外面继承或者override，则标记为`public`，否则标明为`open`

```kotlin
// 推荐
// User不可被外部继承
public class User {
    private var name = "private"
}

// User可被外部继承
open class User {
    private var name = "private"
}
```

##### 3.14.3 访问控制权限关键字应该写在最前面，除了`@IBOutlet`、`IBAction`、`@discardableResult`、`static` 等关键字在最前面

## 3.15 如调用者可以不使用方法的返回值，则需要使用`@discardableResult`标明

```swift
// 推荐
@discardableResult
func printMessage(message: String) -> String {
    let outputMessage = "Output : \(message)"
    print(outputMessage)

    return outputMessage
}
```

## 3.16 Golden Path，最短路径原则

```swift
// 推荐
func login(with username: String?, password: String?) throws -> LoginError {
  guard let username = contextusername else { 
    throw .noUsername 
  }
  guard let password = password else { 
    throw .noPassword
  }

  /* login code */
}

```



```swift
// 不推荐
func login(with username: String?, password: String?) throws -> LoginError {
  if let username = username {
    if let password = inputDatapassword {
        /* login code */
    } else {
        throw .noPassword
    }
  } else {
      throw .noUsername
  }
}
```

## 3.17 循环引用

##### 3.17.1 使用委托和协议时，避免循环引用，定义属性的时候使用`weak`修饰

```swift
// 推荐
public weak var dataSource: UITableViewDataSource?

public weak var delegate: UITableViewDelegate?
```

##### 3.17.2 在Closures中使用self时避免循环引用

```swift
// 推荐
resource.request().onComplete { [weak self] response in
    guard let strongSelf = self else { 
        return 
    }
    let model = strongSelf.updateModel(response)
    strongSelf.updateUI(model)
}

```

```swift
// 不推荐
// 不推荐使用unowned
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}

```



```swift
// 不推荐
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

## 3.18 单例

```swift
// 推荐
class TestManager {
    static let shared = TestManager()

    /* ... */
}
```

------

# 4. Objective-C兼容

## 4.1 接口兼容

##### 4.1.1 Swift接口不对Objective-C兼容，在编译器或者Coding中就会出现错误

##### 4.1.2 暴漏给Objective-C的任何接口，需要添加@objc关键字，如果定义的类继承自NSObject则不需要添加

##### 4.1.3 如果方法参数或者返回值为空，则需要标明为可选类型

```swift
// 推荐
@objc SwiftClass {
    /* ... */
}

@objc protocol SwiftProtocol {
    /* ... */
}
```

#### 

## 4.2 Runtime兼容

Swift语言本身对Runtime并不支持，需要在属性或者方法前添加dynamic修饰符才能获取动态型，继承自NSObject的类其继承的父类的方法也具有动态型，子类的属性和方法也需要加dynamic才能获取动态性

------

# 参考

- [Swift Programming Language](https://link.jianshu.com?t=https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309)
- [Swift API Design Guidelines](https://link.jianshu.com?t=https://swift.org/documentation/api-design-guidelines/)
- [Raywenderlich Swift Style Guide](https://link.jianshu.com?t=https://github.com/raywenderlich/swift-style-guide#spacing)
- [Linkedin Swift Style Guide](https://link.jianshu.com?t=https://github.com/linkedin/swift-style-guide#1-code-formatting)

