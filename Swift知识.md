## class 和 struct 的区别

**1.struct是值类型，class是引用类型。**

值类型的变量直接包含它们的数据，对于值类型都有它们自己的数据副本，因此对一个变量操作不可能影响另一个变量。

引用类型的变量存储对他们的数据引用，因此后者称为对象，因此对一个变量操作可能影响另一个变量所引用的对象。

**2.二者的本质区别：**

struct是深拷贝，拷贝的是内容；class是浅拷贝，拷贝的是指针。

**3.property的初始化不同：**

class 在初始化时不能直接把 property 放在 默认的constructor 的参数里，而是需要自己创建一个带参数的constructor；而struct可以，把属性放在默认的constructor 的参数里。

**4.变量赋值方式不同：**

struct是值拷贝；class是引用拷贝。

**5.immutable变量：**

swift的可变内容和不可变内容用var和let来甄别，如果初始为let的变量再去修改会发生编译错误。struct遵循这一特性；class不存在这样的问题。

**6.mutating function：** 

struct 和 class 的差別是 struct 的 function 要去改变 property 的值的时候要加上 mutating，而 class 不用。

**7.继承：** 

struct不可以继承，class可以继承。

**8.struct比class更轻量：**

struct分配在栈中，class分配在堆中。

## open与public的区别

public:可以别任何人访问，但是不可以被其他module复写和继承。

open:可以被任何人访问，可以被继承和复写。

## swift中struct作为数据模型

优点:

1.安全性:因为 Struct 是用值类型传递的，它们没有引用计数。

2.内存:由于他们没有引用数，他们不会因为循环引用导致内存泄漏。

  速度:值类型通常来说是以栈的形式分配的，而不是用堆。因此他们比 Class 要快很多!

3.拷贝:Objective-C 里拷贝一个对象,你必须选用正确的拷贝类型（深拷贝、浅拷贝）,而值类型的拷贝则非常轻松！

4.线程安全:值类型是自动线程安全的。无论你从哪个线程去访问你的 Struct ，都非常简单。

缺点:

1.Objective-C与swift混合开发：OC调用的swift代码必须继承于NSObject。

2.继承：struct不能相互继承。

3.NSUserDefaults：Struct 不能被序列化成 NSData 对象

## Swift代码复用的方式有哪些?

1.继承

2.在swift 文件里直接写方法，相当于一个全局函数

3.extension 给类直接扩展方法

## Array、Set、Dictionary 三者的区别

Set:是用来存储相同类型、没有确定顺序、且不重复的值的集

Array:是有序数据的集

Dictionary:是无序的键值对的集

## map、filter、reduce 的作用

map : 映射 ，将一个元素根据某个函数 映射 成另一个元素（可以是同类型，也可以是不同类型）

filter : 过滤 ， 将一个元素传入闭包中，如果返回的是false ， 就过滤掉

reduce ：先映射后融合 ， 将数组中的所有元素映射融合在一起。

## map 与 flatmap 的区别

map:作用于数组中的每个元素，闭包返回一个变换后的元素，接着将所有这些变换后的元素组成一个新的数组。

flatMap:功能和map类似，区别在于flatMap可以去nil,能够将可选类型(optional)转换为非可选类型(non-optionals),把数组中存有数组的数组 一同打开变成一个新的数组(数组降维)

## copy on write

写时复制：每当数组被改变，它首先检查它对存储缓冲区 的引用是否是唯一的，或者说，检查数组本身是不是这块缓冲区的唯一拥有者。如果是，那么 缓冲区可以进行原地变更;也不会有复制被进行。不过，如果缓冲区有一个以上的持有者 (如本 例中)，那么数组就需要先进行复制，然后对复制的值进行变化，而保持其他的持有者不受影响。

在Swift提供一个函数isKnownUniquelyReferenced，能检查一个类的实例是不是唯一的引用，如果是，说明实例没有被共享

eg: isKnownUniquelyReferenced(&object)

## 获取当前代码的函数名和行号

函数名:#function 

行号:#line 

文件名:#file

## guard、defer、where

