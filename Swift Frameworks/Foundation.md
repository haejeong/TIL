# Foundation

## Collection 

### Set

> 순서가 정해저 있지 않은 Collection

#### Declaration
```swift
struct Set<Element> where Element : Hashable
```

#### Overview
집합의 Element 순서와 상관없이 각 Element 가 한 번만 나타나는지 확인해야 할 경우 Array 대신 Set 을 사용한다

Hashable 프로토콜을 준수하는 Element 유형을 사용하여 Set 를 생성할 수 있다. 기본적으로 standart library 에 있는 대부분의 타입들은, 문자열, 숫자, Bool 유형, associated value 가 없는 열거형과 set 자체만으로도 Hasable 이다. 

Swift를 사용하면 배열을 쉽게 만들 수 있다.

```swift
let ingredients: Set = ["cocoa beans", "sugar", "cocoa butter", "salt"]
if ingredients.contains("sugar") {
    print("No thanks, too sweet.")
}
// Prints "No thanks, too sweet."
```

#### Set Operations

Set 은 수학적 set operations 들을 제공한다. 

- `contains(_:)` 은 Set 에 특정 element 가 포함되어 있는지 확인 한다. 
- `==` “equal to” 두 Set 이 같은 element 를 가지고 있는지 확인 한다. 
- `isSubset(of:)` 메소드는 다른 Set 또는 시퀀스의 모든 element 가 포함되어있는지 확인한다. 
- `isSuperset(of:)` 메소드는 Set 의 모든 요소가 다른 Set 이나 Sequence 에 모두 포함되어있는지 확인한다. 
- `isStrictSubset(of:)` 와 `isStrictSuperset(of:)` 메소드는 Set 이 상위 Set 인지, 하위 Set 인지 확인한다. 
- `isDisjoint(with:)` 메소드는 Set 에 다른 Set 와 공통된 element 를 가지고 있는지 확인한다. 

또한 Set 의 결합, 제외 또는 빼기 작업을 할 수 있다. 

- `union(_:)` 메소드를 사용하여 Set 와 다른 Set 이나 Sequence 를 합하여 새로운 Set 을 만들 수 있다. 
- `intersection(_:)` Set 와 다른 Set 또는 Sequence 의 공통된 요소로 새로운 Set 을 만들 수 있다. 
- `symmetricDifference(_:)` Set 와 다른 Set 중에 하나이지만 둘다 아닌 Element 로만 새 Set 를 만들 수 있다. 
-  `subtracting(_:)` 다른 Set 와 Sequnce 에 포함되어 있지 않은 새로운 element 로 새로운 Set 를 만들 수 있다. 

다음과 같은 메소드로 Set 자신을 수정할 수 있다. `formUnion(_:)`, `formIntersection(_:)`, `formSymmetricDifference(_:)`,  `subtract(_:)`.

Set operation 는 다른 Set 과 사용하는데 제한이 없다. 다르 Set, Array 그리고 다른 Sequence 타입들과 연산할 수 있다. 

```swift
var primes: Set = [2, 3, 5, 7]

// Tests whether primes is a subset of a Range<Int>
print(primes.isSubset(of: 0..<10))
// Prints "true"

// Performs an intersection with an Array<Int>
let favoriteNumbers = [5, 7, 15, 21]
print(primes.intersection(favoriteNumbers))
// Prints "[5, 7]"

```

#### Sequence and Collection Operations

Set 타입의 Set operations 외에도 다른 Collection 메소드들과 사용할 수 있다. 

```swift
if primes.isEmpty {
    print("No primes!")
} else {
    print("We have \(primes.count) primes.")
}
// Prints "We have 4 primes."

let primesSum = primes.reduce(0, +)
// 'primesSum' == 17

let primeStrings = primes.sorted().map(String.init)
// 'primeStrings' == ["2", "3", "5", "7"]

```

아래와 같이 Set 의 순서가 없는 요소들을 나열 할 수 있다. 

```swift
for number in primes {
    print(number)
}
// Prints "5"
// Prints "7"
// Prints "2"
// Prints "3"
```

```swift
let morePrimes = primes.union([11, 13, 17, 19])

let laterPrimes = morePrimes.filter { $0 > 10 }
// 'laterPrimes' is of type Array<Int>

let laterPrimesSet = Set(morePrimes.filter { $0 > 10 })
// 'laterPrimesSet' is of type Set<Int>

```


