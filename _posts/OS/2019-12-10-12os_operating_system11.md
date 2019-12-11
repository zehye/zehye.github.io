---
layout: post
title: 전통적 동기화 예제(생산자-소비자문제, RW문제, 식사하는 철학자 문제)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 전통적 동기화 예제 (Classical Synchronization Problem)

### 1. Producer and Consumer Problem (생산자-소비자 문제)

다른말로 유한버퍼 문제(`Bounded Buffer Problem`)라고도 한다.<br>
생산자가 데이터를 생산하면 소비자는 그것을 소비하는데, 그 생산자와 소비자 사이에는 `저장공간인 buffer`가 있고 이 buffer의 크기는 유한하다.

예) 컴파일러 -> 어셈블러, 파일서버 > 클라이언트, 웹서버 > 웹 클라이언트

**Bounded Buffer?**

Bounded: 한계가 있다.<br>
buffer: 데이터를 모아둘 수 있는 메모리/디스크 공간

- 생산된 데이터는 버퍼에 일단 저장(속도차이 등)
- 현실 시스템에서 버퍼 크기는 유한
- 생산자는 버퍼가 가득 차면 더 넣을 수 없다 (size = count)
- 소비자는 버퍼가 비면 뺄 수 없다.

<center>
<figure>
<img src="/assets/post-img/OS/32.jpeg" alt="" width="50%">
</figure>
</center>


```java
class Buffer {
  int[] buf;
  int size; //버퍼 갯수(용량), 크기
  int count; //버퍼안의 생산되어 저장되어있는 갯수
  int in; //처음 in 인덱스 값부터 생산자의 데이터를 넣겠다.
  int out; // 빼내는 위치
}

Buffer(int size){
  buf = new int(size);
  this.size = size;
  count = in = out = 0
}
```

```java
// check if buf is full
void insert(int item) { // 생산자가 버퍼에 데이터를 집어넣는 함수
  while (count == size) // 같다면 무한루프를 돌며 여기서 멈춰있다.

// buf is not full
  buf(in) = item; //소비자가 하나를 가져가 1의 공간이 생긴다면 item을 in에 넣고
  in = (in + 1) % size; //나머지가 0
  count ++;
}

int remove() {
  // check if buf is empty
  while (count == 0) //버퍼가 비어져있으면 무한루프 돌면서 소비자는 기다리고 있음

// buf is not empty
  int item = buf(out); //buf에 out한 값을 item에 넣는다.
  out = (out + 1)%size; //나머지가 0
  count --;
  return item
}
```

```java
class Test {
  public static void main(String[] args) {
    Buffer b = new Buffer(100);
    Producer p = new Producer(b, 10000);
    Consumer c = new Consumer(b, 10000);
    p.start()
    c.start()
    try {
      p.join()
      c.join()
    } catch (InterruptedException e) {
      System.out.println("Number of items in the buf", b.count);
    }
  }
}
```

정상적으로 실행시켜보면 `count=0`이 나올것이다(동기화를 하는 이유)

**그러나 잘못된 결과가 실행되는 경우가 있다.**<br>

1. 실행불가<br>
2. count ≠ 0<br>

이런 결과가 나타나는 이유는 결국 아래와 같다.
- 공통변수 count(), buf[]에 대한 동시 업데이트 진행
- 공통변수 업데이트 구간(임계구역)에 대한 동시 진입이 진행

