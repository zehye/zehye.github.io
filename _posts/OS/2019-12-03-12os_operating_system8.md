---
layout: post
title: 프로세스 생성과 소멸, 쓰레드
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 프로세스의 생성과 소멸

### Process Creation

- 프로세스는 프로세스에 의해 만들어진다. (init process에 의해)
  - 부모 프로세스(Parent process)
  - 자식 프로세스(Child process)
    - cf. Sibling process
  - 프로세스 트리 (process tree)

<center>
<figure>
<img src="/assets/post-img/OS/29.jpeg" alt="" width="50%">
</figure>
</center>

- Process Identified(PID): 프로세스 번호, 중복되지 않음, 그 번호의 프로세스가 죽기전까지는 그 번호를 재할당못함
  - Typically an integer number: 전형적으로 정수형 숫자
  - cf. PPID: 부모의 할당 ID

- 프로세스 생성
  - fork(): 새로운 프로세스를 생성하는 system call
  - exec(): 만들어진 프로세스로 하여금 실행하도록 하기 위해서 실행파일을 메모리로 가져오기 위해서 복사

ps: process state / ps -a <br>
UID: User ID <br>
PID: Process ID <br>
PPID: Parent Process ID


### Process Termination

- 프로세스 종료
  - exit() system call: 해당 시스템 콜을 호출하면 프로세스가 종료됨
  - 해당 프로세스가 가졌던 모든 자원(메모리, 파일, 입출력 장치 등..)은 OS에게 반환: 이 OS는 이 자원을 필요로 하는 애한테 다시 보내줄 것이다.


## 쓰레드 (Thread)

프로그램 내부의 흐름, 맥

```java
class Test {
  public static void main(Sting[] args) {
    int n = 0;
    int m = 6;

    System.out.println(n+m);

    while (n < m)
    n++;

    System.out.println("Bye");
  }
}
```

하나의 프로그램은 하나의 맥이 있고 이러한 맥을 쓰레드라고 한다.


### Multithreads

- 다중 쓰레드(Multithreads)
  - 한 프로그램에 2개 이상의 맥
  - 맥이 빠른 시간 간격으로 스위칭 된다 => 여러 맥이 동시에 실행되는 것처럼 보인다. (`concurrent` vs simultaneous)

동시에 돌 수 있는 이유는 맥이 빠른시간으로 스위칭 되기 때문이다. 그래서 실제로 같이 하고 있는 것처럼 보이지만 사실 CPU는 하나이기 때문에 우리는 하나만 사용하고 있다. 우리는 일반적으로 concurrent!

- 예: Web breoser
  - 화면 출력하는 쓰데르 + 데이터 읽어오는 쓰레드

- 예: Word Processor
  - 화면 출력하는 쓰레드 + 키보드 입력 받는 쓰레드 + 철자/문법 오류 확인 쓰레드

- 예: 음악 연주기, 동영상 플레이어, Eclipse IDE...

**실제 context switching 되는 단위는 process단위가 아니라 thread단위이다!**


## Thread vs Process

- 한 프로세스에는 기본 1개의 쓰레드: 단일 쓰레드(single thread) 프로그램
- 한 프로세스에 여러개의 쓰레드: 다중 쓰레드 (multi-thread) 프로그램

- 쓰레드 구조
  - 프로세스의 메모리 공간을 공유한다 (code, data)
  - 프로세스의 자원공유(file, io..)
  - 비공유: 개별적인 PC(program counter), SP(stack pointer), Registers, stack
    - CPU가 p1에 있는 각각의 쓰레드가 가지고 있는 PC값은 다를 것이다. 그래서 스위칭 될때마다 각자가 가리키고 있는 값이다르니 이 값들은 공유하지 않는다.

- 프로세스의 스위칭 vs 쓰레드의 스위칭
: 처음에는 프로세스만 스위칭 되었지만 이제 모든 운영체제는 `context switching 단위가 쓰레드간의 단위이다.`
