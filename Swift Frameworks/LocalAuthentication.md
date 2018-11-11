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

#### Evaluating Authentication Policies

##### evaluatePolicy(_:localizedReason:reply:)

> 지정된 인증 정책을 평가한다. 

```swift
func evaluatePolicy(_ policy: LAPolicy, 
    localizedReason: String, 
              reply: @escaping (Bool, Error?) -> Void)
```

이 메소드는 인증 정책을 비동기로 평가한다. 정책을 평가하려면 사용자에게 다양한 종류의 상호 작용 또는 인증을 요청해야 한다. 실제 동작은 평가된 정책과 장치 유형에 따라 달라진다. 설치된 설정 프로필의 영향을 받을 수 있다. 

인증 다이얼로그에서 사용자에게 표시되는 현지화된 문자열에서 인증 요청에 대한 명확한 이유를 제공하고 결과 작업을 설명한다. 메시지를 간결하고 명확하게 하고 사용자의 언어로 제공한다. 인증 다이얼로그에 이미 나타나는 앱 이름을 포함하지 말자.

이전에 성공한 정책 평가가 미래의 평가도 성공할 것이라는 것을 의미할거라 가정하지 말자. 정책 평가는 사용자나 시스템에 의한 취소를 포함하여 다양한 이유로 실패할 수 있다. 

##### evaluatedPolicyDomainState

> 평가된 정책 도메인의 현재 상태

```swift
var evaluatedPolicyDomainState: Data? { get }
```

이 속성은 `canEvaluatePolicy(_:error:)` 메소드가 생체 인식 정책에 성공하거나 `evaluatePolicy(_:localizedReason:reply:)` 메소드가 호출되고 성공적인 생체 인증인 경우에만 값을 리턴한다. 그렇지 않으면 0이 반환된다. 

반환된 데이터는 불투명 구조이다. 이 값을 사용하여 이 속성에서 반환된 다른 값과 비교하여 승인된 데이터베이스가 업데이트되었는지 확인할 수 있다. 그러나 변경의 특성은 이 데이터에서 결정할 수 없다.

#### Customizing Authentication Prompts

##### localizedReason

> 사용자에게 제공된 대화상자에 표시된 인증에 대한 현지화된 설명.

이 속성은 인증 사유가 `evaluatePolicy(_:localizedReason:reply:)`에 제공된 경우 덮어쓴다.

사용자에게 표시되는 현지화된 문자열은 해당 문자열이 자신을 인증하도록 요청하는 이유와 해당 인증을 기반으로 수행할 작업에 대한 명확한 이유를 제공한다. 이 문자열은 사용자의 현재 언어로 제공되어야 하며 짧고 선명해야 한다. 인증 대화 상자의 다른 위치에 표시되므로 앱 이름을 포함할 수 없다. 

##### touchIDAuthenticationAllowableReuseDuration

> 터치 ID 인증 재사용이 허용되는 시간

```swift
var touchIDAuthenticationAllowableReuseDuration: TimeInterval { get set }
```

사용자가 지정된 시간 간격 내에 터치 ID를 사용하여 장치의 잠금을 해제하면 사용자에게 터치 ID를 입력하라는 메시지가 표시되지 않고 수신기에 대한 인증이 자동으로 성공한다. 이는 사용자가 기기의 잠금을 해제하고 나서 다른 지문을 입력하라는 메시지가 거의 즉시 나타나는 시나리오를 우회한다.

기본값은 0. 즉, 터치 ID 인증이 재사용되지 않는다.

터치 ID 인증 재사용에 허용되는 최대 기간은 `LATouchIDAuthenticationMaximumAllowableReuseDuration` 상수로 지정된다. 이 속성을 이 상수보다 큰 값으로 설정하면 더 긴 기간을 지정할 수 없다.