defer:defer所声明的 block 会在当前代码执行退出后被调用，如果有多个 defer, 那么后加入的先执行

guard:可以理解为拦截，凡是不满足 guard 后面条件的，都不会再执行下面的代码。

where:在Swift语法里where关键字的作用跟SQL的where一样， 即附加条件判断。where关键字可以用在集合遍历、switch/case、协议中； Swift3时if let和guard场景的where已经被Swift4的逗号取代

## String 与 NSString 的关系与区别

String为Swift的Struct结构，值类型；NSString为OC对象，引用类型，能够互相转换

## throws 和 rethrows 的用法与作用

throws 用在函数上, 表示这个函数会抛出错误.

有两种情况会抛出错误, 一种是直接使用 throw 抛出, 另一种是调用其他抛出异常的函数时, 直接使用 try xx 没有处理异常.

```swift
enum DivideError: Error {
    case EqualZeroError;
}
func divide(_ a: Double, _ b: Double) throws -> Double {
    guard b != Double(0) else {
        throw DivideError.EqualZeroError
    }
    return a / b
}
func split(pieces: Int) throws -> Double {
    return try divide(1, Double(pieces))
}
```

rethrows 与 throws 类似, 不过只适用于参数中有函数, 且函数会抛出异常的情况, rethrows 可以用 throws 替换, 反过来不行

```swift
func processNumber(a: Double, b: Double, function: (Double, Double) throws -> Double) rethrows -> Double {
    return try function(a, b)
}
```

## try?和 try!

这两个都用于处理可抛出异常的函数, 使用这两个关键字可以不用写 do catch.

区别在于, try? 在用于处理可抛出异常函数时, 如果函数抛出异常, 则返回 nil, 否则返回函数返回值的可选值

而 try! 则在函数抛出异常的时候崩溃, 否则则返会函数返回值, 相当于(try? xxx)!

## associatedtype 的作用

简单来说就是 protocol 使用的泛型

## Swift 下的 KVO , KVC

KVO, KVC 都是Objective-C 运行时的特性, Swift 是不具有的, 想要使用, 必须要继承 NSObject, 自然, 继承都没有的结构体也是没有 KVO, KVC 的.

KVC：Swift 下的 KVC 用起来很简单, 只要继承 NSObject 就行了

KVO：KVO 就稍微麻烦一些了,由于 Swift 为了效率, 默认禁用了动态派发, 因此想用 Swift 来实现 KVO, 除了继承NSObject，还需要将想要观测的对象标记为 dynamic

## @objc

OC 是基于运行时，遵循了 KVC 和动态派发，而 Swift 是静态语言，为了追求性能，在编译时就已经确定，而不需要在运行时的，在 Swift 类型文件中，为了解决这个问题，需要暴露给 OC 使用的任何地方（类，属性，方法等）的生命前面加上 @objc 修饰符

如果用 Swift 写的 class 是继承 NSObject 的话， Swift 会默认自动为所有非 private 的类和成员加上@objc

## 自定义下标获取

实现 subscript 即可, 索引除了数字之外, 其他类型也是可以的

```swift
extension AnyList {
    subscript(index: Int) -> T{
        return self.list[index]
    }

    subscript(indexString: String) -> T?{
        guard let index = Int(indexString) else {
            return nil
        }
        return self.list[index]
    }
}
```

## lazy 的作用

懒加载, 当属性要使用的时候, 才去完成初始化

## 一个类型表示选项，可以同时表示有几个选项选中（类似 UIViewAnimationOptions ），用什么类型表示

需要实现OptionSet, 一般使用 struct 实现. 由于 OptionSet 要求有一个不可失败的init(rawValue:) 构造器, 而 枚举无法做到这一点(枚举的原始值构造器是可失败的, 而且有些组合值, 是没办法用一个枚举值表示的)

```swift
struct` `SomeOption: OptionSet {``  ``let rawValue: Int``  ``static` `let option1 = SomeOption(rawValue: 1 << 0)``  ``static` `let option2 = SomeOption(rawValue:1 << 1)``  ``static` `let option3 = SomeOption(rawValue:1 << 2)``}``let options: SomeOption = [.option1, .option2]
```

