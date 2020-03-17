---
layout: post
title: View Controller의 생명주기(LIfe-Cycle)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

iOS를 공부하면서 간단한 실습을 하다보니 view controller의 생명주기에대해 공부할 필요성을 느꼈다. 처음 접헀을때는 이게 쓸일이 있을까? 싶은 코드였는데, 꼭꼭 반드시 필요하고 제대로 알고 있어야하는 개념이었다. 실제 코드에 사용해보기도 했지만, 조금 헷갈리는 부분들이 있어 정리를 하게 되었다.

## View Controller Life Cycle

앱들은 모두 **View Controller** 로 이루어져있다. 화면이 하나만으로 구성되는 앱도 있겠지만, 대부분의 앱은 여러개의 화면들로 구성되어있을 것이다. 이런 각각의 뷰들은 각각의 생명주기를 가지고 있고, 가져야 한다. 뷰 컨트롤러에게 있어서 생명주기란 **보여지고 사라지는 주기** 를 의미한다.

<center>
<figure>
<img src="/assets/post-img/iOS/23.jpeg" alt="" width="50%">
</figure>
</center>

뷰 컨트롤러는 위 사진과 같은 주기를 가지게 된다. 기본적으로 우리가 이해하기에는 아래를 이해하면 된다.

```
viewDidLoad
viewWillAppear
viewDidAppear
viewwillDisappear
viewDidDisappear
```

그냥 보면 무슨 함수인지 모르겠지만, 함수의 이름을 잘 살펴보면 **Will** 과 **Did** 가 보임을 알 수 있다. Will은 우리가 알고있듯 미래를 의미할 것이고, Did는 과거를 의미할 것이다. 하나씩 제대로 살펴보도록 하자!


### viewDidLoad

익숙한 함수일 것이다. 프로젝트를 만들면 뷰 컨트롤러에 항상 꼭 보이는 함수이기 때문이다.

<center>
<figure>
<img src="/assets/post-img/iOS/24.png" alt="" width="50%">
</figure>
</center>

이 함수는 왜 항상 존재하는것이며 그래서 이 함수가 하는 일은 무엇일까?<br>
애플 문서에 따르면

> "Called after the controller's view is loaded into memory"

`뷰의 컨트롤러가 메모리에 로드되고 난 후 호출된다`라고 의미한다. 우선 모든 뷰는 메모리에 올라가야 우리가 접근이 가능할 것이다. 그렇기 때문에 이 viewDidLoad 함수의 기능은 뷰의 로딩이 완료되었을때 시스템에 의해 자동으로 호출되어 일반적으로 리소스를 초기화하거나 초기화면을 구성하는 용도로 사용한다.

화면이 처음 만들어질때 한번만 실행되기 때문에 처음 한번만 실행해야 하는 초기화 코드가 있을 경우 해당 메소드 내부에 작성하면 되는 것이다!


### viewWillAppear

그럼 이제 뷰는 로드가 되었고 그 이후에 동작할 수 있는 첫번째 함수가 `viewWillAppear`함수이다. 뷰가 로드되었으니 이제 해당 뷰는 우리눈에 보여야할 것이다. viewWillAppear 함수가 하는 일이 바로 뷰가 나타날 것이라는 신호를 컨트롤러에게 알리는 것이다.

즉 뷰가 나타나기 직전에 호출된다.

11
