---
layout: post
title: Django - Docker 설치 후 실행하기
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Docker install for mac
> [[Docker for mac]](https://docs.docker.com/docker-for-mac/#explore-the-application)

## Docker 시작하기

```shell
docker run ubuntu:16.04
```

이렇게 하면 컨테이너는 정상적으로 실행됐지만 뭘 하라고 명령어를 전달하지 않았기 때문에, 컨테이너가 실행되지 마자 종료된다. 이번에는 `/bin/bash`를 실행해본다.

```shell
docker run --rm -it ubuntu:16.04 /bin/bash
```

여기서 `rm`은 종료됐을 때 컨테이너를 삭제한다는 뜻이고 `-it`는 사용자 입력을 받을 수 있도록 만든다는 것이고 `/bin/bash`는 binary/bash를 실행한다는 것으로 bash가 실행되는 것을 볼 수 있다.

```shell
root@4b614ebdd95c:/#
```
여기서의 프롬프트는 컨테이너 안에서 실행된 것이다.

파이참으로 돌아와 Dockerfile을 만들어보자 (plugin/Docker Integration)

```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh
```
`-y`는 실행중에 y/n를 물어보는 경우 무조건 y를 설정한다는 것으로 docker는 실행중에 이런 사용자 입력을 넣어주게 되면 무조건 중단이 되기 때문에 `-y`를 꼭! 넣어줘야 한다.

쉘로 돌아와

```shell
docker build -t ec2-deploy -f Dockerfile .
```
`-t`는 어떤 이미지 이름을 지을 건지, `-f`는 이 이미지를 만들기 위해 어떤 도커 파일을 사용할 것인지 그리고 이 도커파일을 생성하기 위한 위치를 어디로 정할 것인가 `.`를 지정해준다.

명령어 치고나면 성공! 그럼 더 추가해서 설치해본다.

```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh
```
위에서의 `curl`은 커맨드라인에서 데이터를 다운받을 때 사용한다. (request의 GET과 같은 의미) 즉, curl다음에 나오는 명령줄을 다운받아오는데 curl을 실행하기 위해서 zsh과 함께 curl로 설치해준다.

이제 이를 실행해보기 위해서

```shell
docker run --rm -it ec2-deploy /bin/zsh
```

성공하면 이제 pyenv를 설치해본다
```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh

# pyenv
RUN                 curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
RUN                 apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                           libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                           xz-utils tk-dev libffi-dev

# pyenv settings
RUN                 echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv init -)"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

> echo 'export PATH="~/.pyenv/bin:$PATH"': path라는 변수를 새로 만들어 바깥으로 보낸다(export)

원래 있던 PATH를 참조하는데 ($PATH)-쉘 스크립트에서 변수를 참조하기 위해서는 $를 사용하는데, :을 붙이고 새로운 경로를 추가했다.(~/.pyenv/bin) 그러면 이 경로를 포함한 PATH를 바깥으로 보내겠다는 의미이다.

shell에서 zshrc(환경설정)를 수정하고 나서는 터미널을 나갔다 들어와야하는데 도커에서는 어떻게 나갈 수 있을까?

파이썬을 설치하기 전 아예 환경변수를 설정해놓는다. (`ENV PATH /root/.pyenv/bin:$PATH`)

# install python
```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh

# pyenv
RUN                 curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
RUN                 apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                           libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                           xz-utils tk-dev libffi-dev

# pyenv settings
RUN                 echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv init -)"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# install python
ENV                 PATH /root/.pyenv/bin:$PATH
RUN                 pyenv install 3.6.5
```

이제 더 나아가 pipenv를 설치해본다.

```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh

# pyenv
RUN                 curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
RUN                 apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                           libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                           xz-utils tk-dev libffi-dev

# pyenv settings
RUN                 echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv init -)"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# install python
ENV                 PATH /root/.pyenv/bin:$PATH
RUN                 pyenv install 3.6.5

# pipenv
RUN                 apt -y install software-properties-common python-software-properties
RUN                 add-apt-repository -y ppa:pypa/ppa
RUN                 apt -y update
RUN                 apt -y install pipenv

# Language
ENV                 LANG    C.UTF-8
ENV                 LC_ALL  C.UTF-8
```

이렇게 하고나면 이제 우리가 만들었던 프로그램들을 도커파일 내부로 다 복사해와야 한다. (scp와 같은 원리)

```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh

# pyenv
RUN                 curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
RUN                 apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                           libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                           xz-utils tk-dev libffi-dev

# pyenv settings
ENV                 PATH /root/.pyenv/bun:$PATH

                    # pyenv명령어를 실행하기 위한 한줄
RUN                 echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.zshrc
                    # pyenv를 통해 어떤 파이썬 버전을 가져오기 위한 한줄
RUN                 echo 'eval "$(pyenv init -)"' >> ~/.zshrc
                    # pyenv를 통해 virtualenv를 가져오기 위한 한줄
RUN                 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# 위의 두줄이 정상적으로 작동을 해야 그 쉘에서 정상적으로 pyenv가 가지고 있는 python, virtualenv가 작동이 된다. 그런데 이 내용이 zshrc에 들어가 있게 되면서, 밑에서 이 내용을 직접 실행해주지 않으면 작업이 이루어지지 않는다.

# 그래서 지금 우리가 python3.6.5를 설치하는 데에는 성공했지만, 나중에 python을 실행할때 pyenv에서 가지고 오지를 못한다.

# install python, apply pyenv settings
ENV                 PATH /root/.pyenv/bin:$PATH
# 그래서 아래 두 내용을 한번 더 추가해줘야 한다.
RUN                 eval "$(pyenv init -)"
RUN                 eval "$(pyenv virtualenv-init -)"
RUN                 pyenv install 3.6.5

# pipenv
RUN                 apt -y install software-properties-common python-software-properties
RUN                 add-apt-repository -y ppa:pypa/ppa
RUN                 apt -y update
RUN                 apt -y install pipenv

# Language
ENV                 LANG    C.UTF-8
ENV                 LC_ALL  C.UTF-8

RUN                 apt -y install vim

# Copy project file
COPY                .   /srv/project
WORKDIR             /srv/project
RUN                 pipenv install --skip-lock # 실행되는 시간을 줄이기 위해
```

그런데 이렇게 해서 docker run을 해서 도커 내부로 들어와 runserver를 실행하면 이는 연결이 되지 않는다. 즉 도커는 외부와의 연결고리가 존재하지 않기때문에 특별한 옵션을 통해 외부와의 연결이 필요하다. 이때 사용하는게 `-p`로 이를 포워딩이라고 한다.

```shell
docker run --rm -it -p 9999:8000 ec2-deploy /bin/zsh
```
이는 호스트의 9999로 온 요청을 이 도커 컨테이너의 8000번 포트에 있는 프로그램으로 다 보내겠다는 의미이다. 지금 우리가 보고 있는 인터넷 창 (localhost:9999)에 어떤 요청이 오면 8000으로 다 보내고 마찬가지로 8000으로 응답을 다시 9999로 보내주는 것이다.


pipenv 설치하기

```Dockerfile
CMD                 pipenv run uwsgi \
                          --http :8000 \
                          --chdir /srv/project/app \
                          --module config.wsgi
```

RUN은 이미지를 생성해주는 것이고, CMD는 이미지가 만들어진 다음에 하는 것으로 한번만 실행된다. 그리고 이렇게 docker run을 할때에는 /bin/zsh를 없애줘야 한다. 왜냐하면 CMD가 실행할때 뒤에 명령어가 존재하면 CMD를 무시하게 되기 때문이다.
