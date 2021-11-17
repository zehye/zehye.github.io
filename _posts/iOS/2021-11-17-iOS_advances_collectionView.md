---
layout: post
title: Advances CollectionvView - Custom Cell 로 구성해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 개요

일반적으로 컬렉션뷰는 dataSource를 통해 셀에 표시할 정보를 가져옵니다.

뷰 컨트롤러에서 dataSource 프로토콜을 채택하게되면 기본적으로 실행해야할 메소드들이 있고, 이를 통해 우리는 셀에 대한 데이터를 가져와 화면에 보여줄 수 있게 됩니다.
그런데 WWDC19부터 이번에 발표했던 WWDC21까지 dataSource를 넘어서 cell에 대한 새로운 기능, 더 확장되고 심화된 개념의 컬렉션뷰가 등장하였습니다.

애플에서도 이를 `Advances CollectionView`라 칭하면서 그 안에 다양한 기능들을 소개하고 있습니다.

이번 포스팅에서 저는 이 Advances CollectionView에서 소개하는 크게 총 2가지의 기능을 사용해 프로젝트를 구성해보려고 합니다.

1. Diffable DataSource / Section Snapshot
2. UICollectionViewCompositionalLayout

만들어볼 화면 구성은 다음과 같습니다.

1. CollectionView를 통해 json 파일 내 데이터를 보여준다.
2. CollectionView 내 데이터 구성은 총 3개의 Section으로 나누어 보여준다.
3. 해당 데이터는 DiffableDataSource를 통해 Cell에 나타내도록 한다.
4. 각 셀은 커스텀 셀로 구성되어진다.

+ 추가적으로 확인해 볼 내용은 다음과 같습니다.

1. 셀이 무한 스크롤 되는 경우 dataSource와 layout이 어떻게 호출되는 지 확인
2. 섹션 사이에 다른 섹션이 들어갈 수 있는지 확인


1~4까지는 기본적으로 만들어볼 화면이며, 추가 1,2는 글 하단에 추가적으로 다뤄볼 예정입니다.


<br/>


## Advances CollectionView - CustomCell로 구성해보기

우선 프로젝트에 들어가기 앞서 저는 json 파일을 제 프로젝트 내에 넣어놓고 해당 파일을 불러와 데이터를 가져올 것입니다.

json 파일은 아래와 같습니다. (Home.json)

```json
{
    "companyInfoList" : [
        {
            "companyName" : "닥프렌즈 병원",
            "addrRoad" : "서울 특별시",
            "introPath" : ""
        },
    ],
    "expertList" : [
        {
            "expertInfo" : {
                "expertTypeName" : "한의사",
                "name" : "박지혜",
                "profilePath": ""
            }
        }
    ],
    "consultList" : [
        {
            "regDate" : 1635472394000,
            "readCnt" : 22,
            "title" : "감기에 걸렸을때 어느 병원으로 가야할까요?",
            "lastAnswer" : {
                "content" : "안녕하세요, 닥톡-네이버 지식iN 상담한의사 박지혜입니다.\n",
                "profileImg" : "",
            }
        }
    ]
}
```

해당 데이터를 불러오기 위해 모델을 구성해보겠습니다.<br>
json 파일 데이터를 불러오기 위해 `swiftyJSON`을 사용하였습니다.



<br/>



#### Model Company

```swift
import UIKit
import SwiftyJSON

struct Company {
    var addrRoad: String?
    var companyName: String?
    var introPath: UIImage? = nil

    init(_ json: [String: JSON]?) {
        let json = json ?? [:]
        self.addrRoad = json["addrRoad"]?.string ?? ""
        self.companyName = json["companyName"]?.string ?? ""
        if let url = URL(string: json["introPath"]?.stringValue ?? ""),
           let image = try? Data(contentsOf: url) {
            self.introPath = UIImage(data: image)
        }
    }

    init(_ json: JSON?) {
        self.init(json?.dictionaryValue)
    }
}

extension Company: Hashable {
    func hash(into hasher: inout Hasher) {
        hasher.combine(addrRoad)
        hasher.combine(companyName)
        hasher.combine(introPath)
    }
}
```



