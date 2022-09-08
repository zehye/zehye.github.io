---
layout: post
title: iOS UICollectionView Cell 하나를 화면 전체에 채우고 싶을때는?
category: iOS
tags: [iOS]
comments: true
---

## UICollectionView Cell 하나를 화면 전체에 채우고 싶을때는?

```swift 
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
    let itemWidth = collectionView.bounds.width
    let itemHeight = collectionView.bounds.height
    return CGSize(width: itemWidth, height: itemHeight)
}
```