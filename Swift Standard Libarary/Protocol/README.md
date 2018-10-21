# Protocol

## RandomAccessCollection

>A collection that supports efficient random-access index traversal.

```swift
protocol RandomAccessCollection where Self.Indices : RandomAccessCollection, Self.SubSequence : RandomAccessCollection
```

Random-access collections 은 O(1) 시간 내에서 원하는 거리만큼 인덱스를 이동할 수 있으며 인덱스 간의 거리를 측정할 수 있다. 


## OptionSet

>A type that presents a mathematical set interface to a bit set.

OptionSet 프로토콜은 bitset 타입을 표현할 때 사용한다.  
rawValue 로는 Int 나 UInt 같은 FiextedWidthInteger 프로토콜을 준수하는 유형을 사용

```swift
struct ShippingOptions: OptionSet {
    let rawValue: Int

    static let nextDay    = ShippingOptions(rawValue: 1 << 0)
    static let secondDay  = ShippingOptions(rawValue: 1 << 1)
    static let priority   = ShippingOptions(rawValue: 1 << 2)
    static let standard   = ShippingOptions(rawValue: 1 << 3)

    static let express: ShippingOptions = [.nextDay, .secondDay]
    static let all: ShippingOptions = [.express, .priority, .standard]
}
```

OptionSet 에 다중 멤버를 배열로 가질 수 있음. 
```swift 
let multipleOptions: ShippingOptions = [.nextDay, .secondDay, .priority]
```

해당 OptionSet 의 값을 추가할 때에 append/insert, 제거할 때 remove, 포함되어 있는지 확인할 때에 contains 를 사용한다.

```swift 
freeOptions.insert(.express)
freeOptions.append(.express)
```

```swift
freeOptions.remove(.express)
```	

```swift
if freeOptions.contains(.priority) {
	print("You've earned free priority shipping!")
} else {
	print("Add more to your cart for free priority shipping!")
}
```

UIControlState 도 OptionSet 이구나 !! 
```swift
public struct UIControlState : OptionSet {
    public init(rawValue: UInt)
    
    public static var normal: UIControlState { get }
    public static var highlighted: UIControlState { get }
    public static var disabled: UIControlState { get }
    public static var selected: UIControlState { get }
    @available(iOS 9.0, *)
    public static var focused: UIControlState { get }
    public static var application: UIControlState { get }
    public static var reserved: UIControlState { get }
}
``` 

## CaseIterable

> A type that provides a collection of all of its values.


프로토콜 `CaseIterable` 을 준수하는 것은 associated value 가 없는 열거형이다.  
`CaseIterable`을 사용하게 되면 allCases 프로퍼티를 이용하여 열거형의 모든 값이 접근 할 수 있다. 

기존에 열거형에서 모든 값들에 접근하기 위해서는 아래와 같이 프로퍼티를 하나 더 만들었다. 

```swift
enum CompassDirection {
    case north, south, east, west

    var allCases: [CompassDirection] {
        return [.north, .south, .east, .west]
    }
}

```

하지만 swift 4에서는 아래와 같이 사용이 가능하다. 

```swift
enum CompassDirection: CaseIterable {
    case north, south, east, west
}

print("There are \(CompassDirection.allCases.count) directions.")
// Prints "There are 4 directions."
let caseList = CompassDirection.allCases
                               .map({ "\($0)" })
                               .joined(separator: ", ")
// caseList == "north, south, east, west"
```

allCases 의 순서는 선언된 순서대로 제공한다.  



