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
