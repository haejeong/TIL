# Swift

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
UnsignedOverflow 변수는 UInt8이 가질 수 있는 최대 값(255 또는 11111111)으로 초기화된다. `&+`를 이용하여 1을 증가시키면 이렇게 하면 오버플로우 추가 후 UInt8의 범위 내에 남아 있는 값은 00000000 또는 0이 된다.

![Alt text](overflowAddition_2x.png "Optional title")

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow is now equal to 255
```
UInt8이 가질 수 있는 가장 작은 값은 0 또는 00000000이다. 만약 `&-`를 이용하여 1을 감소시키면 값은 11111111 또는 255가 된다.

![Alt text](overflowUnsignedSubtraction_2x.png "Optional title")

Int8가 가질 수 있는 최소 값은 -128 또는 10000000이다. 만약 `&-`를 이용하여 1을 감소시키면 Int8의 최대값인 01111111 또는 127이 된다.

![Alt text](overflowSignedSubtraction_2x.png "Optional title")