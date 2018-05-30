---
layout: post
title: python을 활용한 웹툰 크롤링 1단계 - os.path.exists, requests get parameters
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.

<hr>

## 1단계 문제
HTML 받아와서 html변수에 문자열을 할당한다.

### os.path.exists 활용하기
- (파이썬 공식 문서 확인 url: https://docs.python.org/3/library/os.path.html)

### requests get parameters
- data/episode_list.html파일이 없다면
- 죽음에 관하여 (재) 페이지를 requests를 사용해서 pycharm내 data/episode_list.html에 저장한다.
- 네이버 웬툰 url: http://comic.naver.com/webtoon/list.nhn?titleId=703845&weekday=wed
- list.nhn뒤 ? 부터는 url에 넣지 말고 get parameters로 처리한다.
- (requests문서의 'Passing Parameters In URLs' 확인 url: http://docs.python-requests.org/en/master/user/quickstart/#passing-parameters-in-urls)
- 저장 후에는 파일을 불러와 html 변수에 할당한다.

- 그리고 이미 data/episode_list.html이 있다면 html 변수에 파일을 불러와 할당한다.


```python
import requests
import os
# 내장함수 os를 불러오는 것 잊지말자

if os.path.exists('data/episode_list.html'):
  # HTML 파일이 로컬에 저장되어 있는지를 검사
  html = open('data/episode_list.html', 'rt').read()
  # 저장되어 있다면, 해당 파일을 읽어서 html변수에 할당

else:
  payload = {'titleID':'703845', 'weekday':'wed'}
  # HTTP요청시 전달할 GET parameters
  response = requests.get('http://comic.naver.com/webtoon/list.nhn', params=payload)
  # 저장되어 있지않다면 requests를 사용해 HTTP get요청
  html = response.text
  # 요청 응답 객체의 text속성값을 html변수에 할당
  with open('data/episode_list.html', 'wt') as f:
    f.write(html)
    # 받은 텍스트 데이터(html)를 로컬에 쓰기 모드로 가져옴
```


**강사님 코드**


```python
import requests
import os

file_path = 'data/episode_list.html'
# HTML 파일을 저장하거나 불러올 경로
url_episode_path = 'http://comic.naver.com/webtoon/list.nhn?titleId=703845&weekday=wed'
# HTTP 요청을 보낼 주소
params = {'titleID':'703845', 'weekday':'wed'}
# HTTP요청시 전달할 GET parameters

if os.path.exists(file_path):
  html = open(file_path, 'rt').read()

else:
  response = requests.get(url_episode_path, params)
  html = response.text
  open(file_path, 'wt').write(html)
```