## static和class的区别

static 可以在类、结构体、或者枚举中使用。而 class 只能在类中使用。

static 可以修饰存储属性，static 修饰的存储属性称为静态变量(常量)。而 class 不能修饰存储属性。

static 修饰的计算属性不能被重写。而 class 修饰的可以被重写。

static 修饰的静态方法不能被重写。而 class 修饰的类方法可以被重写。

class 修饰的计算属性被重写时，可以使用 static 让其变为静态属性。

class 修饰的类方法被重写时，可以使用 static 让方法变为静态方法

class关键字指定的类方法可以被子类重写，但是用static关键字指定的类方法是不能被子类重写的

## mutating

mutating用于函数的最前面,用于告诉编译器这个方法会改变自身.

swift增强了结构体和枚举的使用,结构体和枚举也可以有构造方法和实例方法,但结构体和枚举是值类型,如果我们要在实例方法中修改当前类型的实例属性的值或当前对象实例的值,必须在func前面添加mutating关键字,表示当前方法将会将会修改它相关联的对象实例的实例属性值.

协议中,当在结构体或者枚举实现协议方法时,若对自身属性作修改,需要将协议的方法声明为mutating,对类无影响.

在扩展中,同样若对自身属性作修改,需要将方法声明为mutating

## swift多线程

### 1.Thread

```swift
//方式1：使用类方法，不需要手动启动线程
Thread.detachNewThreadSelector(#selector(ViewController.downloadImage), toTarget: self, with: nil)

//方式2：实例方法-便利构造器，需调用start()启动线程
let myThread = Thread(target: self, selector: #selector(thread), object: nil)
myThread.start()
```

线程同步：线程同步方法通过锁来实现，每个线程都只用一个锁，这个锁与一个特定的线程关联。

```swift
//定义两个线程
var thread1:Thread?
var thread2:Thread?
//定义两个线程条件，用于锁住线程
let condition1 = NSCondition()
let condition2 = NSCondition()
     
override func viewDidLoad() {
   super.viewDidLoad()
   thread2 = Thread(target: self, selector: #selector(ViewController.method2), object: nil)
   thread1 = Thread(target: self, selector: #selector(ViewController.method1), object: nil)
   thread1?.start()
}
     
//定义两方法，用于两个线程调用
func method1(sender:AnyObject){
   for i in 0 ..< 10 {
      print("Thread 1 running \(i)")
      sleep(1)
      if i == 2 {
         thread2?.start() //启动线程2
         //本线程（thread1）锁定
         condition1.lock()
         condition1.wait()
         condition1.unlock()
      }
 　}
   print("Thread 1 over")
   //线程2激活
   condition2.signal()
}
     
//方法2
func method2(sender:AnyObject){
    for i in 0 ..< 10 {
        print("Thread 2 running \(i)")
        sleep(1)
        if i == 2 {
            //线程1激活
            condition1.signal()
            //本线程（thread2）锁定
            condition2.lock()
            condition2.wait()
            condition2.unlock()
         }
    }
    print("Thread 2 over")
}
```

### 2.Operation和OperationQueue

2.1 直接用定义好的子类：BlockOperation。

```swift
let operation = BlockOperation(block: { [weak self] in
    // 执行代码return
})
//创建一个NSOperationQueue实例并添加operation
let queue = OperationQueue()
queue.addOperation(operation)
```

2.2 继承Operation 

　　把Operation子类的对象放入OperationQueue队列中，一旦这个对象被加入到队列，队列就开始处理这个对象，直到这个对象的所有操作完成，然后它被队列释放。

```swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        //创建线程对象
        let downloadImageOperation = DownloadImageOperation()
        //创建一个OperationQueue实例并添加operation
        let queue = OperationQueue()
        queue.addOperation(downloadImageOperation)
    }
}
class DownloadImageOperation: Operation {
    override func main(){
        let imageUrl = "http://hangge.com/blog/images/logo.png"
        let data = try! Data(contentsOf: URL(string: imageUrl)!)
        print(data.count)
    }
}
```