<br/>



#### Model Expert

```swift
import UIKit
import SwiftyJSON

struct Expert {
    var expertTypeName: String?
    var name: String?
    var profilePath: UIImage? = nil

    init(_ json: [String:JSON]?) {
        let json = json ?? [:]
        self.expertTypeName = json["expertInfo"]?["expertTypeName"].string ?? ""
        self.name = json["expertInfo"]?["name"].string ?? ""
        if let url = URL(string: json["expertInfo"]?["profilePath"].stringValue ?? ""),
           let image = try? Data(contentsOf: url) {
            self.profilePath = UIImage(data: image)
        }
    }

    init(_ json: JSON?) {
        self.init(json?.dictionaryValue)
    }
}

extension Expert: Hashable {
    func hash(into hasher: inout Hasher) {
        hasher.combine(expertTypeName)
        hasher.combine(name)
        hasher.combine(profilePath)
    }
}
```



<br/>



#### Model Consult

```swift
import UIKit
import SwiftyJSON

struct Consult {
    let title: String?
    let readCnt: Int?
    let content: String?
    var regDate: Date?
    var profileImg: UIImage? = nil

    init(_ json: [String:JSON]?) {
        let json = json ?? [:]
        self.title = json["title"]?.string ?? ""
        self.readCnt = json["readCnt"]?.int ?? 0
        self.content = json["lastAnswer"]?["content"].string ?? ""
        if let url = URL(string: json["lastAnswer"]?["profileImg"].stringValue ?? ""),
           let image = try? Data(contentsOf: url) {
            self.profileImg = UIImage(data: image)
        }
        self.regDate = Date(timeIntervalSince1970: (json["regDate"]!.doubleValue/1000))
    }

    init(_ json: JSON?) {
        self.init(json?.dictionaryValue)
    }
}

extension Consult: Hashable {
    func hash(into hasher: inout Hasher) {
        hasher.combine(title)
        hasher.combine(readCnt)
        hasher.combine(content)
        hasher.combine(profileImg)
        hasher.combine(regDate)
    }
}
```

이렇게 모델을 구성하고 나면 실제 json 파일을 해당 모델에 맞게 불어와야겠죠.


<br/>



#### ViewController

```swift
import UIKit
import SwiftyJSON

class ViewController: UIViewController {
    var companyList = Array<Company>()
    var consultList = Array<Consult>()
    var expertList = Array<Expert>()

    func setData() {
        if let path = Bundle.main.path(forResource: "Home", ofType: "json"), let data = try? Data(contentsOf: URL(fileURLWithPath: path)) { print("path : \(path)")
            do {
                let jsonResult = try JSON(data: data)

                let companyResult = jsonResult["companyInfoList"]
                self.companyList = companyResult.arrayValue.compactMap({Company($0)})

                let consultResult = jsonResult["consultList"]
                self.consultList = consultResult.arrayValue.compactMap({Consult($0)})

                let expertResult = jsonResult["expertList"]
                self.expertList = expertResult.arrayValue.compactMap({Expert($0)})

            }catch {
                print("error : \(error)")
            }
        }
    }
}
```

이렇게 하면 프로젝트 내 있는 jons파일을 불러와 제가 원하는 데이터를 가지고 있을 수 있게 됩니다. <br>
이때 각 모델에서 Hashable 프로토콜을 채택한 이유는 무엇일까요?

우리는 앞으로 Diffable DataSource를 사용할 것입니다.

이때 해당 데이터들은 반드시 각각 본인이 고유한 데이터임을 증명해주어야 합니다. 그렇기 때문에 이렇게 Hashable을 통해 진행을 하게 되는데요. <br>
사실상 더 많은 데이터를 다루기에는 위 같은 방법은 옳지 않습니다.

프로젝트 진행시 넘어오는 json 안에 `seq` 혹은 `uid` 같이 반드시 고유할것이라는 증명된 변수가 있다면<br>
이 변수를 고유하게 지정해주면 됩니다.


