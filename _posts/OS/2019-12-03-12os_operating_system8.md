---
layout: post
title: 프로세스 생성과 소멸
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
