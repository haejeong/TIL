# Swift

## Dictionary 

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

### DictionaryLiteral
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