<br/>


### Section과 Item

저는 우선 화면 구성을 크게 3개의 section으로 구분지어 표현할 예정입니다.

1. Consult Section
2. Company Section
3. Expert Section

그리고 각 섹션에는 Json 파일로부터 긁어온 데이터들을 아이템으로 넣어줄 것입니다.<br>
이것을 코드로 표현해보겠습니다.


<br/>


```swift
class ViewController: UIViewController {

    enum Section: Int, Hashable, CaseIterable, CustomStringConvertible {
        case consult, company, expert

        var description: String {
            switch self {
            case .consult: return "Consult"
            case .company: return "Company"
            case .expert: return "Expert"
            }
        }
    }

    struct Item: Hashable {
        let expert: Expert?
        let consult: Consult?
        let company: Company?

        init(consult: Consult? = nil, company: Company? = nil, expert: Expert? = nil) {
            self.expert = expert
            self.consult = consult
            self.company = company
        }

        private let identifier = UUID()
    }
}
```


<br/>


### Section과 Item들의 레이아웃 설정

각 섹션과 아이템들의 위치 즉 레이아웃을 잡아주기 위해 우리는 UICollectionViewCompositionalLayout을 사용할 것입니다.

이 개념을 이해하기 위한 사진 하나를 보여드리겠습니다.


<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/26.png" alt="" width="50%">
</figure>
</center>

해당 사진에서 유추할 수 있듯 각 레이아웃을 잡아주는 순서는 다음과 같습니다.

1. item에서 group
2. group에서 section
3. 이렇게 결합한 뒤 전체 layout으로 구성요소 결합

각 섹션에 대한 코드는 다음과 같습니다.


<br/>


```swift
func createLayout() -> UICollectionViewLayout {
    print("createLayout")
    let sectionProvider = { (sectionIndex: Int, layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
        guard let sectionKind = Section(rawValue: sectionIndex) else { return nil }
        let section: NSCollectionLayoutSection
        if sectionKind == .consult{
            print("layout consult")
            // item의 width와 height 지정
            let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
            let item = NSCollectionLayoutItem(layoutSize: itemSize)
            item.contentInsets = NSDirectionalEdgeInsets(top: 2, leading: 2, bottom: 2, trailing: 2)

            let groupHeight = NSCollectionLayoutDimension.absolute(200)
            let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: groupHeight)
            // 그룹 내 1.0 배율 안에서 보여질 아이템의 수는 1개라는 의미
            let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitem: item, count: 1)

            section = NSCollectionLayoutSection(group: group)

        } else if sectionKind == .company {
            print("layout company")
            let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
            let item = NSCollectionLayoutItem(layoutSize: itemSize)

            let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(350), heightDimension: .absolute(200))
            let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

            section = NSCollectionLayoutSection(group: group)
            section.interGroupSpacing = 10
            section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
            section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)

        } else if sectionKind == .expert {
            print("layout expert")
            let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
            let item = NSCollectionLayoutItem(layoutSize: itemSize)

            let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(150), heightDimension: .absolute(150))
            let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

            section = NSCollectionLayoutSection(group: group)
            section.interGroupSpacing = 10
            section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
            section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)
        }
        else {
            fatalError("Unknown section!")
        }
        return section
    }
    return UICollectionViewCompositionalLayout(sectionProvider: sectionProvider)
}
```

코드를 살펴보면 Consult Section의 경우 수직으로 스크롤이 되고 있으며, 나머지 Compant, Expert Section은 수평으로 스크롤이 되고있음을 알 수 있습니다.

이를 구분짓는 코드는 `section.orthogonalScrollingBehavior`입니다.<br>
하나의 섹션 단위의 스크롤 방향을 지정해주는 것으로 이 행위를 넣지 않으면 디폴트는 수직입니다. <br>
그래서 데이터들이 아래로 쌓여져 보이게 될 것입니다.

추가로 코드 안에서 `NSCollectionLayoutSize`를 통해 item과 group의 사이즈(width, height)를 구성하고 있음을 볼 수 있습니다.

