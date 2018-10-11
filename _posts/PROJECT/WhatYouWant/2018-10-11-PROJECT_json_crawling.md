---
layout: post
title: 두번째 프로젝트, WhatYouWant Project (json 크롤링하기)
category: PROJECT
tags: [project, whatyouwant]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## json 크롤링하기

앞서 전시 내용과 디테일을 가져오기 위해서는 해당 전시에 대한 api주소만이 존재한다. postman을 통해서 api주소를 받아보면

```json
{
    "name": "스타일랩",
        "serviceName": "[대림미술관]나는 코코 카피탄, 오늘을 살아가는 너에게",
        "isImp": true,
        "isNmbRelated": false,
        "desc": "대림미술관은 2018년 8월 2일부터 2019년 1월 27일까지 세계적인 브랜드 및 매체가 주목하고 있는 ‘영 아트 스타(Young Art Star)’ 코코 카피탄(Coco Capitán)의 전시 ~\n",
        "addressJson": {
            "detail": null,
        }
}
```
이런식으로 도출되는 것을 알 수 있다.


### urllib.request

이렇게 나오는 데이터들은 우리가 기존에 사용했던 `beautifulsoup`이 아닌 `urllib.request`를 사용해야한다.

< 참고한 문서 및 블로그 >
- [참고문헌 python docs](https://docs.python.org/3.0/library/urllib.request.html)
- [calyfactory](https://calyfactory.github.io/%EC%A0%9C%EC%9D%B4%EC%8A%A8%ED%8C%8C%EC%8B%B1/)
- [thecoding](http://thecoding.kr/python-json-%ED%99%9C%EC%9A%A9/)

나는 일단 기본적으로 json파일을 저장해놓고 가져와서 쓰는 게 아니기때문에 `file open, read`방식을 사용할 수 없었다. (urllib사용한 이유)

### 활용방법

우선 가져오고자 하는 json 형식의 주소를 가져온다.

```python

# 이 주소는 가상의 주소다. 실제로 이 주소로 하게되면 []가 출력될 것이다.
response = urllib.request.urlopen('https://api.booking.naver.com/v3.0/businesses/')
data = json.load(response)
```

이렇게 해당 api 주소를 통해 파일을 가져와 볼 수 있다.

실제로 `print(data)` 를 하게되면 가져오려고 했던 json파일을 그대로 가져왔을 것이다.

이렇게 가져온 json을
```
name = data['name']
```
이런식으로 본인이 필요한 데이터들을 변수에 넣고 가져오는 게 가능하다.