2.3 其他方法

```swift
//队列设置并发数
queue.maxConcurrentOperationCount = 5
//队列取消所有线程操作
queue.cancelAllOperations()
//给operation设置回调
operation.completionBlock = { () -> Void in
    print("--- operation.completionBlock ---")
}
```

### 3.GCD

3.1 创建队列

```swift
//------自己创建队列
//创建串行队列
let serial = DispatchQueue(label: "serialQueue1")
//创建并行队列
let concurrent = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)

//------系统global队列
//Global Dispatch Queue有4个执行优先级：
//.userInitiated  高
//.default  正常
//.utility  低
//.background 非常低的优先级（这个优先级只用于不太关心完成时间的真正的后台任务）
let globalQueue = DispatchQueue.global(qos: .default)

//------系统主线程队列
//因为主线程只有一个，所有这自然是串行队列。一起跟UI有关的操作必须放在主线程中执行。
let mainQueue = DispatchQueue.main
```

3.2 添加任务到队列

async异步追加Block块（async函数不做任何等待）

```swift
DispatchQueue.global(qos: .default).async {
    //处理耗时操作的代码块...
     
    //操作完成，调用主线程来刷新界面
    DispatchQueue.main.async {
        print("main refresh")
    }
}
```

sync同步追加Block块 

```swift
//同步追加Block块，与上面相反。在追加Block结束之前，sync函数会一直等待，等待队列前面的所有任务完成后才能执行追加的任务。
//添加同步代码块到global队列
//不会造成死锁，但会一直等待代码块执行完毕
DispatchQueue.global(qos: .default).sync {
    print("sync1")
}
print("end1")
 
 
//添加同步代码块到main队列
//会引起死锁
//因为在主线程里面添加一个任务，因为是同步，所以要等添加的任务执行完毕后才能继续走下去。但是新添加的任务排在
//队列的末尾，要执行完成必须等前面的任务执行完成，由此又回到了第一步，程序卡死
DispatchQueue.main.sync {
    print("sync2")
}
```

3.3 暂停或者继续队列

　　这两个函数是异步的，而且只在不同的blocks之间生效，对已经正在执行的任务没有影响。suspend()后，追加到Dispatch Queue中尚未执行的任务在此之后停止执行。而resume()则使得这些任务能够继续执行。

```swift
//创建并行队列
let conQueue = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
//暂停一个队列
conQueue.suspend()
//继续队列
conQueue.resume()
```

3.4 asyncAfter 延迟调用

　　asyncAfter 并不是在指定时间后执行任务处理，而是在指定时间后把任务追加到queue里面。因此会有少许延迟。注意，我们不能（直接）取消我们已经提交到 asyncAfter 里的代码。

```swift
//延时2秒执行
DispatchQueue.global(qos: .default).asyncAfter(deadline: DispatchTime.now() + 2.0) {
    print("after!")
}
```

　　如果需要取消正在等待执行的Block操作，我们可以先将这个Block封装到DispatchWorkItem对象中，然后对其发送cancle，来取消一个正在等待执行的block。

```swift
//将要执行的操作封装到DispatchWorkItem中
let task = DispatchWorkItem { print("after!") }
//延时2秒执行
DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 2, execute: task)
//取消任务
task.cancel()
```

 3.5 DispatchWorkItem

　　DispatchWorkItem可以将任务封装成DispatchWorkItem对象。

　　可以调用workItem的`perform()`函数执行任务，也可以将workItem追加到DispatchQueue或DispatchGroup中。以上所有传block的地方都可换成DispatchWorkItem对象。
DispatchQueue还可以使用`notify`函数观察workItem中的任务执行结束，以及通过`cancel()`函数取消任务。

　　另外，workItem也可以像DispatchGroup一样调用`wait()`函数等待任务完成。需要注意的是，追加workItem的队列或调用`perform()`所在的队列不能与调用`workItem.wait()`的队列是同一个队列，否则会出现线程死锁。

