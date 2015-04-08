---
layout: post
title: Swift中的遍历（Iteration）#1
category: 
  - Learning Notes
comments: true
tags: Swift
---

```swift
for item in someArray {}
for character in someString {}
for item:String in aStringArray {}
for i in 1...10 {}
for i in 1..<10 {}
for var i = 1; i < 10; ++i {}
for _ in 1...10 {}
for (key, value) in someDictionary {}
for (index, value) in enumerate(array) {}
```

<!--more-->

### 如何让自己写的类在for...in这样的句式中使用呢？
凡是能在这个句式中使用的类，都是实现了SequenceType Protocal的。

```swift
for x in someSequence {}
```
Swift在编译的时候会将其翻译成下面的代码

```swift
var __g = someSequence.generate() // SequenceType protocol
while let x = __g.next() {}       // GeneratorType protocol
```
下面举个例子：

```swift
class ParkingLot: SequenceType{
    var cars = [Car]() // 假设我们已经定义了一个汽车类
    func park(car: Car) {
        cars.append(car)
    }
    func unpark() -> Car? {
        if !cars.isEmpty {
            return cars.removeLast()
        }
        return nil
    }
    //下面的函数就是实现SequenceType所需要的
    func generate() -> CarGenerator {
        return CarGenerator(cars: cars) // 接着我们再来定义一个CarGenerator类
    }
}
class CarGenerator: GeneratorType {
    private var cars: [Car]?
    private var nextIndex: Int
    init(cars: [Car]){
        self.cars = cars
        nextIndex = cars.count - 1
    }
    func next() -> Car? {
        if (nextIndex < 0) {
            return nil
        }
        return self.cars![nextIndex--]
    }
}
```
下面我们写一段测试代码

```swift
var c1 = Car(name: "A")
var c2 = Car(name: "B")
var c3 = Car(name: "C")
var lot = ParkingLot()
lot.park(c1)
lot.park(c2)
lot.park(c3)
for car in lot {
    println(car)
}
// 输出结果是：
// Car C
// Car B
// Car A
```
这里Swift还提供了一种更简便的方法（省去一个Generator类）

```swift
class ParkingLot: SequenceType {
    // ...
    // Swift 提供了一个范型结构体 GeneratorOf<T>
    func generate() -> GeneratorOf<Car> {
        var nextIndex = cars.count - 1
        return GeneratorOf<Car> {
            if (nextIndex < 0) {
                return nil;
            }
            return self.cars[nextIndex--]
        }
    }
}
```
### 附录
WWDC幻灯片上给出了两个Protocol的定义如下

```swift
protocol Generator {
	typealias Element
	mutating func next() -> Element?
}
protocol Sequence {
	typealias GeneratorType: Generator
	func generate() -> GeneratorType
}
```
### 参考资料
1. [WWDC 2014 Session Video: Advanced Swift](https://developer.apple.com/videos/wwdc/2014/#404)
2. http://robots.thoughtbot.com/swift-sequences
3. http://lillylabs.no/2014/09/30/make-iterable-swift-collection-type-sequencetype/
