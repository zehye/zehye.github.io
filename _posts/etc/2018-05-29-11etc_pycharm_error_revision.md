---
layout: post
title: pip list출력 포맷 변경
category: etc
tags: [python, pycharm]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## pip list출력 포맷 변경

pip-list 시 --format=(legacy|columns)과 같 경고가 나오는 경우 (아래와 같은)
> DEPRECATION: The default format will switch to columns in the future. You can use --format=(legacy|columns) (or define a format=(legacy|columns) in your pip.conf under the [list] section) to disable this warning.

그런데 찾아보니
> You are using pip version 9.0.3, however version 10.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.

단순 업그레이드의 문제였다.

zsh에

```
pip install --upgrade paginate_path
```

Package    Version
---------- -------
Django     2.0.5  
pip        10.0.1
pytz       2018.4
setuptools 39.0.1

정상 작동이 되는 것을 알 수 있다.
단순 업그레이드의 문제라면 이런식으로 해결하면 되는 것 같다.