```swift
// 初始化方法
init(qos: DispatchQoS, flags: DispatchWorkItemFlags, block: () -> Void)

let workItem = DispatchWorkItem.init {
      print("执行任务")
}
```

3.6 DispatchQueue.concurrentPerform

　　sync函数和Dispatch Group的关联API。会按指定次数异步执行任务，并且会等待指定次数的任务全部执行完成，即会阻塞线程。建议在子线程中使用。

```swift
DispatchQueue.global().async {
     DispatchQueue.concurrentPerform(iterations: 5) { (i) in
         print("执行任务\(i+1)")
     }
     print("任务执行完成")
}
```

3.7 DispatchSemaphore

　　信号量:用于控制访问资源的数量。比如系统有两个资源可以被利用，同时有三个线程要访问，只能允许两个线程访问，第三个会等待资源被释放后再访问。

　　信号量的初始化方法：`DispatchSemaphore.init(value: Int)`，value表示允许访问资源的线程数量，当value为0时对访问资源的线程没有限制。

　　信号量配套使用`wait()`函数与`signal()`函数控制访问资源。

**`　　wait`函数会阻塞当前线程直到信号量计数大于或等于1，当信号量大于或等于1时，将信号量计数-1, 然后执行后面的代码。`signal()`函数会将信号量计数+1。**

　　信号量是GCD同步的一种方式。前面介绍过的`DispatchWorkItemFlags.barrier`是对queue中的任务进行批量同步处理，sync函数是对queue中的任务单个同步处理，而　　　　

　　DispatchSemaphore是对queue中的某个任务中的某部分（某段代码）同步处理。此时将`DispatchSemaphore.init(value: Int)`中的参数value传入1。

```swift
var arr = [Int]()
let semaphore = DispatchSemaphore.init(value: 1) // 创建信号量，控制同时访问资源的线程数为1
for i in 0...100 {
    DispatchQueue.global().async {         
        /*
        其他并发操作
        */    
        semaphore.wait() // 如果信号量计数>=1,将信号量计数减1；如果信号量计数<1，阻塞线程直到信号量计数>=1
        arr.append(i)
        semaphore.signal() // 信号量计加1  
        /*
        其他并发操作
        */
     }
}
```

##### 问题一:

下面代码中变量 tutorial1.difficulty 和 tutorial2.difficulty 的值分别是什么? 如果 Tutorial 是一个类，会有什么不同吗？为什么?

```swift
struct Tutorial {
  var difficulty: Int = 1
}

var tutorial1 = Tutorial()
var tutorial2 = tutorial1
tutorial2.difficulty = 2
```

##### 回答:

tutorial1.difficulty 等于 1, tutorial2.difficulty 等于 2.
 swift中的结构体是值类型。是按值类型而不是引用类型复制值的。

创建了一个tutorial1的副本，并将其分配给tutorial2：

```csharp
var tutorial2 = tutorial1
```

“ tutorial1 ”中未反映对“ tutorial12”的更改

如果 Tutorial 是一个类，那么 tutorial1 和 tutorial2 都等于 2.
 swift中的类是引用类型。当你更改Tutorial1的属性时，你将看到它反映在Tutorial2中，反之亦然。

##### 问题二:

你用var声明了view1，用let声明了view2。有什么区别，最后一行会编译通过吗？

```swift
import UIKit

var view1 = UIView()
view1.alpha = 0.5

let view2 = UIView()
view2.alpha = 0.5 // 此行是否编译？
```

##### 回答:

是的，最后一行可以编译。view1 是一个变量，可以给它重新分配一个 UIView 类型的新实例。使用let，只能分配一次值，因此不会编译以下代码：

```cpp
view2 = view1 // Error: view2 is immutable
```

UIView是一个具有引用语义的类，因此你可以改变view2的属性-这意味着最后一行将编译：

```csharp
let view2 = UIView()
view2.alpha = 0.5 // Yes!
```

##### 问题三:

这段复杂的代码按字母顺序对名称数组进行排序。尽可能简化闭包。

