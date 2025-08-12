---
layout: post
title: __name__ , __main__
category: Python
tags: [python, 크롤링]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## name변수

`__name__` 변수는 현재 실행중인 모듈의 이름을 뜻한다.

우리가 interpreter를 통해 어떤 모듈을 실행시키면 그 모듈의 이름은 `__name__`이다.

interpreter로 실행시켰다는 것은 python ~~.py로 실행시킨 것을 의미한다. name변수를 통해 모듈을 실행시키게 되면 해당하는 함수만을 사용하게 된다. = `__main__`

그 외의 모듈이 불리는 경우는 import가 되어있는 경우만이 해당된다.

import가 되어있다면 굳이 `__name__`=`__main__`을 사용하지는 않는다.
