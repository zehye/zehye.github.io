---
layout: post
title: iOS Dispatch Group 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## Dispatch Queue

dispatchQueue 라는 객체는 메인 스레드나 백그라운드 스레드에서 업무를 직렬이나 병렬적으로 수행하도록 한다.

큐에 업무를 보낼때, 백그라운드 큐에 넣으면 시스템이 알아서 수행해준다. 그런데 각 테스크가 어떤 스레드에 의해 수행되는지는 알수가 없다. 직접 구현해야하는 부분은 동기/비동기적으로 테스크가 수행할 수 있도록 하는 것. 이때 중요한 점은 메인스레드에서는 데드락 이슈로 인해 동기적인 처리를 하지는 않아야 한다.

- 동기: 하나의 테스크가 끝나야 그 다음 테스크 진행
- 비동기: 여러 테스크가 동시에 진행

이때 중요한 것은 스레드를 생성할 수 있는 갯수의 한계는 존재하기 때문에, dispatchQueue는 너무 많이 만들면 안된다.

- 직렬: 하나씩 차례대로 테스크 전달
- 병렬: 한번에 여러개의 테스크 전달

### Dispatch Group

서로 다른 테스크들을 그룹화하여 테스크들이 완료될때까지 기다리거나 완료되면 알림을 받을 수 있도록 하는 것<br>
여러작업을 하나로 묶는 것. 따라서 그룹에 포함된 모든 작업이 완료되어야 그룹이 완료된다. 


#### 사용 방법

1. 테스크를 위해 concurrent queue를 생성
2. 각각의 테스크를 묶을 수 있는 DispatchGroup를 생성
3. 각각의 테스크들을 queue에 담을 때 .enter() 호출
4. 각 테스크의 완료 시점에 .leave()를 호출
5. .notify를 통해 완료 이벤트의 알림을 받고 각각의 테스크가 다 끝난 후 필요한 작업을 진행


```swift
let myQueue = dispatchQueue(label: "com.nil.work", attribute: .concurrent)

let myGroup = DispatchGroup()

myGroup.enter()
myQueue.async {
  for i in 1...10 {
    print("\(i), 가")
  }
  myGroup.leave()
}

myGroup.enter()
myQueue.async {
  for i in 100...105 {
    print("\(i), aaaa")
  }
  myGroup.leave()
}

myGroup.notify(queue: myQueue) {
  print("end...")
}
```

- .enter()는 myGroup에 테스크를 포함시키는 것을 의미
- .leave()는 DispatchGroup에 해당 테스크가 완료되었다고 알려주는 의미

**enter와 leave는 서로 쌍을 이루어야 한다!!**
