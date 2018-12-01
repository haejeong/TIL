# Articles

## Choosing Between Structures and Classes

구조체와 클래스는 데이터를 저장하거나 애플리케이션 동작을 모델링할 때 좋은 선택이지만, 구조체와 클래스는 유사하기 때문에 하나를 선택하기 어려울 수 있다. 

적절한 옵션을 선택하기 위해서는 아래와 같은 사항을 생각해보자. 

### 기본적으로 구조체를 사용하자.
> 구조를 사용하여 일반적인 종류의 데이터를 나타낸다. Swift의 구조에는 다른 언어의 클래스로 제한된 많은 기능이 포함되어 있다. 저장된 속성, 계산된 속성 및 메서드가 포함될 수 있다. 더욱이, 스위프트 구조는 기본 구현을 통해 행동을 취하기 위해 프로토콜을 채택할 수 있다. Swift 표준 라이브러리 및 Foundation은 우리가 자주 사용하는 숫자, 문자열, 어레이 및 사전과 같이 자주 사용하는 유형에 대한 구조체를 사용한다.  

구조를 사용하면 앱의 전체 상태를 고려할 필요 없이 코드의 일부에 대해 쉽게 설명할 수 있습니다. 구조는 클래스와는 달리 값 유형이기 때문에 변경 내용을 앱 흐름의 일부로 의도적으로 전달하지 않는 한 구조물에 대한 로컬 변경은 앱의 나머지 부분에 표시되지 않습니다. 따라서 코드의 한 섹션을 보고 해당 섹션의 인스턴스 변경이 눈에 보이지 않게 관련된 기능 호출을 통해 수행되기 보다는 명시적으로 변경된다는 점을 더 확신할 수 있습니다.

구조체를 사용하면 앱 전체의 상태를 생각할 필요없이 코드의 부분에 대해 쉽게 추론할 수 있다. 구조체는 클래스와는 다르게 값 유형이기 때문에 변경 내용을 앱 흐름의 일부로 의도적으로 전달하지 않는 이상 구조체에 대한 변경은 앱의 나머지 부분이 표시되지 않는다.(????? - 다시 확인하기). 따라서 코드의 한 부분을 보고 해당 코드의 인스턴스 변경이 눈에 보이지 않게 관련된 기능 호출을 통해 수행되기 보다는 명시적으로 변경되는 것을 확인 할 수 있다. 

###  Obj-c 와의 상호처리가 필요할 때는 클래스를 사용하자. 


### 모델링하는 데이터의 ID를 제어해야 하는 경우 클래스를 사용한다.


### 프로토콜을 사용하여 공유 implement 을 적용할 때 구조체를 사용한다. 


## Using Key-Value Observing in Swift


>  다른 Object 에게 Property 의 변화를 알려준다. 

#### Overview

Key-value obvserving 은 다른 Object 에게 Property 들의 변화를 알려줄 수 있는 코코아 프로그래밍 패턴이다. 모델과 뷰처럼 논리적으로 분리되어있는 것들에게 변화를 알려줄 때 유용하게 사용할 수 있다. NSObject 를 상속받는 클래스에서만 key-value observing 을 사용할 수 있다. 

#### Annotate a Property for Key-Value Observing

key-value observing 을 하고 싶은 프로퍼티에 @objc 와 dynamic 을 사용한다. 아래 예제에서 MyObjectToObserve 클래스에 myDate 프로퍼티의 변화를 감지할 수 있다. 

```swift
class MyObjectToObserve: NSObject {
    @objc dynamic var myDate = NSDate(timeIntervalSince1970: 0) // 1970
    func updateDate() {
        myDate = myDate.addingTimeInterval(Double(2 << 30)) // Adds about 68 years.
    }
}
```

#### Define an Observer

Observer 클래스의 인스턴스는 하나 이상의 Property 들의 변화를 관리할 수 있다. observer 를 생성할 때`observe(_:options:changeHandler)` 메소드를 호출함으로써 observing 을 시작 할 수 있다. 

아래 예제와 같이 `\.objectToObserve.myDate` MyObjectToObserve 의 프로퍼티인 myDate 를 키패스를 참조하게 한다. 

```swift
class MyObserver: NSObject {
    @objc var objectToObserve: MyObjectToObserve
    var observation: NSKeyValueObservation?
    
    init(object: MyObjectToObserve) {
        objectToObserve = object
        super.init()
        
        observation = observe(
            \.objectToObserve.myDate,
            options: [.old, .new]
        ) { object, change in
            print("myDate changed from: \(change.oldValue!), updated to: \(change.newValue!)")
        }
    }
}
```

`NSKeyValueObservedChange` 의 프로퍼티인 oldValue 와 newValue 를 이용하여 값이 어떻게 바뀌었는지 관찰 할 수 있다. 

값이 어떻게 변화하였는지 알고 싶지 않을 때는 options 파라메터를 비워두면 된다. options 를 비워두면 oldValue 와 newValue 프로퍼티가 nil 이 된다. 

#### Associate the Observer with the Property to Observe

Object 를 Observer 의 initializer 에 전달하여 프로퍼티들을 Observer 와 연결한다. 

```swift
let observed = MyObjectToObserve()
let observer = MyObserver(object: observed)
```

#### Respond to a Property Change

key-value observing 을 설정한 Object들은 이들의 Observer 에게 프로퍼티의 값 변화를 알려준다. 아래와 같은 예제에서는 updateDate 메소드를 호출함으로써 myDate 프로퍼티의 값을 변경한다. 이 메소드는 Observer 의 변경 처리기를 자동적으로 트리거한다. 

```swift
observed.updateDate() // Triggers the observer's change handler.
// Prints "myDate changed from: 1970-01-01 00:00:00 +0000, updated to: 2038-01-19 03:14:08 +0000"
```

## Adopting Common Protocols

커스텀 타입을 사용하여 데이터를 모델링할 때 두 값이 동일한지 또는 다른지 또는 특정 값이 값 목록에 포함되어 있는지 확인해야 하는 경우가 많다. 이 기능은 값을 집합에 저장하거나 Dictionary의 키로 사용하는 기능뿐만 아니라 관련된 두 가지 표준 라이브러리 프로토콜인 Equible 및 Hashable로 관리한다. 

- 등호 (==) 와 같지 않음 (!=) 연산자를 사용하여 등가 유형의 인스턴스를 비교할 수 있다. 
- 해시 가능한 유형의 인스턴스는 값을 수학적으로 Integer 로 줄일 수 있고, 이 값은 Sets, Dicationary 를 이용하여 검색 속도를 일관되게 높일 수 있다. 

문자열, 정수, 부동 소수점 값, bool 값, 집합을 포함하여 많은 표준 라이브러리 유형은 동등하고 해시 가능한 형식입니다. 다음 예에서 == 비교와 contains(_:) 메서드 호출은 문자열과 정수가 동일하다는 점에 따라 달라집니다.

```swift
if username == "Arturo" {
    print("Hi, Arturo!")
}

let favoriteNumbers = [4, 7, 8, 9]
if favoriteNumbers.contains(todaysDate.day) {
    print("It's a good day today!")
}
```

Equatable과 Hashable 프로토콜을 준수한다는 것은 간단하며 Swift에서 자신의 유형을 더 쉽게 사용할 수 있게 합니다. 모든 사용자 정의 모델 유형이 일치하는 것이 좋다. 



