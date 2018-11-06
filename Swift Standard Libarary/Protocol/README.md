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


## Error

> 던질 수 있는 오류값을 나타내는 타입

`Error` 프로토콜을 준수하는 모든 유형은 Swift 의 오류 처리 시 사용할 수 있다. `Error Protocol`에는 요구 사항이 없다.  

### Using Enumerations as Errors

Swift 의 열거형들은 단순한 에러를 표현하기에 적합하다. 가능한 각 오류에 대한 케이스를 포함하는 `Error Protocol` 을 준수하는 열거형을 만든다.     복구에 이용할 수 있는 오류에 대한 추가 정보가 있는 경우 관련 값을 associated value 로 넘긴다. 

다음은 `IntParsingError` 예제, 문자에서 숫자로 파싱 할 때 발생할 수 있는 두 가지 유형의 오류를 보여준다.

```swift
enum IntParsingError: Error {
    case overflow
    case invalidInput(String)
}
```

`invalidInput` 케이스에는 잘못된 문자가 associated value 로 포함된다. 

다음 코드 샘플은 문자열 인스턴스의 정수 값을 구문 분석하는 Int 유형에 대한 확장 가능성을 보여 주고 구문 분석 중에 오류가 발생할 경우 오류를 표시합니다.

다음 샘플은 문자열의 인스턴스를 정수로 파싱 할 때에 발생하는 에러를 Int 의 extension 으로 보여준다.  

```swift
extension Int {
    init(validating input: String) throws {
        // ...
        if !_isValid(s) {
            throw IntParsingError.invalidInput(s)
        }
        // ...
    }
}
```

do 문에서 Int 초기화를 할 때 사용자가 정의한 에러와 associated value 에 알맞는 error 패턴으로 엑세스 할 수 있다. 

```swift
do {
    let price = try Int(validating: "$100")
} catch IntParsingError.invalidInput(let invalid) {
    print("Invalid character: '\(invalid)'")
} catch IntParsingError.overflow {
    print("Overflow error")
} catch {
    print("Other error")
}
// Prints "Invalid character: '$'"
```

### Including More Data in Errors

가끔 파일의 위치나 프로그램의 상태와 같은 동일한 공통 데이터를 다른 오류 상태로 포함 시킬 수 있다. 이 경우 구조체를 사용하여 에러를 나타낸다. 다음 예제는 xml 문서를 파싱할 때 에러가 발생한 줄 및 열 번호를 나타내도록 한다. 

```swift
struct XMLParsingError: Error {
    enum ErrorKind {
        case invalidCharacter
        case mismatchedTag
        case internalError
    }

    let line: Int
    let column: Int
    let kind: ErrorKind
}

func parse(_ source: String) throws -> XMLDoc {
    // ...
    throw XMLParsingError(line: 19, column: 5, kind: .mismatchedTag)
    // ...
}
```

다시한번 패턴 매칭을 사용하여 에러를 조건적으로 잡아낸다. 다음은 parse(_:) 함수에 의해 발생한 XMLParsingError를 감지하는 방법이다. 

```swift
do {
    let xmlDoc = try parse(myXMLData)
} catch let e as XMLParsingError {
    print("Parsing error: \(e.kind) [\(e.line):\(e.column)]")
} catch {
    print("Other error: \(error)")
}
// Prints "Parsing error: mismatchedTag [19:5]"
```


## Equatable

값이 같은지 비교할 수 있는 타입 

#### Declaration

```swift
protocol Equatable
```

#### Overview

`Equatable` 프로토콜을 준수하는 타입들은 == 나 != 을 이용하여 같음을 비교할 수 있다. `Swift Standard Libarary` 의 대부분은 `Equatable` 을 준수한다. 

일부 sequence 나 collection 작업은 element 가 Equatable 을 준수 할 때 더 간단히 사용할 수 있다. 예를 들어 배열에 특정 값이 포함되어 있는지 확인하려면, 배열 요소가 동일한지 확인한 후에 제공하는 클로저를 사용하는 대신 `contains(_:)` 메소드를 사용할 수 있다.  다음 예제에서는 문자 배열에서contains(_:) 메소드를 사용하는 방법을 보여준다. 

```swift
let students = ["Kofi", "Abena", "Efua", "Kweku", "Akosua"]

let nameToCheck = "Kofi"
if students.contains(nameToCheck) {
    print("\(nameToCheck) is signed up!")
} else {
    print("No record of \(nameToCheck).")
}
// Prints "Kofi is signed up!"
```

#### Conforming to the Equatable Protocol

만드려는 타입에 `Equatable` 프로토콜을 추가하여 collection 안에서 특정 인스턴스를 찾을 때 더 편한 API를 제공받을 수 있다. `Equatable`은 `Hashable`과 `Comparable` 프로토콜을 기반으로 하여  세트 구성이나 element 들을 정렬하는 것과 같은 더 많은 api 를 사용할 수 있다. 

- 구조체의 경우 프로퍼티들은 반드시 Equatable 을 준수해야한다. 
- 열거형에 경우 모든 associated value 들이 `Equatable` 을 준수해야한다. ( associated value 가 없는 열거형에서는 선언이 없어도 `Equatable` 을 준수하게 된다. 

Equatable 을 준수하는 하거나 위에 나열된 기준을 충족하지 않는 유형의 Equatable 을 애택하거나, 기존 유형을 Equatable 에 맞게 확정하려면 static method 인 (==) 를 재정의 하자. 표준 라이버러리는 동등하지 않은 연산자 (!=) 에 대해 사용자 정의 == 함수를 호출하고 결과를 부정하는 것을 제공하도록 구현한다. 

예를 들어 주소, 집/빌딩 번호, 거리 이름, 그리고 unit 숫자를 저장하는 StreetAddress 라는 클래가 있다고 생각해보자.  
StreetAddress 선언은 아래와 같이 했다. 

```swift
class StreetAddress {
    let number: String
    let street: String
    let unit: String?

    init(_ number: String, _ street: String, unit: String? = nil) {
        self.number = number
        self.street = street
        self.unit = unit
    }
}
```

이제 주소들을 저장한 배열에서 특정 주소를 확인해야 한다 가정해보자.  
각 요소마다 돌아가며 closure을 사용하지 않고 `contains(_:)` 메소드를 사용하기 위해서는  
StreetAddress 를 Equatable 로 확장한다. 

```swift
extension StreetAddress: Equatable {
    static func == (lhs: StreetAddress, rhs: StreetAddress) -> Bool {
        return
            lhs.number == rhs.number &&
            lhs.street == rhs.street &&
            lhs.unit == rhs.unit
    }
}
```
StreetAddress 타입은 이제 Equatable 을 준수한다.  
이제 == 를 사용하여 두 인스턴스가 같은지 비교할 수 있고, `contains(_:)`메소드를 호출하여 동등 조건을 확인 할 수 있다. 

```swift
let addresses = [StreetAddress("1490", "Grove Street"),
                 StreetAddress("2119", "Maple Avenue"),
                 StreetAddress("1400", "16th Street")]
let home = StreetAddress("1400", "16th Street")

print(addresses[0] == home)
// Prints "false"
print(addresses.contains(home))
// Prints "true"
```
