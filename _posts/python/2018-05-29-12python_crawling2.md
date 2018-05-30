---
layout: post
title: python을 활용한 웹툰 크롤링 2단계 - BeautifulSoup
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.

<hr>

## 2단계 문제

제목, 저자, 웹툰정보 탐색하기


### BeautifulSoup 활용하기

html변수를 사용해 soup변수에 BeautifulSoup객체를 생성하고 soup객체에서

- 제목: 죽음에 관하여 (재)
- 작가: 시니/혀노
- 설명: 삶과 죽음의 경계선, 그 곳엔 누가 있을까의 내용을 가져와 title, author, description변수에 할당.


```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

big_title = soup.select_one('div.detail > h2')
# select: list로 반환하여, 전체를 다 찾는 방법
# select_one: 내가 원하는 태그, 클래스만 찾는 방법

# html파일 내 div 클래스 이름이 detail인 것 중 자식태그가 h2
print(big_title)
```

```
출력 결과물

<h2>
                                죽음에 관하여 (재)<span class="wrt_nm">
                                                        시니 / 혀노</span>
</h2>
```
```python
title = big_title.contents[0].strip()
# big_title의 1번째 요소, h2의 머리와 꼬리를 제거(strip())
author = big_title.contents[1].get_text(strip=True)
# big_title의 두번째 요소, span태그의 text만 가져올 때(.get_text(strip=True))
```

```
출력 결과물

죽음에 관하여 (재)
시니 / 혀노
```
