---
layout: post
title: asyncAfter의 deadline과 wallDeadline의 차이
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## asyncAfter

기본적인 dispatchQueue만 사용해보다가 asyncAfter를 접하게 되었고, 단순히 deadline을 통해 시간차를 두고 동작을 실행시킨다?<br>
정도로만 이해하고 넘어가 있었다. 그런데 찾아보니 deadline 말고도 wallDeadline이란게 있네??? 이게 뭐지?

공식문서를 찾아보니 각각이 의미하는 내용은 같지만 파라미터와 타입이 다른것을 확인할 수 있었다.

```swift
func asyncAfter(deadline: DispatchTime, execute: DispatchWorkItem)

func asyncAfter(wallDeadline: DispatchWallTime, execute; DispatchWorkItem)
```

각각의 정의를 살펴보면 아래와 같다.

- DispatchTime: (Structure)A point in time relative to the default clock, with nanosecond precision
  - struct, 나노세컨드의 정밀도를 가진 시스템 시계에 상대적인 시점 > 시스템 시계에 의존하지 않음
- DispatchWallTime: (Structure)An absolute point in time according to the wall clock, with microsecond precision
  - struct, 마이크로세컨드의 정밀도를 가진 wall time에 따른 절대 시점 > 시스템 시계에 의존함

> wall time은  컴퓨터 프로그램의 시작부터 끝까지 걸린 실제 시간입니다.      
즉, 작업이 완료된 시간과 작업이 시작된 시간의 차이입니다.           
따라서 벽 시간은 프로세서가 특정 작업에 대해 적극적으로 작업하는 시간만을 측정하는 CPU 시간과 다릅니다.

위키 백과에서 찾은 내용이다.

시스템 시계에 의존한다는 것은 무슨 의미를 뜻할까?

```
n초 동안 숫자는 1부터 +1 되며 카운트가 되어 프린트 찍힌다고 생각해보자.

1
2
3
...

이런식으로!

숫자가 하나씩 카운트 되고 있는 상황에서 시스템 시계를 바꿔놓는다고 생각해보자.
그러면 그 즉시 wallDeadline은 발생하지만 deadline은 발생하지 않는다.
즉 deadline은 시스템 시계에 의존하지 않기 때문에 시스템 시계의 설정이 변한다 할지라도 아무런 영향을 받지않지만
wallDeadline은 시스템 시계에 의존하기 때문에 시계의 설정이 변함과 동시에 영향을 받게 된다.

즉, 시스템 시계를 기준으로 그 절대 시점을 보느냐, 상대시점을 보느냐가 두 메서드의 관건이다.
