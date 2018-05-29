---
layout: post
title: python을 활용한 웹툰 크롤링 3단계 - nth-of-type
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.

<hr>

## 3단계 문제
에피소드 정보 목록을 가져오기

모든 에피소드의 목록들을 다 가져와 아래의 순서대로 나열한 뒤
url_thumnail: 썸네일 URL
title: 제목
rating: 별점
create_date: 등록일
no: 에피소드 상세페이지의 고유번호
각 에피소드들은 하나의 dict데이터로 만들어 모든 에피소드들을 list에 넣는다.

```python
# 각 정보들이 table속성 내 존재함으로
table = soup.select_one('table.viewList')
tr_list = table.select('tr')

for index, tr in enumerate(tr_list):
  if tr.get('class'):
    continue

  url_thumnail = tr.select_one('td:nth-of-type(1) img').get('src')

  from urllib import parse
  url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
  query_string = parse.urlsplit(url_detail).query
  query_dict = parse.parse_qs(query_dict)
  no = query_dict['no'][0]

  print(url_detail)
  print(query_string)
  print(query_dict)
  print(no)

  title = tr.select_one('td:nth-of-type(2) a').get_text(strip=True)
  print(title)

  rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
  print(rating)

  create_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)
  print(create_date)
  ```


```
출력 결과물

http://thumb.comic.naver.net/webtoon/703845/10/thumbnail_202x120_dd67c15f-af5e-4149-a71e-720b6ca388c7.jpg
/webtoon/detail.nhn?titleId=703845&no=10&weekday=wed
titleId=703845&no=10&weekday=wed
{'titleId': ['703845'], 'no': ['10'], 'weekday': ['wed']}
10
7화
9.98
2018.03.27
```
