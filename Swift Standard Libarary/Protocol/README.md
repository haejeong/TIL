# Protocol

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