```dart
var animals = ["fish", "cat", "chicken", "dog"]
animals.sort { (one: String, two: String) -> Bool in
    return one < two
}
print(animals)
```

##### 回答:

类型推断系统会自动判断闭包中参数的类型和返回类型，就可以去掉类型：



```kotlin
animals.sort { (one, two) -> Bool in
     return one < two
}
```

可以用$I符号替换参数名：

```bash
animals.sort { return $0 < $1 }
```

在单语句闭包中，可以省略返回关键字。最后一条语句的值将成为闭包的返回值：

```bash
animals.sort { $0 < $1 }
```

后，由于Swift知道数组的元素符合equatable，因此可以简单地编写：

```csharp
animals.sort(by: <)
```

##### 问题四:

此代码创建两个类：Address 和 Person 。然后它创建两个 Person 实例来表示Ray和Brian。

```dart
class Address {
  var fullAddress: String
  var city: String
  
  init(fullAddress: String, city: String) {
    self.fullAddress = fullAddress
    self.city = city
  }
}

class Person {
  var name: String
  var address: Address

  init(name: String, address: Address) {
    self.name = name
    self.address = address
  }
}

var headquarters = Address(fullAddress: "123 Tutorial Street", city: "Appletown")
var ray = Person(name: "Ray", address: headquarters)
var brian = Person(name: "Brian", address: headquarters)
```

假设布 Brian 搬到街对面的新大楼，更新他的地址：

```bash
brian.address.fullAddress = "148 Tutorial Street"
```

它编译,运行时不会出错。如果你现在查一下 Ray 的地址，他也搬到了新大楼。

```css
print (ray.address.fullAddress)
```

这是怎么回事？你如何解决这个问题？

##### 回答:

Address 是一个类，是引用类型，不管是通过Ray还是Brian访问，都是相同的实例。更改 headquarters 地址将同时更改两个人的地址。你能想象如果 Brian 收到Ray的邮件会发生什么情况吗，反之亦然？

解决方案是创建一个新 Address 对象来分配给Brian，或者将 Address 声明为结构体。

## 口述问题

##### 什么是可选的，可选可以解决哪些问题？

使用可选类型（optionals）来处理值可能缺失的情况。在objective-c中，只有在使用nil特殊值的引用类型中才可以表示值缺失。值类型（如int或float）不具有此功能。
 Swift将缺乏值概念扩展到引用类型和值类型。可选变量可以包含值或零，表示是否缺少值。

##### 总结结构体和类之间的主要区别。

差异总结为：

- 类支持继承；结构不支持。
- 类是引用类型；结构体是值类型。

##### 什么是通用类型，它们解决了什么问题？

在swift中，可以在函数和数据类型中使用泛型，例如在类、结构体或枚举中。
 泛型解决了代码重复的问题。当有一个方法接受一种类型的参数时，通常会复制它以适应不同类型的参数。
 例如，在下面的代码中，第二个函数是第一个函数的“克隆”，但它接受字符串而不是整数。

```swift
func areIntEqual(_ x: Int, _ y: Int) -> Bool {
  return x == y
}

func areStringsEqual(_ x: String, _ y: String) -> Bool {
  return x == y
}

areStringsEqual("ray", "ray") // true
areIntEqual(1, 1) // true
```

通过采用泛型，可以将这两个函数合并为一个函数，同时保持类型安全。下面是通用实现：

```swift
func areTheyEqual<T: Equatable>(_ x: T, _ y: T) -> Bool {
  return x == y
}

areTheyEqual("ray", "ray")
areTheyEqual(1, 1)
```

由于在本例中测试的是相等性，所以将参数限制为遵守 Equatable 协议的任何类型。此代码实现了预期的结果，并防止传递不同类型的参数。

##### 在某些情况下，你无法避免使用隐式展开的选项。什么时候？为什么？

使用隐式展开选项的最常见原因是：

- 如果在实例化时无法初始化非本质的属性。
   一个典型的例子是Interface Builder出口，它总是在它的所有者之后初始化。在这种特定情况下 - 假设它在Interface Builder中正确配置 - 你在使用它之前保证outlet是非零的。