따라서 이를 해결하기 위한 방법은 아래와 같다.
- 임계구역에 대한 동시 접근 방지(상호배타)
- 세마포를 사용한 상호배타 (mutual exclusion)
- 세마포: mutex.value = 1(# of permit)

#### Busy-Wait

바쁘게 기다린다는 의미로 cpu에서 생산자는 버퍼가 가득차있으면 더이상 생산을 해낼수가 없고 버퍼가 비어있으면 소비자는 버퍼가 찰때까지 기다려야 한다. 이 기다리는 코드는 아래와 같다.

- 생산자: 버퍼가 가득차면 기다려야 = 빈공간이 있어야 한다.
- 소바자: 버퍼가 비면 기다려야 = 찬 공간이 있어야 한다.

```java
Buffer(int size){
  buf = new int(size);
  this.size = size; //버퍼 크기
  count = in = out = 0 // 생산된 항목 갯수
}
```

size == count라면 무한루프 돌면서 기다려라라고 함으로써 cpu는 이 시간에 계속해서 size == count한 것을 계속 세면서 다른일을 하지못하고 있다. (비효율적) 이런 상황을 `busy wait`이라고 한다.

os는 성능을 높여줘야 하는데, cpu는 아무일도 못하고 무한루프 돌고 있으니 낭비가 굉장히 심하다.

#### 세마포를 사용한 busy-wait 회피
  - mutex.value = 1
  - 생산자: empty.acquire() # of permit = BUF SIZE
  - 소비자: full.acquire() # of permit = 0

<center>
<figure>
<img src="/assets/post-img/OS/32.jpeg" alt="" width="50%">
</figure>
</center>

[생산자] | [소비자]
empty.acquire(); | full.acquire();
PRODUCE; | CONSUME;
full.release(); | empty.release();

즉, 만약 버퍼가 가득차있다면 생산자쪽에 semaphore가 block 처리를 시켜줌으로써 더이상 cpu가 여기서 무한루프를 돌지않도록 해준다. 이를 깨워주는것은 소비자가 이 버퍼에서 빼내어주어서 빈공간이 생기면 다시 생산자가 넣을 수 있도록 해준다. 따라서 block은 자고있다는 뜻으로 서로가 서로를 가두고 풀어줌으로써 효율을 높여준다.

**따라서 semaphore를 통해서 busy-wait이 일어나지 않도록, 즉 운영체에의 성능을 높여주도록 한다.**

### 2. Reader-Writer Problem

주로 `공통된 데이터베이스를 접근할 때 발생하는 문제`이다. 데이터베이스는 공통이니까, 임계구역이 반드시 존재해야한다. 즉 어느 경우에도 한사람만 들어올 수 있도록 해야하는데 그렇게 하면 상당히 비효율적이다. 더 나아가 reader, writer의 경우는 조금 다르다.

- 공유 데이터베이스 접근
  - Reader: read data. never modify it
  - Writer: read data and modify it
  - 상호배타: 한번에 한개의 포로세스만 접근 > 비효율적

- 효율성 제고
  - Each read or write of the shared data must happen within a critical action
  - Guarantee mutual exclusion for Writer
  - Allow multiple readers to execute in the critical section at once

서로 공유하는 데이터에 한해서, 읽기쓰기는 임계구역으로 구현해야하는데 writer 두명이 들어오면 임계구역 해야하지만 reader는 여러명이 들어오기를 혀용한다. (내용을 바꾸는것이 아니니까) 그러다가 reader가 들어와서 database를 조회하고 있는데 writer가 온다면 writer는 기다려야 한다.

- 변종
  - The first R/W problem(readers-preference): 항상 reader 에게 우선권을 준다.
  - The second R/W problem(writer-preference): writer에게 우선권을 준다.
  - The third R/W problem: 우선권을 아무에게도 주지 않는것

**즉, reader가 들어왔는데 writer는 block되어야 하고 반대의 상황도 같다. 그리고 reader가 들어와있는데 다른 reader가 들어오고 싶다면 그건 효율성 측면에서 허용이 된다!**


### 3. Dining Philosopher Problem (식사하는 철학자 문제)

- 식사하는 철학자 문제
  - 5명의 철학자, 5개의 젓가락, 생각->식사->생각->식사...
  - 식사하려면 2개의 젓가락이 필요

- 프로그래밍
  - 젓가락: 세마포(# of permit =1) // 세마포의 초기값을 1로 두고
  - 젓가락과 세마포에 일련번호: 0 - 4 //
  - 왼쪽 젓가락 -> 오른쪽 젓가락

```python
public void run() {
  try {
    while (true) {
      lstick.acquire();
      rstick.acquire();
      eating();

      lstick.release();
      rstick.release();
      thinking();
    }
  } catch (InterruptedException e)
}

void eating() {
  System.out.println('I' + id + "eating");
}

void thinking() {
  System.out.println('I' + id I "thinking");
}
```
```python
class Test {
  static final int num = 3

  public static void main(Sting[] args) {

    // Chopsticks
    Semaphore[] stick = new Semaphore[num];
    for (i=0l i=num; i++) {
      stick[i] = new Semaphore(1)
    }
    // Philosopher
    Philosopher[] phil =new Philosopher[num];
    for (i=0l i=num; i++;){
      phil[i] = new Philosopher(i, stick[i], stick[(i+1)%num]);
    }
    // let Philosopher eat and think
    for (i=0; i<num; i++;){
      phil[i].start();
    }
  }
}
```

#### 무한루프임에도 실행되다가 중지가 되는데, 그 이유가 뭘까?

- `starvation`: 모든 철학자가 식사를 하지 못해 굶어죽는 상황
-> 이유: `교착상태(deadlock)` > 모든 사람들이 다 왼쪽 젓가락을 든 상태


#### 교착상태: Deadlocks

간혹 동기화를 하다보면 `Deadlock`에 빠지는 경우가 있다. (프로세스 관리에서 해결해야하는 문제중 하나!)

프로세스는 실행을 위해 여러 자원을 필요로 한다. (CPU, 메모리, 파일, 프린터 등..) 어떤 자원은 갖고 있으나 다른 자원은 갖지 못할때(다른 프로세스가 이미 사용중일때) 나머지는 대기를 해야한다. 그리고 다른 프로세스 역시 다른 자원을 가지려고 대기할 때 교착상태 가능성 > `이를 os가 잘못 나누어주면 교착상태에 빠진다.`

- 교착상태 필요조건(Necessary Conditions)
  - Mutual exclusive(상호배타): 하나가 사용하면 나머지는 기다려야한다.
  - Hold and wait(보유 및 대기): 하나 가지고 있으면서 나머지가꺼도 기다린다.
  - No Preemption(비선점): 순서를 강제할 수 없고 순서대로 이루어진다.
  - Circular wait(환형대기): 하나의 원을 만들어놓고 있다.


#### 자원(Resource)

Deadlock이 일어나는 이유는 결국 자원 때문이다. 자원(=하드웨어 자원)
자원을 할당받기 위해서는 os에 요청을 하고 요청이 올바르면 자원을 할당해주는데, 이를 다 사용하면 다시 os에게 반납을 하게 되는데, 이런식으로 자원을 사용하게 된다. 이 똑같은 자원이 여러개 있을 수도 있다.(여러개의 자원 = instance)

- 동일 자원
  - 동일 형식(type)자원이 여러개 있을 수 있다.
  - 예) 동일 CPU 2개, 동일 프린터 3개 등

- 자원의 사용
  - 요청(request) -> 사용(use) -> 반납(release)

- 자원 할당도(Resource Allocation Graph)
  - 어떤 자원이 어떤 프로세스에게 할당되었는가?
  - 어떤 프로세스가 어떤 자원을 할당받으려고 기다리고 있었는가?
  - 자원: 사각형, 프로세스: 원, 할당: 화살표
  - 교착상태가 되려면 이 그림이 원을 만들고 있어야 한다.
<center>
<figure>
<img src="/assets/post-img/OS/34.jpeg" alt="" width="50%">
</figure>
</center>

- 교착상태의 필요조건
  - 자원 할당도 상에 원이 만들어져야 한다(환형조건): 충분조건은 아님!
  - 피하려면: 짝수(오->왼), 홀수(왼->오)

<center>
<figure>
<img src="/assets/post-img/OS/35.jpeg" alt="" width="50%"><img src="/assets/post-img/OS/36.jpeg" alt="" width="50%">
</figure>
</center>

<!-- <center>
<figure>
<img src="/assets/post-img/OS/36.jpeg" alt="" width="50%">
</figure>
</center> -->
