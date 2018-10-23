# Swift

## String

### Operators

#### `..<(_:_:)`
```swift
static func ..< (minimum: String, maximum: String) -> Range<String>
```

minimum 보다 크거나 같고 maximum 보다 작은 범위를 리턴한다. 

```swift
let lessThanFive = 0.0..<5.0
print(lessThanFive.contains(3.14))  // Prints "true"
print(lessThanFive.contains(5.0))   // Prints "false"
```

#### `...(_:_:)`
```swift
static func ... (minimum: String, maximum: String) -> ClosedRange<String>
```
minimum 보다 크거나 같고, maximum 보다 작거나 같은 범위를 리턴한다.


```swift
let lowercase = "a"..."z"
print(lowercase.contains("z"))
// Prints "true"
```

#### `..<(_:)`
```swift
prefix static func ..< (maximum: String) -> PartialRangeUpTo<String>
```
maximum 보다 작은 범위를 리턴한다. 

```swift
let upToFive = ..<5.0

upToFive.contains(3.14)       // true
upToFive.contains(6.28)       // false
upToFive.contains(5.0)        // false
```

#### `...(_:)`
```swift
prefix static func ... (maximum: String) -> PartialRangeThrough<String>
```

maximum 보다 작거나 같은 값의 범위를 리턴한다. 

```swift
let throughFive = ...5.0

throughFive.contains(4.0)     // true
throughFive.contains(5.0)     // true
throughFive.contains(6.0)     // false 
```

#### `...(_:)`
```swift
postfix static func ... (minimum: String) -> PartialRangeFrom<String>
```
minimum 이상의 범위를 반환한다. 

```swift
let atLeastFive = 5.0...

atLeastFive.contains(4.0)     // false
atLeastFive.contains(5.0)     // true
atLeastFive.contains(6.0)     // true
```


## Dictionary 

### `init(uniqueKeysWithValues:)`
> key-value 형태로 되어있는 시퀀스로 딕셔너리를 새로 만들 수 있다. 

```swift
init<S>(uniqueKeysWithValues keysAndValues: S) where S : Sequence, S.Element == (Key, Value)
```

아래와 같이 digitWords와 1...5 두개의 시퀀스 값을 zip 으로 페어링 시킨 후에  
`Dictionary(uniqueKeysWithValues:)` 로 새로운 dicationary 를 만든 것을 볼 수 있음.
```swift
let digitWords = ["one", "two", "three", "four", "five"]
let wordToValue = Dictionary(uniqueKeysWithValues: zip(digitWords, 1...5))
print(wordToValue["three"]!)
// Prints "3"
print(wordToValue)
// Prints "["three": 3, "four": 4, "five": 5, "one": 1, "two": 2]"
```

### `init(_:uniquingKeysWith:)`
> key-value 형태로 되어있는 시퀀스로 딕셔너리를 새로 만들 수 있는데,  
클로저를 이용하여 중복값을 처리할 수 있다. 

예제는 아래와 같다. 

```swift
let pairsWithDuplicateKeys = [("a", 1), ("b", 2), ("a", 3), ("b", 4)]

let firstValues = Dictionary(pairsWithDuplicateKeys,
                             uniquingKeysWith: { (first, _) in first })
// ["b": 2, "a": 1]

let lastValues = Dictionary(pairsWithDuplicateKeys,
                            uniquingKeysWith: { (_, last) in last })
// ["b": 4, "a": 3]
```

### `init(grouping:by:)`
> 클로저에서 리턴하는 값으로 Key 를 만들고, 해당 키에 조건에 맞는 Value 를 맵핑하여 새로운 Dictionary 를 만든다. 


```swift
init<S>(grouping values: S, by keyForValue: (S.Element) throws -> Dictionary<Key, Value>.Key) rethrows where Value == [S.Element], S : Sequence
```

예제는 아래와 같다.  
`students` 배열 안 값들의 첫 글자를 key로,  
해당 key 를 첫글자로 시작하는 배열의 값들이 value 로 들어가는 것을 확인 할 수 있다. 

```swift
let students = ["Kofi", "Abena", "Efua", "Kweku", "Akosua"]
let studentsByLetter = Dictionary(grouping: students, by: { $0.first! })
// ["E": ["Efua"], "K": ["Kofi", "Kweku"], "A": ["Abena", "Akosua"]]
```

### `allSatisfy(_:)`
> Returns a Boolean value indicating whether every element of a sequence satisfies a given predicate.

주어진 조건을 모두 만족할 경우에는 true, 그렇지 않을 경우 false 를 리턴한다.

### `merge(_:uniquingKeysWith:)`
> Merges the given dictionary into this dictionary, using a combining closure to determine the value for any duplicate keys.

```swift
mutating func merge(_ other: [Dictionary<Key, Value>.Key : Dictionary<Key, Value>.Value], uniquingKeysWith combine: (Dictionary<Key, Value>.Value, Dictionary<Key, Value>.Value) throws -> Dictionary<Key, Value>.Value) rethrows
```

parameter: 합칠 Dictionary
combine: 중복된 키를 위한 기존 값과 새 값들을 알 수 있는 클로저. 최종 Dictionary 에 넣을 값을 리턴한다.


```swift
var dictionary = ["a": 1, "b": 2]

// Keeping existing value for key "a":
dictionary.merge(["a": 3, "c": 4]) { (current, _) in current }
// ["b": 2, "a": 1, "c": 4]

// Taking the new value for key "a":
dictionary.merge(["a": 5, "d": 6]) { (_, new) in new }
// ["b": 2, "a": 5, "c": 4, "d": 6]
```

