---
layout: post
title: DockerBuild 1
category: Docker
tags: [Docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## EB CLI

> [EB CLI](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-configuration.html)

EB CLI는 Elasticbeanstalk을 사용할 수 있게 해주는 커맨드라인 인터페이스를 뜻한다.

### User만들기 (IAM)

> [AWS](https://aws.amazon.com/ko/)
> IAM - 사용자 - 사용자 추가

- 사용자 이름 설정
- 프로그래밍 방식 액세스 표시
- 기존정책 직접 연결
- AWSElasticBeanstalkFullAccess
- 해당 액세스키 ID, 비밀 액세스 키 credentials에 추가 (vi .aws/credentials)

```
[<IAM 이름>]
aws_access_key_id =
aws_secret_access_key =
```

### Dockerfile

최소단위로 만들어보자(우분투는 용량이 너무 크다)

#### Dockerfile.base

```Dockerfile
FROM          python:3.6.5-slim
MAINTAINER    <이메일>

RUN           apt -y update && apt -y dist-upgrade
```

Dockerfile을 만들기 전에 우선 Dockerfile.base를 통해 미리 확인을 해본다.

```shell
docker build -t eb-docker-new:base -f Dockerfile.base .
```

#### Dockerfile.local

```Dockerfile
FROM          eb-docker-new:base
MAINTAINER    <이메일>

COPY          .   /srv/project
```

기본적으로 Dockerfile.base와 Dockerfile.local의 차이는, base의 이미지는 공개되어도 상관없는 정보라고 생각하는 것이 좋다. COPY명령어를 실행시키는 순간, 우리한테 있는 secret이나 코드들이 전부 들어가게 되는데, 이 정보들은 base이미지에는 들어가면 안된다.

**base이미지는 오로지 환경설정에 있어서만 존재해야 한다.**

이제 만든 이미지안으로 들어가보자.

```shell
docker build -t eb-docker-new:local -f Dockerfile.local .

export DJANGO_SETTINGS_MODULE=config.settings.local
docker run --rm -it -p 9999:8000 eb-docker-new:local /bin/bash
```

```
root@<도커이미지>:/# cd srv/project

# 이미지 컨테이너 안에 runserver를 실행시키기 위해 django를 설치한다.
root@<도커이미지>:/srv/project# pip install django
root@<도커이미지>:/srv/project/app# python manage.py runserver 0:8000
```

runserver가 실행되는 것을 확인해본다!
