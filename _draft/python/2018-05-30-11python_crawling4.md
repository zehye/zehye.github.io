---
layout: post
title: python을 활용한 웹툰 크롤링 4단계 - os.path.exists, requests get parameters
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.

<hr>

```python
import os

from bs4 import BeautifulSoup
import requests

file_path = 'data/new_episode_list.html'
url_episode_path = 'http://comic.naver.com/webtoon/list.nhn?titleId=703845&weekday=wed'
params = {'titleID':'703845', 'weekday':'wed'}

if os.path.exists(file_path):
    html = open(file_path, 'rt').read()

else:
    response = requests.get(url_episode_path, params)
    html = response.text
    open(file_path, 'wt').write(html)

soup = BeautifulSoup(html, 'lxml')
title = soup.select_one('div.detail > h2')
print(title)
real_title = title.contents[0].strip() # 지정된 문자열의 머리와 꼬리를 제거하는 데 사용됩니다.

print(real_title)
author = title.contents[1].get_text(strip=True) # tag로부터 문자열을 가져올때는 get_text를 사용한다. (작가정보가 span tag)

print(type(title))
print(author)

table = soup.select_one('table.viewList')
# print(table)
tr_list = table.select('tr')
#print(tr_list)

for index, tr in enumerate(tr_list[1:]):
    # print('==={}===\n{}\n'.format(index, tr))
    if tr.get('class'):
        continue

    url_thumnail = tr.select_one('td:nth-of-type(1) img').get('src')
    print(url_thumnail)

    from urllib import parse
    url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
    query_string = parse.urlsplit(url_detail).query
    query_dict = parse.parse_qs(query_string)
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