- absolute: 절대값, 정확한 치수를 지정할 때 사용
- estimated: 런타임에 컨텐츠의 크기가 변경될 수 있는 경우 예상값을 사용
- fractional: 분구삾을 통해 item 컨테이너의 너비 기준으로 상대적인 값 정의


<br/>


### Custom Cell

저는 커스텀셀을 만들어와 커스텀 셀을 연결해줄 예정입니다.

커스텀 셀은 아래와 같이 정의하였습니다.

```swift
import UIKit

class CompanyCollectionViewCell: UICollectionViewCell {
    private let profielImgView: UIImageView = {
        let imgView = UIImageView()
        imgView.contentMode = .scaleAspectFill
        imgView.clipsToBounds = true
        imgView.backgroundColor = .clear
        return imgView
    }()

    private let companyLbl: UILabel = {
        let lbl = UILabel()
        lbl.font = .systemFont(ofSize: 16)
        lbl.textAlignment = .center
        return lbl
    }()

    private let addressLbl: UILabel = {
        let lbl = UILabel()
        lbl.font = .systemFont(ofSize: 14)
        lbl.textAlignment = .center
        return lbl
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)
        self.initView()
    }

    required init?(coder: NSCoder) {
        fatalError("init")
    }

    func initView() {
        self.contentView.backgroundColor = .clear

        self.contentView.addSubview(profielImgView)
        self.contentView.addSubview(companyLbl)
        self.contentView.addSubview(addressLbl)

        self.profielImgView.snp.makeConstraints {(make) in
            make.trailing.equalToSuperview().inset(12)
            make.leading.equalToSuperview()
            make.height.equalTo(140)
            make.top.equalToSuperview()
        }

        self.companyLbl.snp.makeConstraints{(make) in
            make.top.equalTo(self.profielImgView.snp.bottom).inset(-8)
            make.trailing.leading.equalToSuperview().inset(24)
        }

        self.addressLbl.snp.makeConstraints {(make) in
            make.top.equalTo(self.companyLbl.snp.bottom).inset(-8)
            make.leading.trailing.equalTo(self.companyLbl)
            make.bottom.lessThanOrEqualToSuperview().inset(8)
        }
    }

    func configure(_ data: Company) {
        self.profielImgView.image = data.introPath
        self.companyLbl.text = data.companyName
        self.addressLbl.text = data.addrRoad
    }
}
```

Cell 파일에서는 단순히 셀에 들어가는 요소들의 객체를 만들어주고 각각의 위치를 잡아주고 있습니다.<br>
특징이라면 configure 메소드에서 모델의 데이터들을 각각 연결해주고 있다는 것이죠.

이렇게 만들어놓은 셀 파일을 이제 뷰컨트롤러에서 등록해주고 연결할 것입니다.


<br/>


```swift
func createCell() {
    print("createCell")
    self.collectionView.register(CompanyCollectionViewCell.self, forCellWithReuseIdentifier: "CompanyCell")
    self.collectionView.register(ConsultCollectionViewCell.self, forCellWithReuseIdentifier: "ConsultCell")
    self.collectionView.register(ExpertCollectionViewCell.self, forCellWithReuseIdentifier: "ExpertCell")
}

func createDataSource() {
    print("createDataSource")
        self.dataSource = UICollectionViewDiffableDataSource<Section, Item>(collectionView: self.collectionView) {(collectionView, indexPath, item) -> UICollectionViewCell? in
        guard let section = Section(rawValue: indexPath.section) else { fatalError() }
        switch section {
        case .company:
            guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CompanyCell", for: indexPath) as? CompanyCollectionViewCell else { preconditionFailure() }
            cell.configure(item.company!)
            return cell
        case .consult:
            print("datasource consult")
            guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ConsultCell", for: indexPath) as? ConsultCollectionViewCell else { preconditionFailure() }
            cell.configure(item.consult!)
            return cell
        case .expert:
            print("datasource expert")
            guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ExpertCell", for: indexPath) as? ExpertCollectionViewCell else { preconditionFailure() }
            cell.configure(item.expert!)
            return cell
        }
    }
}
```

