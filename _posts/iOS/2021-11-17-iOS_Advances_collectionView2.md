---
layout: post
title: Advances CollectionvView
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

이전 글에서는 CustomCell을 register해서 사용하는 방식을 보여드렸습니다. <br>
사실 커스텀셀을 사용하는 방법은 구글에 검색을 해봐도 잘 나오지 않더군요..

있어도 제 입맛에 맞게 수정하는것에 하나하나 제약사항도 너무 많았구요.

[CustomCell 로 보는 Advances CollectionView 포스팅 바로가기](https://www.zehye.kr/ios/2021/11/17/iOS_advances_collectionView/)

이번에는 애플에서 기본적으로 제공해주는 방식으로 셀을 구성해 데이터를 보여주는 컬렉션뷰를 한번 보여드리도록 하겠습니다.

코드는 애플에서 제공해주는 코드입니다.

[Apple Developer](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/implementing_modern_collection_views)


<br/>


## Advances CollectionvView

우선 모델 코드부터 보여드리겠습니다.

- Emoji 구조체는 Hashable 프로토콜을 채택
- 각 카테고리에 해당하는 케이스, 리턴값을 정의

```swift
import UIKit

struct Emoji: Hashable {

    enum Category: CaseIterable, CustomStringConvertible {
        case recents, smileys, nature, food, activities, travel, objects, symbols
    }

    let text: String
    let title: String
    let category: Category
    private let identifier = UUID()
}

extension Emoji.Category {

    var description: String {
        switch self {
        case .recents: return "Recents"
        case .smileys: return "Smileys"
        case .nature: return "Nature"
        case .food: return "Food"
        case .activities: return "Activities"
        case .travel: return "Travel"
        case .objects: return "Objects"
        case .symbols: return "Symbols"
        }
    }

    var emojis: [Emoji] {
        switch self {
        case .recents:
            return [
                Emoji(text: "🤣", title: "Rolling on the floor laughing", category: self),
                Emoji(text: "🥃", title: "Whiskey", category: self),
                Emoji(text: "😎", title: "Cool", category: self),
                Emoji(text: "🏔", title: "Mountains", category: self),
                Emoji(text: "⛺️", title: "Camping", category: self),
                Emoji(text: "⌚️", title: " Watch", category: self),
                Emoji(text: "💯", title: "Best", category: self),
                Emoji(text: "✅", title: "LGTM", category: self)
            ]

        case .smileys:
            return [
                Emoji(text: "😀", title: "Happy", category: self),
                Emoji(text: "😂", title: "Laughing", category: self),
                Emoji(text: "🤣", title: "Rolling on the floor laughing", category: self)
            ]

        case .nature:
            return [
                Emoji(text: "🦊", title: "Fox", category: self),
                Emoji(text: "🐝", title: "Bee", category: self),
                Emoji(text: "🐢", title: "Turtle", category: self)
            ]

        case .food:
            return [
                Emoji(text: "🥃", title: "Whiskey", category: self),
                Emoji(text: "🍎", title: "Apple", category: self),
                Emoji(text: "🍑", title: "Peach", category: self)
            ]
        case .activities:
            return [
                Emoji(text: "🏈", title: "Football", category: self),
                Emoji(text: "🚴‍♀️", title: "Cycling", category: self),
                Emoji(text: "🎤", title: "Singing", category: self)
            ]

        case .travel:
            return [
                Emoji(text: "🏔", title: "Mountains", category: self),
                Emoji(text: "⛺️", title: "Camping", category: self),
                Emoji(text: "🏖", title: "Beach", category: self)
            ]

        case .objects:
            return [
                Emoji(text: "🖥", title: "iMac", category: self),
                Emoji(text: "⌚️", title: " Watch", category: self),
                Emoji(text: "📱", title: "iPhone", category: self)
            ]

        case .symbols:
            return [
                Emoji(text: "❤️", title: "Love", category: self),
                Emoji(text: "☮️", title: "Peace", category: self),
                Emoji(text: "💯", title: "Best", category: self)
            ]

        }
    }
}
```


<br/>


모델은 단순합니다.

위에서 정리했던 것처럼 Emoji 구조체는 Hashable 프로토콜을 채택하고 있습니다.<br>
그 이유는 전에도 말씀드렸던 것처럼 Diffable DataSource는 넘어오는 데이터의 고유함을 반드시 지켜주어야하기 때문입니다.

그리고 아래는 Emoji.Category에 들어가는 각 케이스별 리턴값들을 정의해주고 있습니다.


<br/>



### Section과 Item

실제 우리가 보여줄 Section과 Item을 정의해봅니다.

```swift
import UIKit

class ViewController: UIViewController {
    enum Section: Int, Hashable, CaseIterable, CustomStringConvertible {  
        // 섹션에 들어갈 데이터 정의
        case recents, outline, list, custom

        var description: String {
            switch self {
            case .recents: return "Recents"
            case .outline: return "Outline"
            case .list: return "List"
            case .custom: return "Custom"
            }
        }
    }

    struct Item: Hashable {  
        // 섹션 속 아이템에 들어갈 데이터 정의
        let title: String?
        let emoji: Emoji?
        let hasChild: Bool

        init(emoji: Emoji? = nil, title: String? = nil, hasChild: Bool = false) {
            self.emoji = emoji
            self.title = title
            self.hasChild = hasChild
        }

        private let identifier = UUID()
    }
}
```

- caseIterable > 배열과 같이 순회 가능
- customStringConveertible > 사용자 정의에 따른 텍스트 출력 가능


<br/>


### UICollectionView 정의

```swift
var collectionView: UICollectionView!

func configureHierarchy() {
    collectionView = UICollectionView(frame: view.bounds, collectionViewLayout: createLayout())
    collectionView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
    collectionView.backgroundColor = .systemGroupedBackground
    view.addSubview(collectionView)
}
```

collectionView 객체를 만들어주고 실제 화면에 정의해주는 부분입니다. <br>
해당 컬렉션 뷰 위에 이제 우리가 보여주고싶은 데이터를 뿌려주게 됩니다.


<br/>


### Create Section Layout

이제 각 섹션들의 레이아웃을 정의해줍니다.

```swift
func createLayout() -> UICollectionViewLayout {
    let sectionProvider = { (sectionIndex: Int, layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
        guard let sectionKind = Section(rawValue: sectionIndex) else { return nil }
        let section: NSCollectionLayoutSection

        if sectionKind == .recents {
            let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
            let item = NSCollectionLayoutItem(layoutSize: itemSize)
            item.contentInsets = NSDirectionalEdgeInsets(top: 5, leading: 5, bottom: 5, trailing: 5)

            let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(0.28), heightDimension: .fractionalHeight(0.1))
            let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

            section = NSCollectionLayoutSection(group: group)
            section.interGroupSpacing = 0
            section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
            section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)

        } else if sectionKind == .outline {
            var configuration = UICollectionLayoutListConfiguration(appearance: .sidebar)
            configuration.leadingSwipeActionsConfigurationProvider = { [weak self] (indexPath) in
                guard let self = self else { return nil }
                guard let item = self.dataSource.itemIdentifier(for: indexPath) else { return nil }
                return self.leadingSwipeActionConfigurationForListCellItem(item)
            }
            section = NSCollectionLayoutSection.list(using: configuration, layoutEnvironment: layoutEnvironment)

        } else if sectionKind == .list {
            var configuration = UICollectionLayoutListConfiguration(appearance: .insetGrouped)
            configuration.leadingSwipeActionsConfigurationProvider = { [weak self] (indexPath) in
                guard let self = self else { return nil }
                guard let item = self.dataSource.itemIdentifier(for: indexPath) else { return nil }
                return self.leadingSwipeActionConfigurationForListCellItem(item)
            }
            section = NSCollectionLayoutSection.list(using: configuration, layoutEnvironment: layoutEnvironment)

        } else if sectionKind == .custom {
            section = NSCollectionLayoutSection.list(using: .init(appearance: .plain), layoutEnvironment: layoutEnvironment)
            section.contentInsets = NSDirectionalEdgeInsets(top: 100, leading: 10, bottom: 0, trailing: 10)

        } else {
            fatalError("Unknown section!")
        }
        return section
    }
    return UICollectionViewCompositionalLayout(sectionProvider: sectionProvider)
}
```

각 레이아웃의 환경은 애플에서 제공해주는 요소들로 정의되어져 있습니다.


<br/>


여기서 중요하게 보아야 할 개념은 sectionKind가 outline, list일때 입니다.

```swift
var configuration = UICollectionLayoutListConfiguration(appearance: .sidebar)
```

이때 등장하는 `UICollectionLayoutListConfiguration` 이건 무엇일까요?

iOS14에서는 UICollectionLayoutListConfiguration 라는 새로운 유형을 제공합니다.<br>
list configuration은 테이블뷰 스타일(.plain, .grouped, insetGrouped)과 같은 모양을 제공합니다.

또한 콜렉션 뷰 List 전용 .sideBar, .sideBarPlain 이라는 새로운 스타일 또한 제공함으로써 다중 열 앱을 구축할 수 있게 되었습니다.


따라서 위와 같은 configuration들을 지정해줌으로써 컬렉션뷰에서도 다양한 리스트 형태의 레이아웃을 만들어 낼 수 있게 된것입니다.


그게 바로 지금 저희가 만들 앱 화면의 모습이기도 합니다.


<br/>


### Create Cell and Resister

이제 컬렉션 뷰 내 보여질 섹션과 아이템들의 레이아웃 구성을 하였으니 본격적으로 셀을 만들어주고 지정해보도록 합니다.

```swift
// recent > grid cell registration
func createGridCellRegistration() -> UICollectionView.CellRegistration<UICollectionViewCell, Emoji> {
    // 셀 등록 > iOS14부터는 cell registration을 통해 새롭게 cell을 구성할 수 있음
    return UICollectionView.CellRegistration<UICollectionViewCell, Emoji> { (cell, indexPath, emoji) in
        // 테이블 뷰와 같이 셀에 대한 표준화된 레이아웃을 제공
        var content = UIListContentConfiguration.cell()
        content.text = emoji.text
        content.textProperties.font = .boldSystemFont(ofSize: 38)
        content.textProperties.alignment = .center
        content.directionalLayoutMargins = .zero
        cell.contentConfiguration = content
        var background = UIBackgroundConfiguration.listPlainCell()
        background.cornerRadius = 8
        background.strokeColor = .systemGray3
        background.strokeWidth = 1.0 / cell.traitCollection.displayScale
        cell.backgroundConfiguration = background
    }
}

// outline header cell registration
func createOutlineHeaderCellRegistration() -> UICollectionView.CellRegistration<UICollectionViewListCell, String> {
    return UICollectionView.CellRegistration<UICollectionViewListCell, String> { (cell, indexPath, title) in
        var content = cell.defaultContentConfiguration()
        content.text = title
        cell.contentConfiguration = content
    }
}

// outline cell registration
func createOutlineCellRegistration() -> UICollectionView.CellRegistration<UICollectionViewListCell, Emoji> {
    return UICollectionView.CellRegistration<UICollectionViewListCell, Emoji> { (cell, indexPath, emoji) in
        var content = cell.defaultContentConfiguration()
        content.text = emoji.text
        content.secondaryText = emoji.title
        cell.contentConfiguration = content
    }
}

// list > list cell registration
func createListCellRegistration() -> UICollectionView.CellRegistration<UICollectionViewListCell, Item> {
    return UICollectionView.CellRegistration<UICollectionViewListCell, Item> { [weak self] (cell, indexPath, item) in
        guard let self = self, let emoji = item.emoji else { return }
        var content = UIListContentConfiguration.valueCell()
        content.text = emoji.text
        content.secondaryText = String(describing: emoji.category)
        cell.contentConfiguration = content
    }
}
```

이때 또 새로운 개념이 보이죠?

1. UIListContentConfiguration
2. defaultContentConfiguration

UIListContentConfiguration은 list based content view에 대한 content configuration을 의미합니다.

defaultContentConfiguration은 우리가 기본적으로 테이블뷰 혹은 컬렉션뷰를 사용할때 각 셀안에 이미 존재하는 textLabel, imageView등을 사용하였습니다.
근데 이제 이러한 접근이 iOS14에서부터는 deprecated 되어 접근이 불가능해졌습니다.

그래서 이때 셀에 각 데이터에 접근하기 위해 `defaultContentConfiguration`에 접근해야합니다.

그리고 이제 셀을 등록하기 위해 `UICollectionView.CellRegistration`를 통해 등록하는 것을 볼 수 있습니다.


<br/>


### Diffable DataSource

이제 이렇게 만들어 놓은 셀에 대한 데이터 작업을 해봅니다.

```swift
var dataSource: UICollectionViewDiffableDataSource<Section, Item>!


func configureDataSource() {
    // create registrations up front, then choose the appropriate one to use in the cell provider
    let gridCellRegistration = createGridCellRegistration()
    let listCellRegistration = createListCellRegistration()
    let outlineHeaderCellRegistration = createOutlineHeaderCellRegistration()
    let outlineCellRegistration = createOutlineCellRegistration()
    let createPlaceRegistration = createPlainCellRegistration()

    // data source
    dataSource = UICollectionViewDiffableDataSource<Section, Item>(collectionView: collectionView) {
        (collectionView, indexPath, item) -> UICollectionViewCell? in
        guard let section = Section(rawValue: indexPath.section) else { fatalError("Unknown section") }
        switch section {
        // recent
        case .recents:
            return collectionView.dequeueConfiguredReusableCell(using: gridCellRegistration, for: indexPath, item: item.emoji)
        // 맨 아래 list
        case .list:
            return collectionView.dequeueConfiguredReusableCell(using: listCellRegistration, for: indexPath, item: item)
        // outline > header, cell
        case .outline:
            if item.hasChild {
                return collectionView.dequeueConfiguredReusableCell(using: outlineHeaderCellRegistration, for: indexPath, item: item.title!)
            } else {
                return collectionView.dequeueConfiguredReusableCell(using: outlineCellRegistration, for: indexPath, item: item.emoji)
            }

        case .custom:
            return collectionView.dequeueConfiguredReusableCell(using: createPlaceRegistration, for: indexPath, item: item)

        }
    }
}
```


<br/>


### Snapshot

만들어놓은 데이터소스에 스냅샷을 적용하는 코드입니다.

```swift
// 스냅샷 적용
    // NSDiffableDataSourceSnapshot > 데이터 접근, 특정 인덱스에 데이터 삽입 및 삭제 가능 > apply 통해 변경사항 적용
    func applyInitialSnapshots() {
        let sections = Section.allCases
        var snapshot = NSDiffableDataSourceSnapshot<Section, Item>()
        snapshot.appendSections(sections)
        dataSource.apply(snapshot, animatingDifferences: false)

        // recents
        let recentItems = Emoji.Category.recents.emojis.map { Item(emoji: $0) }
        var recentsSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        recentsSnapshot.append(recentItems)

        // list + outlines
        var allSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        var outlineSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()

        for category in Emoji.Category.allCases where category != .recents {
            // append to the "all items" snapshot
            let allSnapshotItems = category.emojis.map { Item(emoji: $0) }
            allSnapshot.append(allSnapshotItems)

            // setup our parent/child relations
            let rootItem = Item(title: String(describing: category), hasChild: true)
            outlineSnapshot.append([rootItem])
            let outlineItems = category.emojis.map { Item(emoji: $0) }
            outlineSnapshot.append(outlineItems, to: rootItem)
        }

        let customItems = Emoji.Category.recents.emojis.map { Item(emoji: $0) }
        var cusomSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        cusomSnapshot.append(customItems)

        dataSource.apply(recentsSnapshot, to: .recents, animatingDifferences: false)
        dataSource.apply(allSnapshot, to: .list, animatingDifferences: false)
        dataSource.apply(outlineSnapshot, to: .outline, animatingDifferences: false)
        dataSource.apply(cusomSnapshot, to: .list, animatingDifferences: false)
    }
}
```
