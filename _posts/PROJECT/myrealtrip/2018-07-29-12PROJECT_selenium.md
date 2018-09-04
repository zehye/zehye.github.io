---
layout: post
title: 첫번째 프로젝트, My real trip (selenium 사용하기)
category: PROJECT
tags: [project, my real trip]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!     

<hr>

## Selenium 설치

```
pipenv install seleniem
pipenv install bs4
```

참고도 selenium은 업데이트가 잦기 때문에, 매번 새로운 버전의 selenium을 설치하는게 좋다고 한다. (더불어 bs4도 같이 깔아주자)

### webdriver
Selenium은 webdriver라는 것을 통해 디바이스에 설치된 브라우저들을 제어할 수 있다.

([chrome web driver install](https://sites.google.com/a/chromium.org/chromedriver/downloads))

위 폴더를 기준으로 할 경우 /Users/jihye/Downloads/chromedriver가 크롬드라이버의 위치다.
이 경로를 나중에 Selenium 객체를 생성할 때 지정해 주어야 한다.

참고로 django내에 넣어놓으면
```
driver = webdriver.Chrome('../chromedriver')
```
이렇게 설정해주어도 된다.
