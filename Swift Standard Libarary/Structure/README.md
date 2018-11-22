# Basic Behaviors

## Structure

### Hasher

Set 과 Dictionary 에서 사용하는 범용 Hash 함수이다. 

#### Declaration

```swift
struct Hasher
```

#### Overview

Hash 를 사용하여 임의 바이트 시퀀스를 정수 해시 값이 매핑할 수 있다. mutating `combine` 메소드를 연속으로 사용하여 사용하여 해쉬값을 제공할 수 있다. `finalize()` 메소드를 사용하여 해쉬값을 받아 낼 수 있다. 

```swift
var hasher = Hasher()
hasher.combine(23)
hasher.combine("Hello")
let hashValue = hasher.finalize()
```

Swift 프로그램을 실행하는 동안 Hasher 는 동일한 바이트 시퀀스를 제공하는 한 항상 같은 해시값을 제공한다. 그러나 기본 해시 알고리즘은 seed 나 입력 바이트 시퀀스를 약간 변경하면 일반적으로 생성된 해시 값이 크게 변경 된다. 

> 프로그램을 실행하는 동안 해시값을 제사용하거나 저장하지 말자. Hasher 는 대체로 randomly seed 되고, 이것은 프로그램 실행 시마다 다름 값이 반환되는 것을 의미한다. Haser 로 구현한 해시 알고리즘은 standard library 의 두 버전 간에 스스로 변경 될 수 있다. 

### FlattenSequence

> Base sequence 포함된 각 세그먼트에 포함된 모든 요소로 구성된 시퀀스

#### Declaration
```swift
struct FlattenSequence<Base> where Base : Sequence, Base.Element : Sequence
```

#### Overview

이 뷰의 요소는 베이스에 있는 각 시퀀스의 요소를 합친 것이다.

결합된 방법은 항상 lazy이지만 결과에 적용되는 알고리즘에 대한 lazyiness를 암시적으로 부여하지는 않는다. 즉, 일반 시퀀스 s의 경우:

- s.joined() 는 새 저장소를 생성하지 않는다. 
- s.joined().map(f) map으로 새로운 array 를 리턴한다. 
- s.lazy.joined().map(f) lazy 하게 map을 이용하여 LazyMapSequence를 리턴한다. 


### FlattenCollection

> 기본 collection 의 flatten한 view 

#### Declaration

```swift
struct FlattenCollection<Base> where Base : Collection, Base.Element : Collection
```

#### Overview

이 뷰의 요소들은 베이스에 있는 집합들의 각 요소들을 합친 것이다. 

joined 메소드는 항상 lazy 하지만 결과에 적용되는 알고리즘에 대한 laziness 를 암시적으로 부여하지 않는다. 다시 말해 일반 collection c 에 대해서, 

- c.joined() 는 새 저장소를 생성하지 않는다. 
- c.joined().map(f) map으로 새로운 array 를 리턴한다. 
- c.lazy.joined().map(f) lazy 하게 map을 이용하여 LazyMapSequence를 리턴한다. 

### StaticString

> 컴파일 타임에 알려진 텍스트를 나타내도록 설계된 문자열

#### Declaration

```swift
struct StaticString
```

#### Overview

StaticString 유형의 인스턴스는 값을 변경할 수 없다. StaticString은 Swift에서 일반적으로 사용되는 String 유형과 달리 제한된 포인터 기반 콘텐츠 액세스를 제공한다. static 문자열은 해당 값을 ASCII 코드 단위 시퀀스에 대한 포인터, UTF-8 코드 단위 시퀀스에 대한 포인터 또는 단일 유니코드 스칼라 값으로 저장할 수 있다. 

##### hasPointerRepresentation

> static 문자열이 포인터를 ASCII 또는 UTF-8 코드 단위로 저장할지 여부를 나타내는 boolean 값.

```swift
var hasPointerRepresentation: Bool { get }
```

