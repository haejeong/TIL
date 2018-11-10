## LAContext

> 인증 정책과 접근 제어를 평가하는 메커니즘

### Declaration

```swift
class LAContext : NSObject
```

### Overview

인증 컨덱스트를 이용하여 터치ID 또는 페이스ID와 같은 생체 인식기와 패스코드를 제공하여 사용 사용자의 ID 를 평가할 수 있다. 컨텍스트는 사용자 상호 작용을 처리하고 생체 측정 데이터를 관리하는 기본 하드웨어 요소인 Secure Enclave에도 연결한다. 컨텍스트를 생성 및 구성하고 컨텍스트를 요청하여 인증을 수행한다. 인증 성공 또는 실패를 나타내는 비동기 콜백과 오류 인스턴스를 받게 된다. 

> 앱에 생체 인증을 허용하기 위해서 Info.plist 파일에 NSFaceIDUsageDescription 키를 포함시킨다. 그러지 않으면 인증 요청은 실패하게 될 것 이다. 

### Topics

#### Checking Availability

##### canEvaluatePolicy(_:error:)

> 지정된 정책에 대해 인증을 진행할 수 있는지 여부를 확인한다.

#### Declaration

```swift
canEvaluatePolicy(_:error:)
```
- 만약 정책 평가가 가능하면 true, 그렇지 않으면 false 를 리턴한다. 
- 터치 ID 또는 페이스 ID가 비활성화된 경우 생체 인식을 요구하는 정책을 인증할 수 없다.
- 시스템의 변경으로 인해 변경될 수 있으므로 이 방법의 반환 값을 저장하지 않는다. 

##### LAPolicy

> 사용 가능한 로컬 인증정책의 모음이다. 

```swift
case deviceOwnerAuthenticationWithBiometrics
// 생체 인증
case deviceOwnerAuthentication
// 생체 인증 또는 기기 암호로 인증
```

##### biometryType

> 장치에서 지원하는 생체 인증 유형

```swift
var biometryType: LABiometryType { get }
```

이 속성 값을 사용하여 생성한 인증 관련 사용자 프롬프트 장치의 생체 인식 기능과 일치 하는지 확인한다. 예르 들어 이 속성의 값이 `LABiometryType.faceID` 인 경우에는 Touch Id 를 참조하지 말자.  

이 속성은 `canEvaluatePolicy(_:error:)` 메서드를 호출한 후에만 설정되며 호출이 반환하는 것과 관계없이 설정된다. 기본값은`LABiometryType.none` 이다. 


##### LABiometryType

> 가능한 생체 인증 타입들의 모음

```swift
enum LABiometryType : Int
```

```swift
case none // 생체 인증을 제공하지 않음
case faceID // 디바이스가 faceId 를 제공함
case touchID // 디바이스가 touchId 를 제공함
```