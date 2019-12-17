---
layout: post
title: 메모리의 낭비를 없애는 방법, 메모리 절약(동적적재, 동적연결, 스와핑)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 메모리 낭비방지

- Dynamic Loading
- Dynamic Linking
- Swapping


### 동적적재(Dynamic Loading)

loading: 어플리케이션(만들어진 실행파일)을 메인메모리로 올리는 것(적재하는 것)<br>
**프로그램 실행에 반드시 필요한 루틴/데이터만 적재**

- 모든 루틴(routine, 함수, 프로시저)이 다 사용되는 것은 아니다(ex.오류처리)
- 모든 데이터(data)가 다 사용되는 것은 아니다(ex.배열)
- 자바: 모든 클래스가 다 사용되는 것은 아니다.
- `실행 시 필요하면 그때 해당 부분을 메모리에 올린다` cf. <->정적적재(static loading)


### 동적연결(Dynamic Linking)

여러 프로그램에 공통 사용되는 라이브러리(네트워크 관련된 라이브러리(ftp, 이메일..), printf.o 기계어코드 등..)

- 공통 라이브러리 루틴(library routine)을 메모리에 중복으로 올리는 것은 낭비
- `라이브러리 루틴 연결을 실행 시까지 미룬다`
- 오직 하나의 라이브러리 루틴만 메모리에 적재되고 다른 애플리케이션 실행 시 이 루틴과 연결(link)된다. cf. <->정적연결(static linking)
- 공유 라이브러리(shared library, .so) - linux 또는,
- 동적 연결 라이브러리(Dynamic Linking Library, dll) - Windows

즉, 원래는 실행파일(exe file)을 만들기 전에 링크가 되는데(=정적연결), 링크를 미룬다. 일단 로드된 다음에(p1, p2가 메모리에 올라온 다음에) 링크를 한다.<br>
p1과 p2 둘다 공통적으로 코드 내에 printf를 가지고 있다고 한다면, p1을 실행하려할 때 하드디스크에서 printf를 메인메모리에 가져온다.
printf가 메모리에 올라오면 p1과 p2가 실행될때 그때그때 메인메모리에 있는 printf를 링크를 하여 사용한다.


### Swapping

**메모리에 적재되어 있으나 현재 사용되지 않고 있는 프로세스 이미지를 하드디스크의 특정부분으로 몰아냄**<br>
(컴퓨터 한참 사용하다가 중단하는 경우, 그때에도 내 프로그램은 여전히 메모리에 남아있는데 이 부분도 낭비이다.)

<center>
<figure>
<img src="/assets/post-img/OS/38.jpeg" alt="" width="30%">
</figure>
</center>

- 메모리 활용도를 높이기 위해 `Backing store(swap device, 하드디스크의 일부)` 로 몰아내기
- swap-out(몰아내기) vs swap-in(가져오기)
- Relocation register 사용으로 적재 위치는 무관
- 프로세스 크기가 크면 backing store 입출력에 따른 부담이 크다.
