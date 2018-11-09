## Files App 에 앱 폴더 공유 하기

기존에 앱 내에의 도큐먼트 디렉토리를 사용하곤 했다. 

```swift
static let documentDirectory = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true).first!
static let documentDirectoryUrl = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
```

ios 11 부터 제공하는 Files 앱 내에서 그 동안 첨부 파일을 다운로드 한 나의 앱 디렉토리를 공개하고 싶다면 아래와 같이 plist 에 아래와 설정한다.

```swift
<key>UIFileSharingEnabled</key>
<true/>
<key>LSSupportsOpeningDocumentsInPlace</key>
<true/>
```

![](files-app.png)

위의 이미지와 같이 폴더가 보이고, 그 안에 저장한 파일들을 확인 할 수 있다. 

중요 파일들을 가리고 싶다면, 도큐먼트디렉토리 대신 아래와 같이 설정하도록 한다. 

```swift
static let hideDocumentDirectory = NSSearchPathForDirectoriesInDomains(.applicationSupportDirectory, .userDomainMask, true).first!
static let hideDocumentDirectoryUrl = FileManager.default.urls(for: .applicationSupportDirectory, in: .userDomainMask).first!
```

위와 같이 ios11 부터 제공하는 files 애플리케이션을 이용하여,  
다른 앱과 파일 공유도 가능하며, 내가 다운받은 파일들을 바로 확인 할 수 있다. 