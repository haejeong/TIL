# QLPreviewController

> 아이템을 미리보기할 수 있는 특별한 뷰 

## Declaration

```swift
class QLPreviewController : UIViewController
```
## Overview

미리보기에는 항목 URL의 마지막 경로 요소에서 가져온 제목이 포함되어 있다. 미리보기 항목에 대해 미리보기 `previewItemTitle` 액세스자를 구현하여 이 제목을 재정의할 수 있다.

`QLPreviewController`는 아래와 같은 항목들을 미리보기 할 수 있다. 

- iWork documents
- Microsoft Office documents (Office ‘97 and newer)
- Rich Text Format (RTF) documents
- PDF files
- Images
- Text files whose uniform type identifier (UTI) conforms to the public.text type (see Uniform Type Identifiers Reference)
- Comma-separated value (csv) files
- 3D models in USDZ format (with both standalone and AR views for viewing the model)

`QLPreviewController` 는 현재 뷰컨틀로러에서 `present(_:animated:completion:)` 를 호출하여 화면 전체를 덮는 모달로 띄울 수 있다. 또는 `UINavigationController`를 이용하여 푸시할 수 있다. 

Quick Look 미리 보기 컨트롤러를 사용하려면 데이터 원본 개체를 제공해야 한다. 데이터 원본은 컨트롤러에 미리 보기 항목을 제공하고 미리 보기 탐색 목록에 포함할 항목 수를 알려준다. 목록에 항목이 두 개 이상 있는 경우 사용자가 항목 간에 전환할 수 있도록 모듈식 컨트롤러에서 제공하는 탐색 화살표를 표시한다. 탐색 컨트롤러를 사용하여 푸시된 `Quick Look preview controller`의 경우 탐색 모음에서 단추를 제공하여 탐색 목록을 이동할 수 있다. 

미리보기 컨트롤러에서 제공하는 방법에 대한 자세한 내용은 `QLPreviewControllerDataSource`와 `QLPreviewItem`를 참고하자. 



## Topics 

### Configuring a Quick Look Preview Controller

#### dataSource

> The Quick Look preview controller's data source.

##### Declaration 

````swift
weak var dataSource: QLPreviewControllerDataSource? { get set }
````

##### Discussion

Quick Look preview controller 를 사용하기 위해선 datasource 를 구현해야한다. datasource 는 controller 에 표시할 아이템들을 제공하고, 미리보기 탐색 목록에 포함할 항목 수를 알려준다. 설명은 `QLPreviewControllerDataSource`에 있다. 
 
###### `numberOfPreviewItems(in:)`
> `Quick Look preview controller`에서 미리보기 탐색을 할 항목수를 알아야 할 때 사용함.  

###### `previewController(_:previewItemAt:)`
> 특정 index 위치에 나타내는 아이템을 알아낼 때 사용한다. 

#### delegate

> The Quick Look preview controller’s delegate object.

##### Declaration

```swift
weak var delegate: QLPreviewControllerDelegate? { get set }
```

##### Overview
`QLPreviewController`의 delegate 는 이 프로토콜을 사용하여 적용해야 한다. 

- Quick Look 미리보기를 위해 확대/축소 애니메이션을 제공한다. 
- 사용자가 미리보기에서 클릭하여 url 을 열 것인가의 여부를 정한다. 
- 미리보기를 열고, 닫는 것에 응답한다. 

위에 설명한 방법은 선택사항이지만 필요하다. 

###### `previewController(_:frameFor:inSourceView:)`
> Quick Look 미리보기를 호출 할 때 전체를 덮는 화면으로 표시하거나 닫을 때 zoom 효과를 제공한다. 

```swift
optional func previewController(_ controller: QLPreviewController, 
                       frameFor item: QLPreviewItem, 
                   inSourceView view: AutoreleasingUnsafeMutablePointer<UIView?>) -> CGRect
```

어플리케이션에 표시되는 미리 보기 항목의 프레임을 정의하는 CGRect object. 

###### `previewControllerWillDismiss(_:)`
> 미리보기 컨트롤러가 닫치기 전에 불린다.

```swift
optional func previewControllerWillDismiss(_ controller: QLPreviewController)
```

###### `previewControllerDidDismiss(_:)`
> 미리보기 컨트롤러가 닫히고 나서 불린다.

```swift
optional func previewControllerDidDismiss(_ controller: QLPreviewController)
```

###### `previewController(_:shouldOpen:for:)`

> Quick Look preview controller 에서 url 을 열기 전에 호출된다. 


```swift
optional func previewController(_ controller: QLPreviewController, 
                     shouldOpen url: URL, 
                            for item: QLPreviewItem) -> Bool
```

#### Managing Item Previews

###### `canPreview(_:)`
> Quick Look preview controller 가 항목을 표시할 수 있는지 여부를 알려준다.

```swift
class func canPreview(_ item: QLPreviewItem) -> Bool
```

###### `currentPreviewItem`

> Quick Look preview controller 에 현재 표시된 항목 리턴

```swift
var currentPreviewItem: QLPreviewItem? { get }
```

###### `refreshCurrentPreviewItem()`

> Quick Look preview controller를 눌러 현재 미리보기 항목의 표시를 다시 계산한다. 

```swift
func refreshCurrentPreviewItem()
```

###### `reloadData()`

> datasource 를 reload 한다. 


