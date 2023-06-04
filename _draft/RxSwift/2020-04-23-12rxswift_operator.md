---
layout: post
title: 기본적인 Operators(just, from, map, filter)
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

기본 연산자들에 대해 공부해봅니다.

- just: 넣은 그대로 출력
- from: 하나씩 분절되어 출력
- map: 전달되어온 인자들을 매핑하여 출력
- filter: 말그래도 필터


```swift
import RxSwift
import UIKit

class ViewController: UITableViewController {
    @IBOutlet var imageView: UIImageView!
    @IBOutlet weak var progressView: UIActivityIndicatorView!

    var disposeBag = DisposeBag()

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func exJust1() {
        // just에 무언가를 넣어주면 그 다음 subscribe를 해주면 첫번쨰 인자로 바로 넘어가게 해준다
        Observable.just("Hello World")
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exJust2() {
        // array를 넣으면 그 array 그대로 나오듯 뭘 넣든 그게 그대로 나온다
        Observable.just(["Hello", "World"])
            .subscribe(onNext: { arr in
                print(arr)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exFrom1() {
        // array를 넣었는데 실제로 한번에 하나씩 출력된다.
        // array에 있는 것을 하나씩 띄어서 출력, 하나씩 뺴어서 출력 > 타입과 관련 없이!
        Observable.from(["RxSwift", "In", "4", "Hours"])
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exMap1() {
        // hello가 그대로 내려가는데 중간에 map이 걸친다 > map으로 전달된다 > str로 map으로 전달된다는 것
        // str 뒤에 rxswift를 붙여서 그 붙인게(str) 전달되어 출력됨 > 온걸 받아서 내가 매핑해서 전달한다
        Observable.just("Hello")
            .map { str in "\(str) RxSwift" }
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exMap2() {
        // from으로 받으니까 array에 있는 것 하나하나 내려가게 되는데
        // map에 의해 count 되어진다 > string으로 내려와 Int로 바뀌었고 이를 차례대로 출력
        Observable.from(["with", "곰튀김"])
            .map { $0.count }
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exFilter() {
        // from으로 차레대로 일단 내려가면서 1이 먼저 내려가면 filter를 거지고 true/false
        // true일때만 아래로 내려가고 false면 그대로 버린다
        Observable.from([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
            .filter { $0 % 2 == 0 }
            .subscribe(onNext: { n in
                print(n)
            })
            .disposed(by: disposeBag)
    }

    @IBAction func exMap3() {
        //
        Observable.just("800x600")
            .map { $0.replacingOccurrences(of: "x", with: "/") }  // "800/600"
            .map { "https://picsum.photos/\($0)/?random" }  // "https://picsum.photos/800/600/?random"
            .map { URL(string: $0) }  // URL?
            .filter { $0 != nil }
            .map { $0! }  // URL!
            .map { try Data(contentsOf: $0) }  // Data
            .map { UIImage(data: $0) }  //UIImage?
            .subscribe(onNext: { image in
                self.imageView.image = image
            })
            .disposed(by: disposeBag)
    }
}
```

추가적인 operators들이 궁금하다면 >> [Rxswift docs](http://reactivex.io/documentation/ko/operators.html) 참고!<br>
++ [참고>>>>](https://github.com/iamchiwon/RxSwift_In_4_Hours/blob/master/README_s1.md)<br>
++ marble diagram