위 코드는 우리가 일반적으로 컬렉션뷰 데이터소스 프로토콜을 채택했을 때 사용하는 `CellForItem` 메소드 내 구성과 비슷한것을 볼수 있죠?


<br/>



### Snapshot

이렇게 구성한 데이터소소를 가지고 이제 snapshot을 구성해보도록 하겠습니다.

```swift
func applySnapshot() {
    print("appleSnapshot")
    let sections = Section.allCases
    var snapshot = NSDiffableDataSourceSnapshot<Section, Item>()
    snapshot.appendSections(sections)
    print(sections)
    print(snapshot)

    let consultItems = self.consultLis.map { Item(consult: $0) }
    var consultSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
    consultSnapshot.append(consultItems)

    let companyItems = self.companyList.map { Item(company: $0) }
    companySnapshot.append(companyItems)


    let expertItems = self.expertList.map { Item(expert: $0) }
    var expertSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
    expertSnapshot.append(expertItems)

    dataSource.apply(consultSnapshot, to: .consult, animatingDifferences: false)
    dataSource.apply(companySnapshot, to: .company, animatingDifferences: false)
    dataSource.apply(expertSnapshot, to: .expert, animatingDifferences: false)
}
```

이렇게 적용을 하면 제가 원하는 대로 실행이 될 것입니다.


<br/>


아래는 전체 소스코드 입니다.

