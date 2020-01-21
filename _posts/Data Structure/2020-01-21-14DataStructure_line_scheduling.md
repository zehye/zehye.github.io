---
layout: post
title: Data Structure, Assembly Line Scheduling
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Assembly Line Scheduling

실생활에서 Dynamic Programming이 쓰일때는 언제일까? >> 생산공정을 생각해보자

<center>
<figure>
<img src="/assets/post-img/DataStructure/16.jpeg" alt="" width="80%">
</figure>
</center>


<center>
<figure>
<img src="/assets/post-img/DataStructure/17.jpeg" alt="" width="80%">
</figure>
</center>

- Memoization을 통해 minimum tracel time을 확인해 볼 수 있다.
- retrace table을 통해 어떤 선택지를 하였는지 저장을 하여 공정 루트를 만들어낼 수 있었다.