위와 같이 closure 에서 current 값으로 리턴할 경우 기존 값을 유지, new 로 리턴할 경우 새로 들어간 값으로 대체된다.


```swift
let names = ["Sofia", "Camilla", "Martina", "Mateo", "Nicolás"]
let allHaveAtLeastFive = names.allSatisfy({ $0.count >= 5 })
// allHaveAtLeastFive == true
```
위와 같이 Dictionary `names` 에 있는 모든 문자열 길이가 5이상이므로 true 를 리턴한다. 



## DictionaryLiteral
> A lightweight collection of key-value pairs.

`DictionaryLiteral`는 키 조회가 빠를 필요없지만, 순서가 지정된 key-value 형태의 collection 를 사용하고 싶을 때 사용한다.  

```swift
let recordTimes: DictionaryLiteral = ["Florence Griffith-Joyner": 10.49,
                                      "Evelyn Ashford": 10.76,
                                      "Evelyn Ashford": 10.79,
                                      "Marlies Gohr": 10.81]
```

`DictionaryLiteral`는 순서가 유지되는 것뿐만이 아니라 중복 키도 허용된다.  
위의 예제에서 `Evelyn Ashford`라는 키가 두번이 들어간 것을 확인 할 수 있다. 

`DictionaryLiteral` 의 일부는 효율이 좋지 않은데, 
특히 특정 키와 일치하는 값을 찾기 위해서는 collection 의 모든 요소들을 돌리며 찾아내야한다. 

`DictionaryLiteral`는 함수를 호출 할 때 매개변수로 넘길 수 있다. 

```swift
let pairs = IntPairs([1: 2, 1: 1, 3: 4, 2: 1])
```



## Overflow Operators
>Xcode 10.0+ 부터 가능한 Overflow Operators

Overflow Operator 는 너무 크거나, 너무 작은 숫자를 안전하게 연산할 수 있도록 해준다.  
예를 들어 Int16 는 -32768 와 32767 를 나타낼 수 있는데 만약 이 범위를 벗어나면 에러가 발생한다. 

```swift
var potentialOverflow = Int16.max
// potentialOverflow equals 32767, which is the maximum value an Int16 can hold
potentialOverflow += 1
// this causes an error
```
아래와 같은 연산자를 사용한다.
```
static func &+ (Int, Int) -> Int
static func &- (Int, Int) -> Int
static func &* (Int, Int) -> Int
```

### Value Overflow
```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow equals 255, which is the maximum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow is now equal to 0
```
UnsignedOverflow 변수는 UInt8이 가질 수 있는 최대 값(255 또는 11111111)으로 초기화된다.  
`&+`를 이용하여 1을 증가시키면 이렇게 하면 오버플로우 추가 후 UInt8의 범위 내에 남아 있는 값은 00000000 또는 0이 된다.

![Alt text](overflowAddition_2x.png "Optional title")

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow is now equal to 255
```
UInt8이 가질 수 있는 가장 작은 값은 0 또는 00000000이다.  
만약 `&-`를 이용하여 1을 감소시키면 값은 11111111 또는 255가 된다.

![Alt text](overflowUnsignedSubtraction_2x.png "Optional title")

Int8가 가질 수 있는 최소 값은 -128 또는 10000000이다.  
만약 `&-`를 이용하여 1을 감소시키면 Int8의 최대값인 01111111 또는 127이 된다.

![Alt text](overflowSignedSubtraction_2x.png "Optional title")


## Articles

### Conform Automatically to Equatable and Hashable

`Equatable`와 `Hashable` 프로토콜을 선언하여 여러가지 유형으로 equatable & hashable 을 구현할 수 있다. 

```swift 
struct Position: Equatable, Hashable {
    var x: Int
    var y: Int
    
    init(_ x: Int, _ y: Int) {
        self.x = x
        self.y = y
    }
}
```
위와 같이 Equatable, Hashable 프로토콜을 선언해주면,  
아래와 같이 `==`와 `contains`를 사용할 수 있게된다. 

```swift
let availablePositions = [Position(0, 0), Position(0, 1), Position(1, 0)]
let gemPosition = Position(1, 0)

for position in availablePositions {
    if gemPosition == position {
        print("Gem found at (\(position.x), \(position.y))!")
    } else {
        print("No gem at (\(position.x), \(position.y))")
    }
}
// No gem at (0, 0)
// No gem at (0, 1)
// Gem found at (1, 0)!

var visitedPositions: Set = [Position(0, 0), Position(1, 0)]
let currentPosition = Position(1, 3)

if visitedPositions.contains(currentPosition) {
    print("Already visited (\(currentPosition.x), \(currentPosition.y))")
} else {
    print("First time at (\(currentPosition.x), \(currentPosition.y))")
    visitedPositions.insert(currentPosition)
}
// First time at (1, 3)
```

나는 아래와 같이 주소록정보에서 `==`의 정의를 아래와 같이 해주었다. 

```swift
extension AddressInfoModel: Equatable {
    public static func == (lhs: AddressInfoModel, rhs: AddressInfoModel) -> Bool {
        return lhs.name == rhs.name && lhs.addr == rhs.addr
    }
}
```

Equatable and Hashable 프로토콜을 사용하기 위해서는 .. 

*  구조체에서는 모든 프로퍼티들은 Equatable / Hashable 를 준수해야한다. 
*  열거형에서는 모든 associated values가 Equatable / Hashable 를 준수해야한다.  
** associated value가 없는 열거형은 따로 선언하지 않아도 Equatable와 Hashable 를 준수한다. 