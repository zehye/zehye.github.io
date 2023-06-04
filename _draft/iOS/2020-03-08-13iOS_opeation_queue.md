---
layout: post
title: Operation Queue
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Operation Queue

Operation은 태스크(작업)와 관련된 코드와 데이터를 나타내는 추상 클래스이다.

Operation Queue는 연산(Operation)의 실행을 관리한다. 대기열(Queue)에 추가한 동작은 직접 제거할 수 없으며, 연산(Operation)은 작업이 끝날 때까지 대기열에 남아 있다. 연산(Operation)을 대기열에서 제거하는 방법은 연산(Operation)을 취소하는 방법뿐이다. 취소하는 방법은 연산 객체(Operation Object)의 cancel()메서드를 호출하거나 Oeration Queue의 cancelAllOperations() 메서드를 호출하여 대기열에 있는 모든 연산(Operation)을 취소하는 방법이 있다. 그리고 실행 중인 연산(Operation)의 경우 연산 객체(Operation Object)의 취소 상태를 확인하고 실행 중인 연산(Operation)을 중지하고 완료 상태로 변경된다.


### 연산 객체(Operation Object)

연산 객체 (Operation Object)는 애플리케이션에서 수행하려는 연산(Operation)을 캡슐화하는 데 사용하는 Foundation 프레임 워크의 Operation 클래스 인스턴스이다.


### OperationQueue의 주요 메서드/프로퍼티

#### 특정 Operation Queues 가져오기

current : 현재 작업을 시작한 Operation Queue를 반환

```swift
class var current: OperationQueue? { get }​
```

main : 메인 스레드와 연결된 Operation Queue를 반환

```swift
 class var main: OperationQueue { get }​
```

#### 대기열(Queue)에서 동작(Operation) 관리

```swift
// addOperation(_:) : 연산 객체(Operation Object)를 대기열(Queue)에 추가
 func addOperation(_ op: Operation)

// addOperations(_:waitUntilFinished:) : 연산 객체(Operation Object) 배열을 대기열(Queue)에 추가
func addOperations(_ ops: [Operation], waitUntilFinished wait: Bool)

// addOperation(_:) : 전달한 클로저를 연산 객체(Operation Object)에 감싸서 대기열(Queue)에 추가
func addOperation(_ block: @escaping () -> Void)

// cancelAllOperations() : 대기 중이거나 실행 중인 모든 연산(Operation)을 취소
func cancelAllOperations()

// waitUntilAllOperationsAreFinished() : 대기 중인 모든 연산(Operation)과 실행 중인 연산(Operation)이 모두 완료될 때까지 현재 스레드로의 접근을 차단
func waitUntilAllOperationsAreFinished()
```

#### 연산(Operation) 실행 관리

```swift
// maxConcurrentOperationCount : 동시에 실행할 수 있는 연산(Operation)의 최대 수
var maxConcurrentOperationCount: Int { get set }​

// qualityOfService : 대기열 작업을 효율적으로 수행할 수 있도록 여러 우선순위 옵션을 제공
 var qualityOfService: QualityOfService { get set }
```


#### 연산(Operation) 중단

isSuspended : 대기열(Queue)의 연산(Operation) 여부를 나타내기 위한 부울 값

false인 경우 대기열(Queue)에 있는 연산(Operation)을 실행하고, true인 경우 대기열(Queue)에 대기 중인 연산(Operation)을 실행하진 않지만 이미 실행 중인 연산(Operation)은 계속 실행

```swift
var isSuspended: Bool { get set }
```

#### 대기열(Queue)의 구성

name : Operation Queue의 이름

```swift
var name: String? { get set }
```
