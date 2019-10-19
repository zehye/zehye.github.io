---
layout: post
title: 세번째 프로젝트, 각종 에러들 해결하는 방법
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>


## Error: That port is already in use.

`sudo lsof -t -i tcp:8000 | xargs kill -9`



## django.core.exceptions.ImproperlyConfigured: The SECRET_KEY setting must not be empty.

```
vi ~/.zchrc

export DJANGO_SETTINGS_MODULE=config.settings.local
export DJANGO_SETTINGS_MODULE=config.settings.dev
export DJANGO_SETTINGS_MODULE=config.settings.production

source ~/.zchrc
```
