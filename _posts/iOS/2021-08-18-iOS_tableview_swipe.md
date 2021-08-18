---
layout: post
title: iOS UITableview Swipe Actions
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## Tableview Swipe

UITableview의 delegate를 상속하는 뷰컨에서 Swipe를 입력하면 leading, trailing으로 시작하는 메소드가 있다.<br>
말 그대로 leading은 왼쪽 끝에서 보이게 될 swipe action이고 trailing는 오른쪽 끝에서 보이게 될 swipe action이다.

이 swipe action을 구현하기 위해서는 **UIContextualAction** 과 **UISwipeActionsConfiguration** 을 만들어줘야한다.



### UIContextualAction

테이블 로우를 스와이프 했을 때 보여지는 액션<br>
이를 사용하기 위해서는 UITableview delegate에 UISwipeActionsConfiguration을 **초기화** 해야한다.



### UISwipeActionsConfiguration

테이블 뷰의 로우를 스와이프 했을 때 작동할 액션들의 set <br>
테이블 뷰 로우의 커스텀 스와이프 액션들을 모아둔 객체를 의미한다.



#### 직접 구현해보기(leading)

1. UIContextualAction을 만들어서 UISwipeActionsConfiguration에 담아 return

```swift
func tableView(_ tableView: UITableView, leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
    let actions1 = UIContextualAction(style: , title: , handler: )
    return UISwipeActionsConfiguration(actions: )
}
```

- UIContextualAction.style: enum 타입으로 **normal** 와 **destructive** 가 있다.
- UIContextualAction.handler: action, sourceView, completionHandler를 파라미터로 갖는 handler


```swift
func tableView(_ tableView: UITableView, leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
    let actions1 = UIContextualAction(style: .normal, title: "1", handler: { action, view, completionHaldler in
        print("action performed")
        completionHaldler(true)
    })
    return UISwipeActionsConfiguration(actions: [actions1])
}
```




#### 직접 구현해보기(trailing)

leading과 구현방식은 같지만, UISwipeActionsConfiguration의 actions에 UIContextualAction를 담는 순서에 따라 보여지는게 반대이다.


```swift
func tableView(_ tableView: UITableView, leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
    let actions1 = UIContextualAction(style: .normal, title: "1", handler: { action, view, completionHaldler in
        print("action1 performed")
        completionHaldler(true)
    })
    let actions2 = UIContextualAction(style: .normal, title: "2", handler: { action, view, completionHaldler in
        print("action2 performed")
        completionHaldler(true)
    })
    return UISwipeActionsConfiguration(actions: [actions1, actions2])
}

func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
    let actions1 = UIContextualAction(style: .normal, title: "1", handler: { action, view, completionHaldler in
        print("action1 performed")
        completionHaldler(true)
    })
    let actions2 = UIContextualAction(style: .normal, title: "2", handler: { action, view, completionHaldler in
        print("action2 performed")
        completionHaldler(true)
    })
    return UISwipeActionsConfiguration(actions: [actions1, actions2])
}
```

- leading swipe title > 1, 2
- trailing swipe title > 2, 1


즉, 같은 순서로 액션들을 담았음에도 불구하고 반대방향으로 배치되어 나온다.

leading메소드는 UISwipeActionsConfiguration에서 action을 꺼내 leading쪽부터 데이터를 밀어넣고<br>
trailing메소드는 UISwipeActionsConfiguration에서 action을 꺼내 trailing쪽부터 즉, 서로 반대방향으로 밀어넣는다고 생각하면 쉽다.




#### View 꾸며보기

swipe action의 버튼 이미지 혹은 색상을 입히는 방법이다.

```swift
func tableView(_ tableView: UITableView, leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
    let actions1 = UIContextualAction(style: .normal, title: "1", handler: { action, view, completionHaldler in
        print("action1 performed")
        completionHaldler(true)
    })
    actions1.backgroundColor = .systemOrange
    actions1.image = UIImage(named: "comment.png")
    let actions2 = UIContextualAction(style: .normal, title: "2", handler: { action, view, completionHaldler in
        print("action2 performed")
        completionHaldler(true)
    })
    actions2.backgroundColor = .systemYellow
    actions2.image = UIImage(named: "share.png")
    return UISwipeActionsConfiguration(actions: [actions1, actions2])
}
```

이렇게 UIContextualAction변수를 생성하고나서 적용해주면 된다.<br>
이때 주의해야할 점은 swipe action view 영역에 맞는 이미지 크기여야 한다!
