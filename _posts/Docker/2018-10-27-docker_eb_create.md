---
layout: post
title: eb create, eb deploy.sh설정하기 1
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## eb init

eb init을 하기 위해 profile 이름 설정은 초기 .aws/credentials에 설정해 놓은 이름으로 한다.
```
pipenv shell
eb init --profile RCNP-EB-User
```
아래 설정 이행하기
```
Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
4) eu-west-1 : EU (Ireland)
5) eu-central-1 : EU (Frankfurt)
6) ap-south-1 : Asia Pacific (Mumbai)
7) ap-southeast-1 : Asia Pacific (Singapore)
8) ap-southeast-2 : Asia Pacific (Sydney)
9) ap-northeast-1 : Asia Pacific (Tokyo)
10) ap-northeast-2 : Asia Pacific (Seoul)
11) sa-east-1 : South America (Sao Paulo)
12) cn-north-1 : China (Beijing)
13) cn-northwest-1 : China (Ningxia)
14) us-east-2 : US East (Ohio)
15) ca-central-1 : Canada (Central)
16) eu-west-2 : EU (London)
17) eu-west-3 : EU (Paris)
(default is 3): 10

Select an application to use
1) EB Docker Deploy
2) [ Create new Application ]
(default is 2): 1

It appears you are using Python. Is this correct?
(Y/n): n

Select a platform.
1) Node.js
2) PHP
3) Python
4) Ruby
5) Tomcat
6) IIS
7) Docker
8) Multi-container Docker
9) GlassFish
10) Go
11) Java
12) Packer
(default is 1): 7

Select a platform version.
1) Docker 18.06.1-ce
2) Docker 18.03.1-ce
3) Docker 17.12.1-ce
4) Docker 17.09.1-ce
5) Docker 17.06.2-ce
6) Docker 17.03.2-ce
7) Docker 1.12.6
8) Docker 1.11.2
9) Docker 1.9.1
(default is 1):
Note: Elastic Beanstalk now supports AWS CodeCommit; a fully-managed source control service. To learn more, see Docs: https://aws.amazon.com/codecommit/
Do you wish to continue with CodeCommit? (y/N) (default is n):
Do you want to set up SSH for your instances?
(Y/n):

Select a keypair.
1) zehye
2) [ Create new KeyPair ]
(default is 1):
```
이렇게 실행을 하게 되면 우리 파이참에 `.elasticbeanstalk`이라는 폴더가 하나 생기고 그 안의 `config.yml`에는 우리가 방금했던 설정들이 그대로 담겨있다.
그리고 이렇게 설정된 elasticbeanstalk 설정들은 자동으로 `.gitignore`에 추가되면서 git에는 추가가 되지 않는다.

<hr>

## AWS Elasticbeanstalk

