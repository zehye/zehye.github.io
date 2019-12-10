---
layout: post
title: 동기화를 위한 도구 - Semaphore(Mutual exclusive, Ordering)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

### 동기화를 위한 도구

- Semaphores
- Monitors

### 1. Semaphores: 전통적인

- 철도의 신호기(깃발) 등의 사전적 의미를 가짐
- `동기화 문제 해결을 위한 소프트웨어 도구`
- 네덜란드 사람이 만들었음
- 구조: 정수형 변수 + 두개의 동작 (P:Proberen(test)->acquire(), V:Verhogen(increment)->release())
  - 정수형 변수를 테스트하고 값을 증가시키는 동작

#### acquire()

```java
void acquire() {
  value --; // number of permit(무언가를 할 수 있는 권한)
  if (value < 0) {
    add this process/thread to list
    block;
  }
}
```

- acquire()가 실행되면 value값은 1만큼 감소된다.
- value < 0이 되면 acquire를 호출했던 프로세스나 쓰레드를 큐(list)안에 넣는다.
  - 즉, acquire내부를 살펴보면 `value, P, V, 큐(list)`가 존재
- 누가 꺼내주기 전까지는 block한다. (멈춰있다)

#### release()

```java
void release() {
  value ++;
  if (value <= 0) {
    remove a process P from list
    wakeup P;
  }
}
```

- release()가 실행되면 value 값은 1만큼 증가한다.
- value <= 0이 되면 release를 갖혀있던 한 쓰레드를 꺼내서 깨워준다
- 즉 큐에서 해방시켜준다.

<center>
<figure>
<img src="/assets/post-img/OS/31.jpeg" alt="" width="50%">
</figure>
</center>


### 이러한 Semaphores는 어떠한 때에 사용할까?

#### 상황1. Mutual exclusion: 상호 배타 목적으로 사용

- `value = 1`을 둔다.(권한을 1로 두었다는 것을 의미)
- acquire()를 호출하면 아직 value<0이 되지 않음으로 바로 `critical section` 으로 진입
- context switching이 실행되어 다른 프로세스가 돈다고 해보자
- 다른 프로세스가 acquire()하여 value=-1이 되었으니 큐에 갇히게 됨
- 즉, critical section으로 진입이 되지 못함

```python
import java.util.concurrent;

class BankAccount {
  int balance;
  Semaphore sem;
  BankAccount() {
    sem = new Semaphore(1);
  }
  void deposit(int n) {
    try{
    sem.acquire();
    } catch (InterruptedException e) ()
    // critical section
    int temp = balance + n;
    System.out.println("+");
    balance = temp;
    //
    sem.release();
  }
  void withdraw(int n ){
    try{
    sem.acquire();
    } catch (InterruptedException e) ()
    // critical section
    int temp = balance + n;
    System.out.println("-");
    balance = temp;
    //
    sem.release();
  }
  int getBalance() {
    return balance;
  }
}
```

#### 상황2. Ordering

앞에서 이야기 했듯이 프로세스/쓰레드 동기화에서는 임계구역 문제를 해결하는 것도 중요하지만, 프로세스의 실행 순서를 제어하는 것 또한 중요하다.

- sem.value = 0

P1 | P2
  | sem.acquire();
S1; | s2;
sem.release(); |  

- CPU가 P1을 먼저 가리킨다면 자연스럽게 S1이 실행될 것이다.
- 공교롭게 CPU가 P2를 가리키게 된다면 sem.acquire()를 실행하게 되어 value = -1로 만든다.
- 그러면 P2는 sem.acquire()에 block 될 것이고, S1이 실행될 것이다.
- S1을 끝내고 sem.release()를 실행시키면 value = 0 이 되고
- block 된 P2는 다음 코드로 진행이 가능할 것이다.

### 문제: 원하는 방식으로 입금/출금 프로그램 만들기

```python
import java.util.concurrent;

class BankAccount {
  int balance;
  Semaphore sem; dsem; wsem;
  BankAccount() {
    sem = new Semaphore(1);
    dsem = new Semaphore(0);
    wsem = new Semaphore(0);
  }
  void deposit(int n) {
    try{
    sem.acquire();
    // critical section
    int temp = balance + n;
    System.out.println("+");
    balance = temp;
    sem.release();
    //
    wsem.release();
    dsem.acquire();
    } catch (InterruptedException e) ()
  }
  void withdraw(int n ){
    try{
    wsem.acquire();
    } catch (InterruptedException e) ()
    // critical section
    int temp = balance + n;
    System.out.println("-");
    balance = temp;
    sem.release();
    //
    dsem.release();
  }
  int getBalance() {
    return balance;
  }
}
```
