---
layout: post
title: 크롤링한 데이터 데이터프레임으로 만들어보기
category: DA
tags: [DA]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## beautifulSoup 사용

(정말 몇년만에 사용해보는..뷰티풀 숲인지 모르겠다.)

웹 크롤링을 굉장히 편리하게 할 수 있는 라이브러리이다. response.text를 통해 가져온 html 문서를 탐색해 원하는 부분을 뽑아내는 역학을 도와준다. 

```python
pip install beautifulsoup4
```


### 사용해보기(명언)

[명언 홈페이지](https://quotes.toscrape.com/)

```python
from bs4 import beautifulsoup
import requests 

url = 'https://quotes.toscrape.com/'
# url로부터 요청을 받아 reponse에 담음 
response = requests.get(url)

# response에 들어온 html 텍스트를 파싱
soup = BeautifulSoup(response.text, 'html.parser')
# div 태그이면서 class 명이 quote인 모든 친구들을 찾음
quote_div = soup.find_all('div', class_='quote')

for quote_data in quote_div:
    # span태그이면서 class 명이 text인 에들의 텍스트 
    quote_text = quote_data.find('span', class_='text').text
    quote_author = quote_data('small', class_='author').text
    # a 태그안의 href
    quote_url = quote_data('a').get('href')

# 혹은 
quote_text = soup.select('div.quote>span.text')
quote_text_list = [i.text for i in quote_text]

quote_author = soup.select('div.quote>small.author')
quote_author_list = [i.text for i in quote_author]
```



> find와 select의 차이 

1. find: 내가 찾을 html 태그를 직접 입력 
    - soup.find_all('div', class_='quote')
2. select: css 선택자를 통해 html 태그를 찾음 
    - soup.select('div.quote>span.text')


### 데이터프레임으로 만들어보기

```python
from bs4 import beautifulsoup
import requests 

url = 'https://quotes.toscrape.com/'
response = requests.get(url)

soup = BeautifulSoup(response.text, 'html.parser')
quote_div = soup.find_all('div', class_='quote')

text_list = []
author_list = []
url_list = []

for quote_data in quote_div:
    quote_text = quote_data.find('span', class_='text').text
    text_list.append(quote_text)
    quote_author = quote_data.find('small', class_='author').text
    text_list.append(quote_author)
    quote_url = quote_data.find('a').get('href')
    text_list.append(quote_url)
```

```python
import pandas as pd

pd.DataFrame({
    'text': text_list, 
    'author': author_list,
    'url': url_list
})
```


### 테이블 데이터를 크롤링해보기

```python
import pandas as pd

url = 'https://en.wikipedia.org/wiki/List_of_countries_by_stock_market_capitalization'
table_data = pd.read_html(url)

table_data[0]
table_data[0].head()  # 이렇게도 가능하다 
```

이때 table_data안에는 각 테이블들의 데이터가 리스트 안에 들어가 있기 때문에 인덱스틀 통해 테이블 데이터를 가져와서 쓸 수 있다. 