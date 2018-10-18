# Frameworks

## Photos

Assets 을 가져올 때 사전에 캐싱처리 하는게 필요할 때가 있다.  
`PHCachingImageManager`를 이용하는데, `PHCachingImageManager`는 `PHImageManager`의 하위 클래스이다.

```swift
PHCachingImageManager().startCachingImages(for: assets, targetSize: thumbnailSize, contentMode: .aspectFill, options: nil)
```
와 같은 방법으로 캐싱하게되는데 targetSize, ContentMode, Options 이 일치해야한다. 


### Display UICollectionCell with PHAsset

앨범의 사진들을 불러와 뿌려줄 때 cell 을 재사용하면서 다시 비워주고 컨텐츠를 비워줌에도 불구하고,  
다른 사진들의 썸네일이 indexPath 에 노출 될 경우가 있다. 

그럴 때 PHAsset 의 `localIdentifier` 를 사용한다.

```swift
cell.representedAssetIdentifier = asset.localIdentifier
```
해당 셀에 위와 같이 asset 의 localIdentifier 를 저장시켜주고, 

이미지 데이터를 요청 할 때 해당 셀에 미리 저장시켰던 localIdentifier 와 비교하여  
같을 경우에만 이미지를 노출 시킬 수 있도록 해준다.

```swift
imageManager.requestImage(for: asset, targetSize: thumbnailSize, contentMode: .aspectFill, options: nil, resultHandler: { image, _ in
    
    if cell.representedAssetIdentifier == asset.localIdentifier {
        cell.thumbnailImage = image
    }
})
```

### PHImageRequestOptions

`startCachingImages`을 하거나 `requestImage` 를 호출할 때 options 을 넘기는 부분이 있다. 

>deliveryMode: PHImageRequestOptionsDeliveryMode 

이미지의 화질보다 이미지를 불러오는 속도에 우선순위를 두거나,  
이미지를 불러오는 속도보다는 고화질의 이미지를 얻을 때 또는 자동으로 알아서 불러 올 수 있도록 설정할 수 있다. 

>resizeMode: PHImageRequestOptionsResizeMode

이미지를 요청할 때의 이미지 사이즈를 요청한 타겟 사이즈에 어떻게 맞출 것인지 설정하는 부분이다.

>isNetworkAccessAllowed: Bool

만약 true 로 설정했을 경우, 로컬에 이미지가 없을 경우 iCloud에서 불러 올 수 있다. 