```swift
import UIKit
import SwiftyJSON

class ViewController: UIViewController {

    enum Section: Int, Hashable, CaseIterable, CustomStringConvertible {
        case consult, company, expert

        var description: String {
            switch self {
            case .consult: return "Consult"
            case .company: return "Company"
            case .expert: return "Expert"
            }
        }
    }

    struct Item: Hashable {
        let expert: Expert?
        let consult: Consult?
        let company: Company?

        init(consult: Consult? = nil, company: Company? = nil, expert: Expert? = nil) {
            self.expert = expert
            self.consult = consult
            self.company = company
        }

        private let identifier = UUID()
    }

    var dataSource: UICollectionViewDiffableDataSource<Section, Item>!

    @IBOutlet weak var collectionView: UICollectionView!
    var companyList = Array<Company>()
    var consultList = Array<Consult>()
    var expertList = Array<Expert>()

    override func viewDidLoad() {
        overrideUserInterfaceStyle = .light
        super.viewDidLoad()

        setData()
        setNavigation()
        createDataSource()
        self.collectionView.collectionViewLayout = self.createLayout()
        createCell()
        applySnapshot()
    }

    func setData() {
        print("setData")
        if let path = Bundle.main.path(forResource: "Home", ofType: "json"), let data = try? Data(contentsOf: URL(fileURLWithPath: path)) { print("path : \(path)")
            do {
                let jsonResult = try JSON(data: data)

                let companyResult = jsonResult["companyInfoList"]
                self.companyList = companyResult.arrayValue.compactMap({Company($0)})

                let consultResult = jsonResult["consultList"]
                self.consultList = consultResult.arrayValue.compactMap({Consult($0)})

                let expertResult = jsonResult["expertList"]
                self.expertList = expertResult.arrayValue.compactMap({Expert($0)})

            }catch {
                print("error : \(error)")
            }
        }
    }

    func createLayout() -> UICollectionViewLayout {
        print("createLayout")
        let sectionProvider = { (sectionIndex: Int, layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
            guard let sectionKind = Section(rawValue: sectionIndex) else { return nil }
            let section: NSCollectionLayoutSection
            if sectionKind == .consult{
                print("layout consult")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)
                item.contentInsets = NSDirectionalEdgeInsets(top: 2, leading: 2, bottom: 2, trailing: 2)

                let groupHeight = NSCollectionLayoutDimension.absolute(200)
                let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: groupHeight)
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitem: item, count: 1)

                section = NSCollectionLayoutSection(group: group)

            } else if sectionKind == .company {
                print("layout company")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)

                let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(350), heightDimension: .absolute(200))
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

                section = NSCollectionLayoutSection(group: group)
                section.interGroupSpacing = 10
                section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
                section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)

            } else if sectionKind == .expert {
                print("layout expert")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)

                let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(150), heightDimension: .absolute(150))
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

                section = NSCollectionLayoutSection(group: group)
                section.interGroupSpacing = 10
                section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
                section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)
            }
            else {
                fatalError("Unknown section!")
            }
            return section
        }
        return UICollectionViewCompositionalLayout(sectionProvider: sectionProvider)
    }

    func createCell() {
        print("createCell")
        self.collectionView.register(CompanyCollectionViewCell.self, forCellWithReuseIdentifier: "CompanyCell")
        self.collectionView.register(ConsultCollectionViewCell.self, forCellWithReuseIdentifier: "ConsultCell")
        self.collectionView.register(ExpertCollectionViewCell.self, forCellWithReuseIdentifier: "ExpertCell")
    }

    func createDataSource() {
        print("createDataSource")
            self.dataSource = UICollectionViewDiffableDataSource<Section, Item>(collectionView: self.collectionView) {(collectionView, indexPath, item) -> UICollectionViewCell? in
            guard let section = Section(rawValue: indexPath.section) else { fatalError() }
            switch section {
            case .company:
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CompanyCell", for: indexPath) as? CompanyCollectionViewCell else { preconditionFailure() }
                cell.configure(item.company!)
                return cell
            case .consult1, .consult2:
                print("datasource consult")
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ConsultCell", for: indexPath) as? ConsultCollectionViewCell else { preconditionFailure() }
                cell.configure(item.consult!)
                return cell
            case .expert:
                print("datasource expert")
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ExpertCell", for: indexPath) as? ExpertCollectionViewCell else { preconditionFailure() }
                cell.configure(item.expert!)
                return cell
            }
        }
    }

    func setNavigation() {
        navigationItem.title = "홈"    
    }

    func applySnapshot() {
        print("appleSnapshot")
        let sections = Section.allCases
        var snapshot = NSDiffableDataSourceSnapshot<Section, Item>()
        snapshot.appendSections(sections)
        print(sections)
        print(snapshot)

        let consultItems = self.consultList.map { Item(consult: $0) }
        var consultSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        consultSnapshot.append(consultItems)

        let companyItems = self.companyList.map { Item(company: $0) }
        companySnapshot.append(companyItems)

        let expertItems = self.expertList.map { Item(expert: $0) }
        var expertSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        expertSnapshot.append(expertItems)

        dataSource.apply(consultSnapshot, to: .consult, animatingDifferences: false)
        dataSource.apply(companySnapshot, to: .company, animatingDifferences: false)
        dataSource.apply(expertSnapshot, to: .expert, animatingDifferences: false)
    }
}
```


<br/>


## 추가적으로 알아본 것

1. 셀이 무한 스크롤 되는 경우 dataSource와 layout이 어떻게 호출되는 지 확인
2. 섹션 사이에 다른 섹션이 들어갈 수 있는지 확인


<br/>


### 1. 셀이 무한 스크롤 되는 경우 dataSource와 layout이 어떻게 호출되는 지 확인

결과적으로 해당 레이아웃과 데이터소스를 불러오는 메소드가 계속해서 호출되는 것을 볼 수 있었습니다.

하단에 아래와 같은 코드를 추가해보았습니다.

```swift
func appendCompany() {
    print("appendCompany")
    let companyItems = self.companyList.map { Item(company: $0) }
    companySnapshot.append(companyItems)
    dataSource.apply(companySnapshot, to: .company, animatingDifferences: false)
}
```

저는 Company Section에서 데이터 뒤에 계속해서 무한스크롤이 되게 만들어보았습니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/27.png" alt="" width="50%">
</figure>
</center>