eb init을 하고난 뒤에 [elasticbeanstalk](https://console.aws.amazon.com/elasticbeanstalk)에 접속해보자.

<center>
<figure>
<img src="/assets/images/ebinit.png" alt="" width="100%">
<figcaption>elasticbeanstalk</figcaption>
</figure>
</center>

우리가 만들고 있는 프로젝트는 반드시 하나의 환경에서만 돌아갈 필요가 없다. 대표적으로 git으로 관리를 할때, 브랜치를 topic, master, dev 브랜치로 나누어 브랜치별로 사용하도록 쉽게 연동이 되어있는데, 서버를 두개를 틀 수가 있다.(운영서버와 개발서버) 이때의 각각 하나하나의 서버를 환경이라고 한다.  

따라서 하나의 애플리케이션에는 여러개의 환경이 존재할 수 있고, 우리는 각 환경마다 코드를 업데이트해서 배포를 실행해 볼 수 있는것이다. 그래서 우리가 dev환경에서 코드를 업데이트해서 배포가 성공하면 이를 master에 올리고 하듯이 온라인에 서버를 놓되 분리를 하는 것이다.

그러나 문제는 이 환경마다 우리는 EC2를 각각 사용한다. 그 말은 환경 두개를 만들면 과금이 된다는 것으로 이에 대한 설명은 파이참의 `.elasticbeanstalk/config.yml`에서 확인할 수 있다.

### eb create

첫번째 환경을 만들어보자.

```
eb create --profile RCNP-EB-User

Enter Environment Name
(default is EBDockerDeploy-dev): RCNP-EBDockerDeploy
Enter DNS CNAME prefix  # CNAME은 겹치면 안된다.
(default is RCNP-EBDockerDeploy):

Select a load balancer type
1) classic
2) application
3) network
(default is 1): 2
```

이때 에러가 나지 않게 하기 위해서는 Dockerfile을 만들어준다.

```dockerfile
FROM            python:3.6.5-slim
MAINTAINER      zehye.01@gmail.com

RUN             apt -y update && apt -y dist-upgrade
RUN             apt -y install build-essential
RUN             apt -y install nginx supervisor

# 로컬의 requirments.txt파일을 /srv에 복사 후 pip install 실행
# (build하는 환경에 requirments.txt가 있어야함!)
COPY            ./requirements.txt   /srv/
RUN             pip install -r /srv/requirements.txt


ENV             BUILD_MODE              production
ENV             DJANGO_SETTINGS_MODULE  config.settings.${BUILD_MODE}

COPY            .   /srv/project

# Nginx 설정파일들 복사 및 enabled로 링크
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/nginx.conf \
                        /etc/nginx/nginx.conf && \
                cp -f   /srv/project/.config/${BUILD_MODE}/nginx_app.conf \
                        /etc/nginx/sites-available/ && \
                /* rm -rf   /etc/nginx/sites-enabled/* && \ */
                ln -sf  /etc/nginx/sites-available/nginx_app.conf \
                        /etc/nginx/sites-enabled/

# supervisor설정 복사
RUN             cp -f   /srv/project/.config/${BUILD_MODE}/supervisor.conf \
                        /etc/supervisor/conf.d/

# 7000번 포트 open
EXPOSE          7000

# supervisord실행
CMD             supervisord -n
```

그리고 현재 requirements.txt가 없으니 `pipenv lock --requirements > requirements.txt`를 통해 만들어준다.

### eb deploy

위를 다 실행한 뒤 `eb deploy --profile RCNP-EB-User`를 통해 배포를 한다. 배포를 하고 나면 이제 elasticbeanstalk에 새로운 환경이 만들어질 것이다. 이는 파이참에서 `.elasticbeanstalk/config.yml`에서 확인할 수 있는데 이때 `branch-default/master:environment`에 내 eb의 이름이 들어가게 되어있는 것을 볼 수 있다. 그러나 위 같은 명령어를 친다면 `ServiceError`가 뜰것이다.

이는 `eb deploy`를 할때, master 브랜치에 있는 내용만 올라가고 나머지 커밋하지 않은 내용들은 배포가 되지 않는다는 의미이다. 즉 git에 포함된 내용만 올라간다는 의미이다.

- [EB CLI와 Git사용](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb3-cli-git.html) 참고

따라서 eb deploy에서는 최신 커밋을 배포하기 때문에 추가한 Dockerfile과 requirements.txt를 git에 올려야하지만, 커밋을 하지 않고도 배포를 할 수 있는 방법이 있다.

`eb deploy --profile RCNP-EB-User --staged`

이 방식을 쓰는 이유는 굳이 Pipfile이 있기때문에 git에 requirements.txt를 올리고 싶지 않은데(어짜피 Docker에서도 생성 후 바로 삭제하니까) 이를 번거롭게 하는것이 불편하니, `--staged`명령어를 통해 잠시만 requirements.txt를 `staging area`에 넣고 배포를 하고 배포가 끝나면 다시 `staging area`에서 나오도록 하는 것이다.

즉, 배포를 하기 전에 staging area에 넣어놓고 완전히 커밋된 내용이 아닌 staging된 애들까지 올려놓고 배포가 끝나면 staging area에서 빼내는 것이다.

```
git add .gitignore Dockerfile
git commit -m 'eb deploy를 위한 Dockerfile 설정'
```

이렇게 하면 현재 `requirements.txt`만 남아있을 것이다.
```
git add requirements.txt
```
그 이후에
```
eb deploy --profile RCNP-EB-User --staged
```

<hr>

위에서 Dockerfile을 작성할 때, `EXPOSE`명령어를 사용했을 것이다.
- [단일컨테이너 Docker](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create_deploy_docker_image.html) 참고

여기서 EXPOSE에 대한 설명을 보면

(필수) Docker 컨테이너에 표시할 포트를 나열한다. Elasticbeanstalk에서는 ㅗ트값을 사용해 호스트에서 실행중인 역방향 프록시네 Docker컨테이너를 연결한다고 적여있다.

이는 Docker를 실제로 EC2에서 써야할 때 셋팅을 하는 것을 의미한다.

```
Browser -> Elasticbeanstalk
    -> Load balancer
    -> EC2  -> Nginx -> Docker -> Nginx -> uWSGI -> Django
```
이는 우리가 실제로 보낸 요청은 Elasticbeanstalk으로 가도록 설정해놓았지만 실제 요청은 Load balancer가 받도록 되어진다. 이렇게 Load balancer가 받은 요청은 각각의 EC2에게 보내지는데, 이 EC2에게 들어온 요청은 위와같은 순서로 이루러져 보내진다.

그런데 위에서의 Nginx는 각각 다른 Nginx를 의미한다.

첫번째 Nginx는 EB안에 저장된 Nginx이며 두번째 Nginx는 Docker container안에 존재하는 Nginx를 의미한다. 그리고 이들을 연결시키는 방법을 `Reverse proxy`라고 한다.

```
Browser -> Elasticbeanstalk
    -> Load balancer
        -> EC2  -> Nginx(EB)
            (Reverse proxy)
            -> Docker -> Nginx(Docker container) -> uWSGI -> Django
```

이렇듯 역방향 프록시를 통해 이들을 연결시켜 주는데, 이 방식은 위의 첫번째 Nginx로 온 모든 요청 중 어떤 특별한 값을 Docker이미지 안으로 보내준다는 것이다. (proxy는 거쳐간다는 의미) 이렇게 Nginx로 온 요청들을 도커 컨테이너로 보내줄때 포트값을 이용해 보내주게 되는데, 만약 docker container를 7999로 열어놨다고 가정하면 역방향 프록시를 통해 Nginx로 온 요청(80)을 7999로 보내주는 것이다.

즉 Nginx는 외부에서 온 80번 포트 요청을 역방향 프록시틑 통해 내부에 있는 도커에게 보내는데 만약 7999포트가 열려있다면 무조건 7999로 연결이 된다.
