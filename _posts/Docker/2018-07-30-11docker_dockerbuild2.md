---
layout: post
title: DockerBuild 2 (ebCLI)
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## EB CLI 설치하기
```
pipenv install awsebcli --dev
```

### requirements.txt 만들기

설치 후 아래를 참고해 requirements.txt를 생성해보자
```
pipenv lock requirements
pipenv lock requirements --dev
```

### Dockerfile.base

```docker
FROM          python:3.6.5-slim
MAINTAINER    zehye.01@gmail.com

run           apt -y update && apt -y dist-upgrade

COPY          ./requirements.txt    /srv/
RUN           pip install -r /srv/requirements.txt
```
로컬의 requirements.txt파일을 /srv에 복사 후 pip install을 한다.

이렇듯 build하는 환경에 requirements.txt가 있어야 한다.

### Dockerfile.local

```docker
FROM            eb-docker-study:base
MAINTAINER      zehye.01@gmail.com


ENV             BUILD_MODE                  local
ENV             DJANGO_SETTINGS_MODULE      config.settings.${BUILD_MODE}

COPY            .   /srv/project

```

#### build.py

requirements.txt를 pipenv를 통해서 만드는데, 왜냐하면 pipenv라는 명령어를 쓰는 건 도커 이미지 밖에서 미리 만들어져야 하는데, 그래서 만들어진 requirements.txt 파일이 존재하는 상태에서 base 이미지를 빌드한다.

base 이미지를 빌드한 후에는 requirements.txt가 필요없어지니까 다시 삭제한다.

고로 build.py는 local 이미지를 만들기 위해서 존재한다.

```python
#!/usr/bin/env python

import subprocess
import os

try:
  # pipenv lock으로 requirements.txt생성
  subprocess.call('pipenv lock --requirements > requirements.txt')
  # docker build 실행
  subprocess.call('docker build -t eb-docker-new:base -f Dockerfile.base .')

finally:
  # 끝난 후 requirements.txt파일 삭제
  os.remove('requirements.txt')
```
도커파일을 가지고 무언갈 생성하기 requirements.txt를 만들고 설치를 하고 생성이 다 끝난 뒤에는 다시 이를(requirements.txt) 지워줘야 한다. 그래서 build.py를 만들고 python으로 이 스크립트를 실행하도록 한다.

파이썬에서 외부 쉘에서 이를 실행하고 싶을 때에는, 가장 쉽게 사용하는게 `subprocess`이다.

이제 이를 실행시키기 위해 권한을 부여한다.

```
chmod 755 build.py
./build.py
```

> docker run --rm -it -p 9999:8000 eb-docker-new:local python /srv/project/app/manage.py runserver 0:8000