그리고 확인해보니 해당 섹션의 셀이 무한 스크롤 될때마다 전체레이아웃은 계속 불려오고 해당 섹션의 Company DataSource가 계속해서 호출되는것을 발견할 수 있었습니다.
상당히 비효율적임을 볼 수 있겠죠.


<br/>


### 2. 섹션 사이에 다른 섹션이 들어갈 수 있는지 확인

결과적으로는 불가능하다 입니다.

이를 정 가능하게 하고싶다면, 임의로 제가 데이터를 잘라서 섹션을 구분해 넣어 만드는 방식입니다.<br>
아래와 같은 방식이라면 가능합니다.

```swift
import UIKit
import SwiftyJSON

class ViewController: UIViewController {

    enum Section: Int, Hashable, CaseIterable, CustomStringConvertible {
        case consult1, company, consult2, expert

        var description: String {
            switch self {
            case .consult1: return "Consult"
            case .company: return "Company"
            case .consult2: return "Consult"
            case .expert: return "Expert"
            }
        }
    }

    struct Item: Hashable {
        let expert: Expert?
        let consult: Consult?
        let company: Company?

        init(consult: Consult? = nil, company: Company? = nil, expert: Expert? = nil) {
            self.expert = expert
            self.consult = consult
            self.company = company
        }

        private let identifier = UUID()
    }

    var dataSource: UICollectionViewDiffableDataSource<Section, Item>!

    @IBOutlet weak var collectionView: UICollectionView!
    var companyList = Array<Company>()
    var consultList = Array<Consult>()
    var expertList = Array<Expert>()

    var companySnapshot = NSDiffableDataSourceSectionSnapshot<Item>()

    override func viewDidLoad() {
        overrideUserInterfaceStyle = .light
        super.viewDidLoad()

        setData()
        setNavigation()
        createDataSource()
        self.collectionView.collectionViewLayout = self.createLayout()
        createCell()
        applySnapshot()
    }

    func setData() {
        print("setData")
        if let path = Bundle.main.path(forResource: "Home", ofType: "json"), let data = try? Data(contentsOf: URL(fileURLWithPath: path)) { print("path : \(path)")
            do {
                let jsonResult = try JSON(data: data)

                let companyResult = jsonResult["companyInfoList"]
                self.companyList = companyResult.arrayValue.compactMap({Company($0)})

                let consultResult = jsonResult["consultList"]
                self.consultList = consultResult.arrayValue.compactMap({Consult($0)})

                let expertResult = jsonResult["expertList"]
                self.expertList = expertResult.arrayValue.compactMap({Expert($0)})

            }catch {
                print("error : \(error)")
            }
        }
    }

    func createLayout() -> UICollectionViewLayout {
        print("createLayout")
        let sectionProvider = { (sectionIndex: Int, layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
            guard let sectionKind = Section(rawValue: sectionIndex) else { return nil }
            let section: NSCollectionLayoutSection
            if sectionKind == .consult1 || sectionKind == .consult2 {
                print("layout consult")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)
                item.contentInsets = NSDirectionalEdgeInsets(top: 2, leading: 2, bottom: 2, trailing: 2)

                let groupHeight = NSCollectionLayoutDimension.absolute(200)
                let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: groupHeight)
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitem: item, count: 1)

                section = NSCollectionLayoutSection(group: group)

            } else if sectionKind == .company {
                print("layout company")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)
                let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(350), heightDimension: .absolute(200))
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])
                section = NSCollectionLayoutSection(group: group)
                section.interGroupSpacing = 10
                section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
                section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)
            } else if sectionKind == .expert {
                print("layout expert")
                let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0), heightDimension: .fractionalHeight(1.0))
                let item = NSCollectionLayoutItem(layoutSize: itemSize)
                let groupSize = NSCollectionLayoutSize(widthDimension: .absolute(150), heightDimension: .absolute(150))
                let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])
                section = NSCollectionLayoutSection(group: group)
                section.interGroupSpacing = 10
                section.orthogonalScrollingBehavior = .continuousGroupLeadingBoundary
                section.contentInsets = NSDirectionalEdgeInsets(top: 10, leading: 10, bottom: 10, trailing: 10)
            }
            else {
                fatalError("Unknown section!")
            }
            return section
        }
        return UICollectionViewCompositionalLayout(sectionProvider: sectionProvider)
    }

    func createCell() {
        print("createCell")
        self.collectionView.register(CompanyCollectionViewCell.self, forCellWithReuseIdentifier: "CompanyCell")
        self.collectionView.register(ConsultCollectionViewCell.self, forCellWithReuseIdentifier: "ConsultCell")
        self.collectionView.register(ExpertCollectionViewCell.self, forCellWithReuseIdentifier: "ExpertCell")
    }

    func createDataSource() {
        print("createDataSource")
            self.dataSource = UICollectionViewDiffableDataSource<Section, Item>(collectionView: self.collectionView) {(collectionView, indexPath, item) -> UICollectionViewCell? in
            guard let section = Section(rawValue: indexPath.section) else { fatalError() }
            switch section {
            case .company:
                print("datasource company")
                if indexPath.row == self.companyList.count - 1 {
                    self.appendCompany()
                }
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CompanyCell", for: indexPath) as? CompanyCollectionViewCell else { preconditionFailure() }
                cell.configure(item.company!)
                return cell
            case .consult1, .consult2:
                print("datasource consult")
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ConsultCell", for: indexPath) as? ConsultCollectionViewCell else { preconditionFailure() }
                cell.configure(item.consult!)
                return cell
            case .expert:
                print("datasource expert")
                guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ExpertCell", for: indexPath) as? ExpertCollectionViewCell else { preconditionFailure() }
                cell.configure(item.expert!)
                return cell
            }
        }
    }

    func setNavigation() {
        navigationItem.title = "홈"
        if #available(iOS 15, *) {
            let appearance = UINavigationBarAppearance()
            appearance.configureWithTransparentBackground()
            appearance.backgroundColor = .systemGreen
            navigationItem.scrollEdgeAppearance = appearance
            navigationItem.standardAppearance = appearance
            navigationItem.compactAppearance = appearance
            navigationController?.setNeedsStatusBarAppearanceUpdate()
        }

    }

    func applySnapshot() {
        print("appleSnapshot")
        let sections = Section.allCases
        var snapshot = NSDiffableDataSourceSnapshot<Section, Item>()
        snapshot.appendSections(sections)
        print(sections)
        print(snapshot)

        let consultCount = self.consultList.count
        let consultItems1 = self.consultList[0..<consultCount/2].map { Item(consult: $0) }
        var consultSnapshot1 = NSDiffableDataSourceSectionSnapshot<Item>()
        consultSnapshot1.append(consultItems1)

        let companyItems = self.companyList.map { Item(company: $0) }
        companySnapshot.append(companyItems)

        let consultItems2 = self.consultList[consultCount/2..<consultCount].map { Item(consult: $0) }
        var consultSnapshot2 = NSDiffableDataSourceSectionSnapshot<Item>()
        consultSnapshot2.append(consultItems2)

        let expertItems = self.expertList.map { Item(expert: $0) }
        var expertSnapshot = NSDiffableDataSourceSectionSnapshot<Item>()
        expertSnapshot.append(expertItems)

        dataSource.apply(consultSnapshot1, to: .consult1, animatingDifferences: false)
        dataSource.apply(companySnapshot, to: .company, animatingDifferences: false)
        dataSource.apply(consultSnapshot2, to: .consult2, animatingDifferences: false)
        dataSource.apply(expertSnapshot, to: .expert, animatingDifferences: false)
    }

    func appendCompany() {
        print("appendCompany")
        let companyItems = self.companyList.map { Item(company: $0) }
        companySnapshot.append(companyItems)
        dataSource.apply(companySnapshot, to: .company, animatingDifferences: false)
    }

}
```

위 코드와 같이 기존의 Consult Section을 consult1, consult2 로 나누어 그 사이에 원하는 섹션 데이터를 넣는 것은 가능합니다.
그러나 이는 근본적으로 섹션안에 섹션을 넣는 방식은 아니죠.
