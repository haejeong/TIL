# Foundation

## Collection 

### Set

> 순서가 정해저 있지 않은 Collection

```swift
struct Set<Element> where Element : Hashable
```

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