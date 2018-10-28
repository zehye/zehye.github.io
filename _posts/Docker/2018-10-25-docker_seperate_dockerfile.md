---
layout: post
title: dockerfile 분리하기 (base, local, dev, production)
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## docker 기능별로 분리

### Dockerfile.base

```docker
FROM            python:3.6.5-slim
MAINTAINER      zehye.01@gmail.com

RUN             apt -y update && apt -y dist-upgrade
RUN             apt -y install build-essential
RUN             apt -y install nginx supervisor

# 로컬의 requirments.txt파일을 /srv에 복사 후 pip install 실행
# (build하는 환경에 requirments.txt가 있어야함!)
COPY            ./requirements.txt   /srv/
RUN             pip install -r /srv/requirements.txt
```

### Dockerfile.local

```docker
FROM            eb-docker-study:base
MAINTAINER      zehye.01@gmail.com


ENV             BUILD_MODE                  local
ENV             DJANGO_SETTGINS_MODULE      config.settings.${BUILD_MODE}


COPY            .   /srv/project

WORKDIR         /srv/project/app
CMD             python manage.py runserver 0:8000
```

### Dockerfile.dev

```docker
FROM                eb-docker-study:base
MAINTAINER          zehye.01@gmail.com

ENV                 BUILD_MODE                  dev
ENV                 DJANGO_SETTINGS_MODULE      config.settings.${BUILD_MODE}

# requirements.txt를 새로 설치하는 이유는 build.py에서 build_dev를 새로 만들었는데
# requirements.txt를 만들어주는 명령어가 다르다 (requirements.txt -- dev)
# local과 production에서는 최소한의 패키지만을 설치하지만 dev에서는 최대로 설치를 하게 된다.
# 따라서 build_dev에서 만든 패키지는 base이미지에 추가가 되어야 하기 때문에 새로 COPY해줘야 한다.
COPY                ./requirements.txt          /srv/
RUN                 pip install -r /srv/requirements.txt

COPY                .       /srv/project

# nginx설정파일들 복사 및 enabled로 링크
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/nginx.conf \
                        /etc/nginx/nginx.conf && \
                cp -f   /srv/project/.config/${BUILD_MODE}/nginx_app.conf \
                        /etc/nginx/sites-available/ && \
                rm -rf  /etc/nginx/sites-enabled/* && \
                ln -sf  /etc/nginx/sites-available/nginx_app.conf \
                        /etc/nginx/sites-enabled/

# supervisor설정 복사
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/supervisor.conf \
                        /etc/supervisor/conf.d/
# supervisord 실행
CMD             supervisord -n
```
nginx 설정파일들을 복사할 경우 nginx와 nginx_app의 오타가 자주 발생할 수 있는데, 잘못 작성하게 된다면 `server block`에러가 뜬다. 이는 블럭에 있으면 안되는 무언가가 있는 의미로, 설정 파일 내 잘못된 것이 들어가있다고 생각하면 된다. 만약 이 외의 에러를 발견하게 된다면 docker exec로 들어가 하나씩 확인해보는게 좋다.

### Dockerfile.production

```docker
FROM            eb-docker-study:base
MAINTAINER      zehye.01@gmail.com

ENV             BUILD_MODE              production
ENV             DJANGO_SETTINGS_MODULE  config.settings.${BUILD_MODE}

COPY            .   /srv/project

# Nginx 설정파일들 복사 및 enabled로 링크
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/nginx.conf \
                        /etc/nginx/nginx.conf && \
                cp -f   /srv/project/.config/${BUILD_MODE}/nginx_app.conf \
                        /etc/nginx/sites-available/ && \
                rm -rf   /etc/nginx/sites-enabled/* && \
                ln -sf  /etc/nginx/sites-available/nginx_app.conf \
                        /etc/nginx/sites-enabled/

# supervisor설정 복사
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/supervisor.conf \
                        /etc/supervisor/conf.d/

# supervisord실행
CMD             supervisord -n
```

