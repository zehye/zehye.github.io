---
layout: post
title: 페이지 교체 알고리즘(Page Replacement Algorithms)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 페이지 교체 알고리즘(Page Replacement Algorithms)

- FIFO(First-In First-Out)
- OPT(Optimal)
- LRU(Least-Recently-Used)


### FIFO(First-In First-Out)

메인메모리에 제일 먼저 올라온 애를 victim으로 선택

- Simplest idea: **초기화 코드는 더 이상 사용되지 않을 것**
- 예제
  - 페이지 참조열 = 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
  - # of frames = 3  # 남아있는 프레임의 수
  - 15 page fault

<center>
<figure>
<img src="/assets/post-img/OS/44.jpeg" alt="" width="50%">
</figure>
</center>

- **Belady's Anomaly**
  - page fault는 같은 프로그램 안에서 메모리가 작을 수록 자주일어나는데 프레임 수가 늘어남에도(=메모리 용량이 늘어난다) page fault가 늘어나는 현상
  - > 언제나 일어나는 것은 아니고 페이지 참조열이 특정한 상황에서 일어난다.
  - 프레임 수(=메모리 용량) 증가에 PF 회수 증가?

<center>
<figure>
<img src="/assets/post-img/OS/45.jpeg" alt="" width="50%">
</figure>
</center>



### OPT(Optimal)

앞으로 가장 사용되지 않을 것을 victim으로 선택 <br>
Rule: Replace the page that will not be used for the longest period of time

- 예제
  - 페이지 참조열 = 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
  - # of frames = 3  
  - 9 page fault

<center>
<figure>
<img src="/assets/post-img/OS/46.jpeg" alt="" width="50%">
</figure>
</center>

- Unrealistic: 미래는 알 수 없다.
  - cf. SJF CPU scheduling algorithms


### LRU(Least-Recently-Used)

최근에 가장 적게 사용된 것은 victim으로 선택 <br>
Rule: Replace the page that has not been used for the longest period of time <br>
-> 최근에 사용되지 않으면 나중에도 사용되지 않을 것

- 예제
  - 페이지 참조열 = 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
  - # of frames = 3  
  - 12 page fault

OPT보다는 자주일어나고 FIFO보다는 적에 일어난다. 그래서 대부분의 컴퓨터는 LRU를 사용한다.


#### Global vs Local Replacement

- Global replacement: 메모리 상의 모든 프로세스 페이지에 대해 교체
- Local replacement: 메모리 상의 자기 프로세스 페이지에 대해 교체
- 성능 비교: **Global replacement가 더 효율적** 일 수 있다.