- 解决强引用循环问题，即两个实例相互引用并且需要对另一个实例的非空引用。在这种情况下，将一侧标记为无主引用，而另一侧使用隐式解包可选。

##### 打开可选项的各种方法有哪些？他们如何评价安全性？

```dart
var x : String? = "Test"
```

提示：有七种方法。

- 强行打开 - 不安全。

```jsx
let a: String = x!
```

- 隐式解包变量声明 - 在许多情况下不安全。

```csharp
var a = x!
```

- 可选绑定 - 安全。

```bash
if let a = x {
  print("x was successfully unwrapped and is = \(a)")
}
```

- 可选链接 - 安全。

```swift
let a = x?.count
```

- 无合并操作员 - 安全。

```bash
let a = x ?? ""
```

- 警卫声明 - 安全。

```swift
guard let a = x else {
  return
}
```

- 可选模式 - 安全。

```bash
if case let a? = x {
  print(a)
}
```

### 中级面试题

##### 问题一:

nil 和 .none有什么区别?

没有区别，因为Optional.none（简称.none）和nil是等价的。
 实际上，下面的等式输出为真：

```swift
nil == .none
```

使用nil更常见，推荐使用。

这是 thermometer 作为类和结构的模型。编译器在最后一行报错。为什么编译失败？

```swift
public class ThermometerClass {
  private(set) var temperature: Double = 0.0
  public func registerTemperature(_ temperature: Double) {
    self.temperature = temperature
  }
}

let thermometerClass = ThermometerClass()
thermometerClass.registerTemperature(56.0)

public struct ThermometerStruct {
  private(set) var temperature: Double = 0.0
  public mutating func registerTemperature(_ temperature: Double) {
    self.temperature = temperature
  }
}

let thermometerStruct = ThermometerStruct()
thermometerStruct.registerTemperature(56.0)
```

使用可变函数正确声明ThermometerStruct以更改其内部变量temperature。编译器报错是因为你在通过 let 创建的实例上调用了registerTemperature，因为它是不可变的。将let改为var通过编译。
 对于结构体，必须将内部状态更改为mutating的方法标记，但不能从不可变变量中调用它们。

##### 这段代码会打印什么？为什么？

```csharp
var thing = "cars"

let closure = { [thing] in
  print("I love \(thing)")
}

thing = "airplanes"

closure()
```

它会打印：I love cars。声明闭包时，捕获列表会创建一个 thing 副本。这意味着即使为 thing 分配新值，捕获的值也不会改变。

如果省略闭包中的捕获列表，则编译器使用引用而不是副本。因此，当调用闭包时，它会反映对变量的任何更改。可以在以下代码中看到：

```swift
var thing = "cars"

let closure = {    
  print("I love \(thing)")
}

thing = "airplanes"

closure() // Prints: "I love airplanes"
```

##### 问题四

这是一个全局函数，用于计算数组中唯一值的数量：

```swift
func countUniques<T: Comparable>(_ array: Array<T>) -> Int {
  let sorted = array.sorted()
  let initial: (T?, Int) = (.none, 0)
  let reduced = sorted.reduce(initial) {
    ($1, $0.0 == $1 ? $0.1 : $0.1 + 1)
  }
  return reduced.1
}
```

它使用sorted方法，因此它将T限制为符合Comparable协议的类型。
 你这样调用它：

```cpp
countUniques([1, 2, 3, 3]) // result is 3
```

##### 问题: 将此函数重写为Array上的扩展方法，以便你可以编写如下内容：

```csharp
[1, 2, 3, 3].countUniques() // should print 3
```

你可以将全局countUniques（_ :)重写为Array扩展：

```swift
extension Array where Element: Comparable {
  func countUniques() -> Int {
    let sortedValues = sorted()
    let initial: (Element?, Int) = (.none, 0)
    let reduced = sortedValues.reduce(initial) { 
      ($1, $0.0 == $1 ? $0.1 : $0.1 + 1) 
    }
    return reduced.1
  }
}
```

请注意，仅当泛型 Element 符合Comparable时，新方法才可用。

