---
layout: post
title: 운영체제의 주요서비스 - 프로세스, 메모리, 파일관리, 시스템호출
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

<center>
<figure>
<img src="/assets/post-img/OS/11.jpeg" alt="" width="50%">
</figure>
</center>

application: 하드웨어의 자원을 사용<br>
hardware: resource<br>
os: 하드웨어 자원을 효율적으로 어플리케이션에게 나누어줌

이런 os에는 여러가지 관리들이 있는데

- process management(cpu, 프로세서 관리)
- memory management(메인메모리 관리)
- file management(하드디스크)
- IO management
- networking
- protection..

그러면 각 부서에서는 무슨일들을 하는가?


## 1. 프로세스 관리(Process Management)

프로세스: 실제 메인 메모리에서 실행 중인 프로그램 (program in execution)

cpu에 의해서 실행되는 프로그램은 메인 메모리 위로 올라와있는 프로그램이니까 cpu와 관련해서는 프로그램관리가 아닌 프로세스 관리라고 한다. 이들은 os의 프로세스 관리 부분이 처리해준다.

- 프로세스의 생성, 소멸(creation, deletion)
- 프로세스 활동 일시 중지, 활동 재개(suspend, resume)
- 프로세스 간 통신 (interprocess communication, IPC)
- 프로세스 간 동기화 (synchronization)
- 교착상태 처리(deadlock handing


## 2. 주기억장치 관리(Main Memory Management)

- 프로세스에게 메모리 공간 할당(allocation)
- 메인메모리의 어느 부분이 어느 프로세스에게 할당되었는가를 추적/감시
- 프로세스 종료 시 메모리 회수 (deallocation)
- 메모리의 효과적 사용
- 가상메모리: 물리적 실제 메모리보다 큰 용량 갖도록 해줌
  - 일반적으로 메인메모리는 보조기억장치에 비해서 용량이 작다. 실제 메모리는 조금밖에 없지만 크게 보이도록 하는 기술이 가상메모리임


## 3. 파일관리 (File Management)

Track/Sector로 구성된 디스크를 파일이라는 논리적 관점으로 보게 관리

- 파일의 생성과 삭제(file creation/deletion)
- 디렉토리(directory)의 생성과 삭제(또는 폴더 folder)
- 기본동작지원: open, close, read, write, create, delete
- track/sector - file 간의 매핑(mapping)
- 백업(backup)

## 4. 보조기억장치 관리 (Secondary Storage Management)

<center>
<figure>
<img src="/assets/post-img/OS/12.jpeg" alt="" width="50%">
</figure>
</center>

하드디스크, 플래시 메모리 등

- 빈공간 관리(free space management): 처음에는 블락들이 하나도 사용안되고 있다가 사용된다면 그 해당 공간들을 관리
- 저장공간 할당(storage allocation): 3개의 블락이 필요하다면 비어있는 공간중에 어느 블락을 사용할것인지 처리
- 디스크 스케줄링(disk scheduling): 블락들이 흩어져있는데 헤드를 많이 움직이면 시간이 많이 걸릴테니 최소한의 움직임으로 원하는 track/sector를 읽어올 것인가를 결정

## 5. 입출력장치 관리 (I/O Device Management)

장치 드라이브 (Device driver), 장치를 사용하기 위한 드라이브가 필요한데 이 드라이브는 os에 포함되어있다.

- 입출력 장치의 성능향상: buffering, caching, spooling
  - buffering: 입출력장치에서 읽은 내용을 일단 메모리에 들고옴, 그래야 나중에 또 그 파일을 쓸때 시간을 줄여서 사용할 수 있다.
  - caching: buffering과 비슷
  - spooling: 메모리 대신 하드디스크를 중간매체로 사용. 프린트로 글자를 찍는다면 프린트는 속도가 느리니까 일단 하드디스크에 저정하고 그 내용을 프린트에 보냄으로써 그 사이 cpu는 다른일을 할 수 있도록 한다.


## 시스템 콜(System calls)

<center>
<figure>
<img src="/assets/post-img/OS/13.jpeg" alt="" width="50%">
</figure>
</center>

일반 애플리케이션 서비스가 os가 제공하는 서비스를 받기 위해서 호출하는것. 즉, 운영체제 서비스를 받기 위한 호출을 의미한다.

- Process: end, abort(강제종료), load(하드디스크의 프로그램을 메모리에 가져오는 것), execute, create(프로세스를 만들고), terminate(=end), get/set, attributes, wait event, signal event
- Memory: allocate, free(메모리 되돌려줌)
- File: create, delete, open, close, read, write, get/set attributes
- Device: request, release, read, write, get/set attributes, attach/detach devices
- Information: get/set time, get/set system data
- communication: socket, send, receive

<순서><br>

EAX: 해당하는 명령<br>
ECX = attributes<br>
EBX = file name<br>
swi<br>

-> 원하고자 하는 파일이 생성됨<br>

일반 어플리케이션이 os에 요청하는 것을 `system call` 이라고 하고, 시스템 콜은 일반적으로 특정 레지스터에 특정 값을 건 후에 swi를 걸어봄으로써 서비스를 받을 수 있다.
