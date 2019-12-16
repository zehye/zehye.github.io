---
layout: post
title: 동기화의 또다른 도구, 모니터(Monitors)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

이전에 동기화도구로 세마포에 대해서 공부를 했는데, 이 세마포는 굉장히 옛날에 사용하던 방식이다. 이제 많이 사용되는 것은 Monitors이고 이는 자바에서 사용된다.

## 모니터 (Monitors)

- 세마포 이후 프로세스 동기화 도구
- 세마포보다 고수준 개념

### 개념

- 공유자원 + 공유자원 접근함수로 구성됨
- 2개의 queues: 배타동기(`Mutual exclusion queue`) + 조건동기(`Conditional synchronization`)
- 공유자원 접근함수에는 최대 1개의 쓰레드만 집입가능: 나머지는 큐에서 진입못하고 기다리고 있어야한다.(Mutual exclusion queue에서 대기)
- 진입 쓰레드가 조건 동기로 블록되면 새 쓰데르 진입가능: `wait call`> 들어왔던 쓰레드는 Conditions synchronization에 갇히게 된다. 그러면 새 쓰레드가 진입가능하다.
- 새 쓰레드는 조건동기로 블록된 쓰레드를 깨울 수 있음: `notify()`> 블록된 쓰레드를 깨워준다.
- 깨워진 쓰레드는 현재 쓰레드가 나가면 재진입할 수 있음: 하나의 쓰레드만 있을 수 있으니까, 그 비어진 자리에 깨워진 쓰레드가 들어올 수 있게 된다.

### 자바 모니터

자바에서는 모든 객체가 모니터가 될 수 있다.

- 배타동기: `synchronization` 키워드를 사용하여 지정 > 한 쓰레드에만 접근이 가능하다.
- 조건동기: `wait(), notify(), notifyAll()` 메소드를 사용


```java
class C {
  private int value;
  synchronization void f() { // f를 호출하면 다른 쓰레드는 접근 불가능
    ////////code//////////
  }
  synchronization void g() {
    ////////code//////////
  }
  void h() { // 공통변수를 업데이트하는 함수가 아니라는 의미, 이때 이 쓰레드는 동작이 가능
    ////////code//////////
  }
}
```

## 모니터 (Monitors) 사용하는 방법

### Mutual exclusion

```java
synchronization {
  ////////critical section//////////
}
```

세마포처럼 공통변수의 앞과 뒤에 acquire, release 혹은 value = 1와 같은 지정이 특별히 필요없고 단시 `synchronization`만 써주면 된다.

```java
class BankAccount {
  int balance;
  synchronization void deposit(int eat) {
    int temp = balance + ;
    System.out.println('');
    balance = temp;
  }
  synchronization void withdraw(int eat) {
    int temp = balance + eat;
    System.out.println('');
    balance + temp;
  }
  int getBalance() {
    return balance;
  }
}
```

### Ordering

P1 | P2
  | wait();
S1; | S2;
notify(); |

초기값도 지정해주지않고 P1은 곧바로 S1코드를 실행하고 실행이 끝날때 `notify()`만 해주고, P2는 처음 시작하자마자 `wait()`을 실행하고 S2 코드를 실행해주게 된다. (깔-끔)

```java
class BankAccount {
  int balance;
  synchronized void deposit(int eat) {
    int temp = balance + ;
    System.out.println('');
    balance = temp;
    notify();
  }
  synchronized void withdraw(int eat) {
    while (balance =< 0)
      try {
        wait();
      } catch (InterruptedException e) {}
    int temp = balance + eat;
    System.out.println('');
    balance + temp;
  }
  int getBalance() {
    return balance;
  }
}
```

### The Dining Philosopher Problem: Monitor로 구현해보기

```java
class Chopsticks {
  private boolean inUse = false //아무도 젓가락을 사용하지 않겠지
  synchronized void acquire() throws InterruptedException { //젓가락을 잡는것 acquire();
    while (inUse) // 누군가가 젓가락을 집고있으면
      wait(); // 기다리고 철학자는 갇혀있게 됨
    inUse = true;
  }
  synchronized void release() { // 젓가락을 놓는것 release;
    inUse = false;
    notify(); // 누군가 큐에서 기다리고 있다면 깨워줘야지
  }
}
```