##### 问题五

这是 divide 对象的两个可选 Double 类型变量的方法。在执行实际解析之前，有三个先决条件需要验证：

- 变量 dividend 必须包含非空的值
- 变量 divisor 必须包含非空的值
- 变量 divisor 不能等于零

```swift
func divide(_ dividend: Double?, by divisor: Double?) -> Double? {
  if dividend == nil {
    return nil
  }
  if divisor == nil {
    return nil
  }
  if divisor == 0 {
    return nil
  }
  return dividend! / divisor!
}
```

##### 使用guard语句并且不使用强制解包来改进此功能。

```swift
func divide(_ dividend: Double?, by divisor: Double?) -> Double? {
    guard let dividend = dividend, let divisor = divisor, divisor != 0  else {
       return nil
    }
       return dividend / divisor
    }
```

##### 问题六

使用 if let 重写第五题

```swift
func divide(_ dividend: Double?, by divisor: Double?) -> Double? {
   if let dividend = dividend, let divisor = divisor, divisor != 0 {
      return dividend / divisor
   } else {
     return nil
   }
}
```

### 中级口述问题

##### 第一题

在Objective-C中，声明一个这样的常量：

```cpp
const int number = 0;
```

Swift对应的写法：

```tsx
let number = 0
```

const是在编译时使用值或表达式初始化的变量，必须在编译时解析。
 使用let创建的不可变是在运行时确定的常量。你可以使用静态或动态表达式对其进行初始化。这允许声明如下：

```tsx
let higherNumber = number + 5
```

请注意，只能分配一次值。

##### 第二题

声明一个 static 修饰的属性或函数，请在值类型上使用static修饰符。这是一个结构的例子：你用 static 修饰值类型。这是一个结构体的例子：

```swift
struct Sun {
  static func illuminate() {}
}
```

对于类，可以使用static或class修饰符。他们实现了相同的目标，但方式不同。你能解释一下它们有何不同？

static 使属性或方法为静态并不可重写。使用class可以重写属性或方法。
 应用于类时，static将成为class final的别名。
 例如，在此代码中，当你尝试重写 illuminate() 时，编译器会抱错：

```swift
class Star {
  class func spin() {}
  static func illuminate() {}
}
class Sun : Star {
  override class func spin() {
    super.spin()
  }
  // error: class method overrides a 'final' class method
  override static func illuminate() { 
    super.illuminate()
  }
}
```

##### 问题三

你可以在 extension 里添加存储属性吗？为什么行或不行呢？

不，这是不可能的。你可以使用扩展来向现有类型添加新行为，但不能更改类型本身或其接口。如果添加存储的属性，则需要额外的内存来存储新值。扩展程序无法管理此类任务。

##### 问题四

Swift中的协议是什么？

协议是一种定义方法，属性和其他要求蓝图的类型。然后，类，结构或枚举可以遵守协议来实现这些要求。
 遵守协议需要实现该协议的要求。该协议本身不实现任何功能，而是定义功能。可以扩展协议以提供某些要求的默认实现或符合类型可以利用的其他功能。

## 高级书面问题

##### 第一题

思考以下 thermometer 模型结构体：

```swift
public struct Thermometer {
  public var temperature: Double
  public init(temperature: Double) {
    self.temperature = temperature
  }
}
```

可以使用以下代码创建实例：

```csharp
var t: Thermometer = Thermometer(temperature:56.8)
```

但以这种方式初始化它会更好：

```csharp
var thermometer: Thermometer = 56.8
```

你可以吗? 要怎么做?

Swift定义了一些协议，使你可以使用赋值运算符初始化具有文字值的类型。

采用相应的协议并提供公共初始化器允许特定类型的文字初始化。在 Thermometer 的情况下，实现ExpressibleByFloatLiteral如下：

```swift
extension Thermometer: ExpressibleByFloatLiteral {
  public init(floatLiteral value: FloatLiteralType) {
    self.init(temperature: value)
  }
}
```

现在，可以使用float创建实例。

```csharp
var thermometer: Thermometer = 56.8
```