### .config/dev/nginx.conf

```conf
user root;
daemon off;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	# multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;
	gzip_disable "msie6";

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}


#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
#
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}
```

### .config/dev/nginx_app.conf

```conf
server {
    # 80번 포트로부터 request를 받는다
    listen 80;

    # 도메인명이 'localhost'인 경우에 해당
    server_name localhost;

    # 인코딩 방식 지정
    charset utf-8;

    # request/response의 최대 사이즈 지정(기본값이 매우 작음)
    client_max_body_size 128M;

    # '/' (모든 URL로의 연결에 대해)
    location / {
        # uwsgi와의 연결을 unux소켓 (/tmp/app.sock 파일)을 사용한다
        uwsgi_pass      unix:///tmp/app.sock;
        include         uwsgi_params;
    }

    location /static/ {
        alias           /srv/project/.static/;
    }

    location /media/ {
        alias           /srv/project/.media/;
    }
}
```

### .config/dev/supervison.conf

```conf
[program:uwsgi]
command=uwsgi --ini /srv/project/.config/dev/uwsgi.ini

[program:nginx]
command=nginx
```

supervisor가 uwsgi를 실행하는 방법에 변화가 있다. 원래는 pipenv를 사용했지만, 파이썬 이미지 안에서 pip, 파이썬 각각 한개씩 사용하기 때문에 uwsgi라고 실행을 해준다. 그리고 맨 뒤에 socket.ini가 붙은 이유도 우리가 http와 socket을 분리하는 것은 연습용이니 바로 uwsgi.ini를 붙여줬다.


### settings/wsgi/dev.py
```
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.dev")

application = get_wsgi_application()
```

### settings/wsgi/production.py
```
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.production")

application = get_wsgi_application()
```

### .config/dev/uwsgi.ini

```ini
[uwsgi]
;파이썬 프로젝트로 change directory
chdir = /srv/project/app
;가상환경 경로
;home = $(VENV_PATH)
;chdir로 바꾼 파이썬 프로젝트에서 wsgi모듈의 경로(path가 아닌 파이썬 모듈 경로)
module = config.wsgi.dev:application

;socket을 사용해 연결을 주고받음
socket = /tmp/app.sock
;uWSGI가 종료되면 자동으로 소켓파일을 삭제
vacuum = true

;Log
logto = /var/log/uwsgi.log
```

위와 같이 settings의 wsgi를 파이썬 리팩토링을 한 후 dev와 production을 각각 바꿔주지 않으면 `setdefault`로 설정해주는 내용들이 uwsgi.ini에서 쓰는 `module = config.wsgi.dev:application` 즉, wsgi 어플리케이션과 파이썬 어플리케이션과 연결이 될 때 어떤 내용을 쓸건지가 중요한데, 지금 uwsgi.ini에서는 dev와 연결한다고 되어있으니 settings/wsgi/dev와 연결이 되어진다.


<hr>
그리고 추가적으로 .config에 production을 추가한다. production은 dev에 들어간 파일들과 내용은 같지만 supervisor.conf와 uwsgi.ini에서 dev를 production으로 바꿔주기만 하면 된다.  

### settings/dev.py

```
from .base import *

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []

WSGI_APPLICATION = 'config.wsgi.dev.application'


# Database
# https://docs.djangoproject.com/en/2.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/

STATIC_URL = '/static/'
```

### settings/production.py

그러고 production의 도커 이미지를 만들고 docker run을 하기 전에 DEBUG = False와 ALLOWED_HOSTS=['localhost',]설정을 해준다.

DEBUG가 False일 경우 일단 ALLOWED_HOSTS가 없다고 처리가 되기 때문에 localhost를 설정해준다.
```
from .base import *

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []

WSGI_APPLICATION = 'config.wsgi.production.application'


# Database
# https://docs.djangoproject.com/en/2.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/

STATIC_URL = '/static/'
```
