---
layout: post
title: 페이지 교체(Page Replacement)
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 페이지 교체(Page Replacement)

메모리가 가득차게되면 페이지를 교체해줘야 한다.

- Demand Paging: 요구되어지는 페이지만 backing store에서 가져온다.
  - 프로그램 실행 계속에 따라 오구페이지가 늘어나고, 언젠가는 메모리가 가득차게 된다.

- Memory full > victim page
  - **page-out**: 메모리가 가득하면 추가로 페이지를 가져오기 위해 어떤 페이지는 backing store로 몰아낸다.
  - **page-in**: 그 빈 공간으로 페이지를 가져온다.


### Victim Page, 어느 페이지를 몰아낼 것인가?

- I/O 시간 절약을 위해
- 기왕이면 **modify 되지 않은 페이지** 를 victim으로 선택: modified bit(= dirty bit)

<center>
<figure>
<img src="/assets/post-img/OS/43.jpeg" alt="" width="50%">
</figure>
</center>

**modified bit인 여러 페이지 중에서도 무엇을 victim으로 할 것인가?**

- Random
- First-In / First-Out(FIFO)
- 그외
- page replacement algorithms


### 페이지 참조 열(Page reference string)

페이지 번호중에서 바로 이어지는 숫자는 스킵하고 넘어가는 것

- cpu가 내는 주소: 100 101 102 432 612 103 104 611 612
- page size = 100바이트라면
- 페이지 번호 = 1 1 1 4 6 1 1 6 6
- **page reference string = 1 4 6 1 6**
