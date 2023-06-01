---
layout: post
title: postgresql settings for macOS
category: DATABASE
tags: [DATABASE, postgresql]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

맥에서 postgresql를 설치하는 방법은 매우 간단하다. > homebrew가 있기 때문이다. <br>
만약 homebrew가 깔려있지 않다면 우선 깔아주고 시작하자.

참고 >[homebrew install](https://brew.sh/index_ko)

터미널에서

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

homebrew 설치가 완료되고나면 터미널창 새창을 열고

```
brew search postgresql
brew install postgresql
```

```
brew services start postgresql
psql postgres
psql (11.5)
Type "help" for help.

postgres=#
```
완료!

나가고 싶다면

```
brew services stop postgresql
```
