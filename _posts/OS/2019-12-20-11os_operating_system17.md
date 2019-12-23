---
layout: post
title: 세그멘테이션(Segmentation)와 외부단편화 그리고 Paged segmentation
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 세그멘테이션(Segmentation)

프로세스를 논리적 내용(=세그먼트)으로 잘라서 메모리에 배치하는 것

- 프로세스는 세그먼트(segment)의 집합: 하나의 프로세스는 최소 3개 이상의 세그먼트로 구성
  - 프로세스는 기본적으로 code + data + stack으로 구성되어있다. 하나의 프로그램이 돌기 위해서는 이 3개로 구성된 뭉치가 있어야한다.
- 세그먼트의 크기는 일반적으로 같지 않다: 코드길이, 데이터길이, 스택길이가 다르고 각자의 길이 모두가 다르기 때문


### 세그먼트를 메모리에 할당!

MMU내의 재배치 레지스터 값을 바꿈으로써 CPU는 프로세스가 연속된 메모리 공간에 위치한다고 착각한다.<br>
MMU는 세그먼트 테이블(segment table)이 된다.


### 주소 변환(Address Translation)

- 논리주소(Logical address): cpu가 내는 주소는 segment 번호(s) + 변위(d)
- 주소변환: 논리주소 -> 물리주소(Physical address)
  - 세그먼트 테이블 내용: base + limit
  - 세그먼트 번호(s)는 세그먼트 테이블 인덱스 값
  - s에 해당되는 테이블 내용으로 시작 위치 및 한계값 파악
  - 한계(limit)를 넘어서면 segment violation 예외 상황 처리
  - 물리주소 = base[s] + d

segment table은 base(시작번지)와 limit(한계값)으로 이루어져있고, page table은 1차원 배열로 이루어진 반면 segment table은 2차원으로 이루어져있다.

- 예제
  - 논리주소 (2,100)은 물리주소 무엇인가? 4400
  - 논리주소 (1,500)은 물리주소 무엇인가? 6800(x) 해당주소 없음(limit를 넘어감)

limit | base
1000 | 1400
400 | 6300
400 | 4300
1100 | 3200
1000 | 4700


## 보호와 공유

**보호(Protection): 해킹 등 방지** <br>
모든 주소는 세그먼트 테이블을 경유하므로, 세그먼트 테이블 엔트리마다 r,w,x 비트를 부여한다.<br>
해당 세그먼트에 대한 접근 제어가 가능하고 이는 페이징보다 우월하다.

그 이유는 프로세스를 자를 때 페이징은 일정크기로 자르는데, 그렇게 되면 각 페이지에는 code, data, stack이 구분없이 자려져 있어 섞일 수 있다.<br>
그러나 세그먼트는 부위별로 자르기에 섞이지가 않다. code끼리, data끼리, stack끼리..

즉, 페이징은 한 페이지 안의 r,w,x를 구분짓기가 어려워진다는 의미

**공유(Sharing): 메모리 낭비 방지** <br>
같은 프로그램을 쓰는 복수개의 프로세스가 있다면, Code + data + stack에서 code는 공유가 가능하다.<br>
(단, non-self-modifying code = reentrant code = pure code인 경우) <br>
프로세스의 세그먼트 테이블 코드 영역이 같은 곳을 가리키게 하고 이는 페이징보다 우월하다.

그러나 대부분의 os는 페이징을 사용한다.


### Segmentation의 단점: 외부단편화(External Fragmentation)

세그먼트 크기는 고정이 아니라 가변적이다. 그렇기 때문에 크기가 다른 각 세그먼트를 메모리에 두려면 **동적 메모리를 할당** 해야한다.<br>
first, best, worst-fit, compaction등 문제

프로세스를 잘라서 메모리에 올리는 이유는 외부단편화를 없애기 위해서인데, 이를 해결해주지 못한다. -> 치명적인 문제


### Segmentation + Paging

**세그멘테이션은 보호와 공유면에서 효과적이고 페이징은 외부 단편화 문제를 해결해준다.** <br>
따라서 세그먼트를 페이징한다! = Paged segmentation, ex) Intel 80x86

1. 프로세스를 처음에는 세그먼트로 자른다. (code + data + stack)
2. 각 세그먼트를 페이지로 자른다.

이 또한 그러나 MMU를 두개를 거치게 됨으로 속도면에서는 조금 떨어진다는 단점이 존재한다. = trade off
