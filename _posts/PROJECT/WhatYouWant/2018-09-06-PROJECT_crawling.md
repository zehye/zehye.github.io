---
layout: post
title: 두번째 프로젝트, WhatYouWant Project (js형식 파일의 크롤링)
category: PROJECT
tags: [project, whatyouwant]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

<p class="message">
  내가 기존에 사용했던 매우 간편했던<br>
  개발자도구에서 파싱해오는 크롤링 방식이 통하지 않는단 걸 발견하고<br>
  ([] 세상에서 제일 슬픈 빈 리스트..)<br>
  새로운 방법을 터득해보았다.  
</p>

잘은 모르겠지만 자바스크립트를 통해 가져오는 파일의 경우 간혹 데이터가 넘어오지 않는다고 한다. 그래서 그런지

```
import requests
import BeautifulSoup

response = requests.get('https://swindow.naver.com/art/external/booking')
print(response.text)

>>> []
```
이렇게 빈 리스트만이 출력이 되는걸 볼 수 있었는데, 이러한 경우 `개발자도구 > Network`에서 넘어오는 값들을 하나하나 postman을 통해 넘어오는 값을 확인해본다면 언젠가는 찾아볼 수 있다.

너무나 비효율적으로 보일 수도 있겠으나 아직까지 이 방법이 내게는 최선이었다.

### 전시 리스트를 가져오는 주소

postman에서 `https://swindow.naver.com/art/external/booking/more`에 GET요청을 보내면 해당하는 리스트를 볼 수 있는데,

한가지 주의해서 봐야할 점은 한 요청당 10개의 게시글만 가져오는 것을 볼 수 있었고 이때 파라미터를 `formdata`의 `pagination`설정을 통해 원하는 페이지의 원하는 게시글들을 가져올 수 있도록 설정해주면 된다.

### 전시 디테일을 가져오는 주소

비교적 쉽게 찾았던 디테일 뷰

`https://api.booking.naver.com/v3.0/businesses/163950/biz-items?noCache=1536210087312&isDeleted=false&isImp=true`

<hr>

나중에 크롤링할 때 원하는 부분을 파싱해서 가져오면 될듯

혹시나 하는 마음에 해봤던 크롤링인데 의외의 복병을 처음 마주하게 돼서 당황했지만 클리어~
