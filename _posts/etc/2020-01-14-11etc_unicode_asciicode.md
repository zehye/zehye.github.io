---
layout: post
title: 아스키코드와 유니코드(ASCII, Unicode)
category: etc
tags: [ascii code, unicode]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!  

<hr>

## 들어가기에 앞서...

- 컴퓨터의 기본 저장 단위는 바이트(byte)이다.
- 1바이트는 8비트(bit)이다.
- 1바이트에는 2의 8승에 해당하는 256개의 고유한 값을 저장할 수 있다.
- 문자나 기호들의 집합을 컴퓨터에 저장하거나, 통신 목적으로 사용할 경우에는 부호로 바꿔야한다.
  - 이를 **'문자인코딩(encoding)'** 또는 **'부호화'** 라고 하며 부호화된 문자를 복원하는 것을 **'복호화'** 라고 한다.
- 모스부호도 일종의 문자 인코딩이다.


## 아스키(ASCII) American Standard Code for Information Interchange

아스키코드는 7비트, 증 128개의 고유한 값만 사용한다.

7비트만 활용하는 이유는 1비트를 통신 에러 검출을 위해 사용하기 때문이라고 한다. >> **Parity Bit**

<center>
<figure>
<img src="/assets/post-img/etc/ascii.png" alt="" width="80%">
</figure>
</center>

위 이미지를 통해 0~127까지 각각 고유한 값이 할당되어 있는 것을 알 수 있다.

- 0~32: 인쇄와 전송 제어용으로 사용됨
- 33~126: 숫자, 알파벳 대/소문자, 특수기호 등이 할당

이렇게 0~127까지 총 128개의 고유값을 저장하고 있다.

이는 영문 키보드로 입력할 수 있는 모든 가능성을 다 담고 있지만, 이 외의 다른 언어를 표현하기에는 7비트로는 부족하다는 것을 알 수 있다.

그래서 이를 보완하기 위해 용량을 더 크게 확장한 유니코드가 등장하게 되었다.


## 유니코드(Unicode)

한글은 자음과 모음의 조합 가능 개수만 따져도 128개는 가뿐히 넘는다.

즉, 아스키 코드의 한계를 극복하기 위해 유니코드가 등장하게 되었다.
