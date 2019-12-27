---
layout: post
title: 파일할당(File Allocation)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

- 컴퓨터 시스템 자원 관리 **CPU: 프로세스 관리(CPU 스케줄링, 프로세스 동기화)**
  - 주기억장치: 메인 메모리 관리(페이징, 가상 메모리)
  - 보조기억장치: 파일시스템

- 보조기억장치(하드 디스크)
  - 하드디스크: track(cylinder), sector
  - Sector size = 512 bytes, cf. Block size: sector를 모아둔 것->block
  - 디스크는 블록 단위의 읽기/쓰기(block device)함 <-> character device
  - 디스크 = pool of free blocks, free blocks들의 모음
  - 각각의 파일에 대해 free block을 어떻게 할당해줄까?


## 파일할당

- 연속 할당(Contiguous Allocation)
- 연결 할당(Linked Allocation)
- 색인 할당(Indexed Allocation)


### 연속 할당(Contiguous Allocation)

각 파일에 대해 디스크 상의 `연속된 블록`을 할당(배열)

**장점** : 디스크 헤더의 이동 최고화 = 빠른 I/O 성능<br>
**단점** : 외부 단편화로 인한 디스크 공간의 낭비 발생

옛날 IBM VM/CMS 에서 사용했으며 동영상, 음악, VOD 등에 적합하다. <br>
파일의 크기가 큰 애들이 흩어져있다면 가져오는데 시간이 오래걸리지만 모여있으면 가져오는데에 시간이 적게걸린다.

- sequential access(순차접근): 순서적으로 읽기 가능
- direct access(직접접근): 특정 부분을 바로 읽기 가능
  - directory: 파일들의 정보를 모아둔 테이블


#### 연속 할당의 단점

파일이 삭제되면 hole이 생성되는데, 이렇듯 파일의 생성/삭제가 반복되면 곳곳에 hole들이 흩어져있게 된다.
그렇다면 새로운 파일을 어느 hole에 둘 것인가에 대한 고민이 생기고 이때 **외부단편화** 문제가 발생하게 된다

+ 디스크의 compaction은 가능하지만 시간이 굉장히 오래 걸린다.

즉, 외부 단편화로 인한 디스크 공간의 낭비 발생한다.

더 나아가, 사실 파일 생성 당시 파일의 크기를 알 수 없다. 즉, 파일을 어느 hole에 넣어야 할 지 알 수가 없다.<br>
더 나아가 파일의 크기가 계속 증가할 수 있다(log file) -> 기존의 hole 배치로는 불가!


### 연결 할당(Linked Allocation)

파일을 연속적으로 나누는게 아니고, 자료구조의 linked list로 바라봄으로써 아무데나 들어갈 수 있다.

file = linked list of data blocks

파일 디렉토리(directory)는 제일 처음 블록을 가리킨다.<br>
각 블록은 포인터 저장(다음 블록은 어디있는지)을 위한 4바이트 또는 이상을 소모한다.

**새로운 파일 만들기**<br>
1. 비어있는 임의의 블록을 첫 블록으로
2. 파일이 커지면 다른 블록을 할당 받고 연결
-> **장점** :외부 단편화 발생하지 않음


#### 연결 할당의 단점

순서대로 읽기(sequential access)만 가능하며, direct access(중간부터 읽기) 불가하며 포인터 저장 위해 4바이트 이상 손실한다.

- 낮은 신뢰성: 포인터 끊어지면 이하 접근 불가하다
- 느린 속도: 헤더의 움직임


#### 연결 할당의 개선점: FAT 파일 시스템

File Allocation Table 파일 시스템으로 MS-DOS, OS/2, Windows등에서 사용한다.

연결할당의 변종으로 포인터들만 모은 테이블(FAT)을 별도 블록에 저장하고 FAT 손실 시 복구를 위해 이중 저장하는 시스템을 갖는다.
이는 direct access도 가능하며(FAT내용을 가져와 헤더를 움직임) FAT는 일반적으로 메모리 캐싱을 한다.


### 색인 할당(Indexed Allocation)

연결할당과 같이 아무데에나 파일을 할당할 수 있고, 그 정보를 저장하는 블락을 할당

데이터 블럭 외에 각 파일 당 한 개의 인덱스 블록이 존재한다. <br>
인덱스 블록은 포인터의 모음이며 디렉토리는 인덱스 블록을 가리킨다. Unix/Linux 등에서 사용한다.

**장점** : direct access 가능, 외부 단편화 없음<br>
**단점** : 인덱스 블록 할당에 따른 저장공간 손실<br>
-> 예: 1바이트 파일 위에 데이터 1블록 + 인덱스 1블록


#### 파일의 최대 크기

- 예제: 1블록 = 512 bytes = 4bytes x 128개의 인덱스 = 128*512bytes = 64KB
- 예제: 1블록 = 1KB = 4bytes x 256개의 인덱스 = 256*1KB = 256KB
- 해결방법: Linked, Multilevel index, Combined 등
  - Linked: 인덱스블락이 다른 인덱스 블락을 가리키도록 함
  - Multilevel index: 블락을 여러개를 둠
  - Combined: Linked와 Multilevel index를 합친 것
