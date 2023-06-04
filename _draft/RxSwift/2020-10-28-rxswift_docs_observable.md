---
layout: post
title: Rxswift Docs - Observable
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
본 내용은 공식문서를 참고하여 정리하였습니다. >> [공식문서 바로가기](http://reactivex.io/documentation/ko/observable.html)

<hr>

## Observable

일반적으로 개발자들은 자신의 코드가 점진적으로 작성된 순서에 따라 실행되고 완료되기를 기대하지만, Rx에서는 `Observer`에 의해 임의의 순서에 따라 병렬로 실행되고 결과는 나중에 연산된다. 즉, 메서드 호출보다는 `Observable`안의 데이터를 조회하고 변환하는 메커니즘을 정의한 후 Observable이 이벤트를 발생시키면 옵저버가 그 순간을 감지하고 준비된 연산을 실행시켜 결과를 리턴하는 메커니즘이다.

이 접근방법은 의존관계가 없는 많은 코드들을 실행할 때, 하나의 코드 블럭이 실행결과를 리턴할 때까지 기다릴 필요없이 계속해서 다음 코드 블럭을 실해할 수 있기 때문에 한번에 여러개의 코드를 실행시킬 수 있어 결과적으로 전체 코드의 실행 시간은 메커니즘 내 실행시간이 긴 동작만큼의 시간 만큼만 걸린다는 큰 장점을 제공한다.

- 옵저버는 Observable을 subscribe 한다.
- Observable은 항목들을 emit(방출)하거나 Observable의 메서드 호출을 통해 옵저버에 알림을 보낸다.


### 옵저버 생성

RX는 기본적으로 비동기와 병렬로 메서드를 호출한다.

**Subscribe** 메서드를 통해 옵저버와 Observable을 연결한다.

- onNext: Observable은 새로운 항목들을 배출할 때마다 이 메서드를 호출. 이 메서드는 Observable이 배출하는 항목을 파라미터로 전달받는다.
- onError: Observable은 기대하는 데이터가 생성되지 않았거나 다른 이유로 오류가 발생할 경우 오류를 알리기 위해 이 메서드를 호출. 이 메서드가 호출되면 `onNext`나 `onCompleted`는 더이상 호출되지 않는다. `onError`메서드는 오류정보를 저장하고 있는 객체를 파라미터로 전달받는다.
- onCompleted: 오류가 발생하지 않았다면 Observable은 마지막 `onNext`를 호출한 후 이 메서드를 호출

Observable의 조건은 아래와 같다.

1. onNext는 0번 이상 호출될 수 있다.
2. 그 이후에는 onCompleted 또는 onError 둘중 하나를 마지막으로 호출한다.
3. 단 onCompleted와 onError 모두를 동시에 호출하지는 않는다.
4. onNext 호출을 항목의 배출(emit)으로 부르며 onCompleted 혹은 onError 호출을 알림(notification)이라 부른다.


### Unsubscribe

현재 구독중인 Observable 중, 옵저버가 더이상 구독을 원하지 않는 경우 `unsubscribe` 메서드를 호출함으로써 구독을 해제할 수 있다. 만약 더이상 관심없는 다른 옵저버가 존재하지 않는다면 Observable들은 새로운 항목들을 배출하지 않는다. 즉, unsubscribe는 연산자 체인을 통해 옵저버가 구독하고 있었던 Observable들이 더이상 항목들을 배출하지 못하도록 체인 안에 연결된 링크들을 끊어버릴 수 있다.



## 명명 규칙에 대한 참고 내용

Rx를 구현하는 언어들은 자신만의 독특한 특징들을 갖고 있다. 반드시 지켜야하는 강제성 있는 규칙은 아니지만, 언어 별 구현체 간에 공통적으로 유지해야하는 명명 규칙들은 존재하고 있다. 뿐만 아니라, 규칙 간에는 각기 다른 문맥에서 서로 다른 함축적 의미로 사용되는 것들이 있으며 특정 언어의 관용구에서는 상당히 어색한 의미로 해석되기도 한다.



### Hot Observable & Cold Observable

Observable은 연속된 항목들에 대해 Observable에 따라 다르게 배출한다.

Hot Observable은 생성되자마자 항목들을 배출하기도 하기 때문에 이 Observable을 구독하는 옵저버들은 어떤 경우에 항목들이 배출되는 중간부터 Observable을 subscribe할 수 있다. 반대로, Cold Observable의 경우 옵저버가 구독할때까지 항목을 배출하지 않기 때문에 이 Observable을 구독하는 옵저버는 Observable이 배출하는 항목 전체를 구독할 수 있도록 보장받는다.


### 연산자 체인

대부분의 연산자들은 Observable 상에서 동작하고 Observable을 리턴한다. 이런 접근 방법은 연산자들을 하나의 Observable에 적용하고 또 다음 연산자에 다시 적용할 수 있는 연산자 체인을 제공한다. 연산자 체인에 걸려있는 각각의 연산자들은 이전 연산자가 리턴한 Observable을 변경한다.

특정 클래스의 다양한 메서드 연산을 통해 같은 클래스에 있는 항목들을 변경하는 빌더 패턴도 존재한다. 이 패턴 역시 비슷한 방법으로 메서드 체인을 제공한다. 그러나, 연산자 호출 순서가 문제가 되지 않는 빌더 패턴과는 달리 Observable 연산자들은 호출 순서에 영향을 받는다.

Observable 연산자 체인은 원본 Observable과는 떨어져서 동작할 수 없고 순서대로 동작하기 때문에, 호출 체인 중 바로 이전에 호출된 연산자가 리턴한 Observable을 기반으로 실행된다. 
