---
layout: post
title: Django - deploy sh
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Deploy

`#!/usr/bin/python`: manage.py인 파이썬 파일을 실행하려 할때, 파이썬 모듈이기 때문에 `python manage.py`로 실행하는데 사실 `./manage.py`로도 실행이 가능하다. 이 의미는 현재 위치에 있는 manage.py를 실행하겠다는 의미이다.

그런데 그냥 `manage.py`라고 치게되면, 이 명령어를 path에서 찾는다. (> zshrc)

우리가 그전에 zshrc에서 `export PATH=$HOME/bin:/usr/local/bin:$PATH`를 추가했었는데, 여기서의 `:`은 연결을 의미한다.

deploy.sh의 sh는 shell script의 의미로 여기서 언어를 짜면 다른 프로그램을 설치할 필요가 없다. 쉘에 대부분의 프로그램이 탑재가 되어있기 때문에, 근데 그럼에도 불구하고 여기서 프로그램을 안짜는 이유는 너무 어렵고 복잡하기 때문이다.

```shell
#!/usr/bin/env bash

# 서버의 파일을 지움
ssh -i ~/.ssh/zehye.pem ubuntu@ec2-13-209-88-249.ap-northeast-2.compute.amazonaws.com rm -rf /home/ubuntu/project

# 서버에 프로젝트 파일을 다시 업로드
scp -i ~/.ssh/zehye.pem \         
-r ~/projects/deploy/ec2-deploy \
ubuntu@ec2-13-125-252-52.ap-northeast-2.compute.amazonaws.com:/home/ubuntu/project
```

중복을 없애자
```shell
# $HOME대신 ~은 안된다. HOME은 `echo HOME`을 해보면 나온다. < /Users/jihye >
IDENTITY_FILE="$HOME/.ssh/zehye.pem"
USER="ubuntu"
HOST="ec2-13-125-252-52.ap-northeast-2.compute.amazonaws.com"
PROJECT_DIR="$HOME/projects/deploy/ec2-deploy"
SERVER_DIR="/home/ubuntu/project"

# 서버의 파일을 지움
echo "Start Deploy"
ssh -i ${IDENTITY_FILE} ${USER}@${HOST} rm -rf ${SERVER_DIR}
echo "- Delete server files"

# 서버에 프로젝트 파일을 다시 업로드, -q(quiet)옵션을 통해서 올라가는 파일들을 보이지 않게 해준다.
scp -q -i ${IDENTITY_FILE} -r ${PROJECT_DIR} ${USER}@${HOST}:${SERVER_DIR}
echo "- Upload files"

echo "- Deploy Complete"
```

그러고 `./deploy.sh`를 하면 `Permission denied`가 뜬다.

`x`권한이 없기때문에 발생하는 에러이므로 `chmod 755 deploy.sh`를 하여 쓰는것은 소유자만 하고 읽거나 실행하는건 누구나 할 수 있도록 해준다.

> deploy의 test코드를 만들기 위해 test.sh를 만들어 실행해본다.

```shell
IDENTITY_FILE="$HOME/.ssh/zehye.pem"
USER="ubuntu"
HOST="ec2-13-125-252-52.ap-northeast-2.compute.amazonaws.com"
PROJECT_DIR="$HOME/projects/deploy/ec2-deploy"
SERVER_DIR="/home/ubuntu/project"

# ssh로 서버에 접속하는 명령어/ ssh를 통해 HOST에 접속하는 것까지 실행
CMD_CONNECT="ssh -i ${IDENTITY_FILE} ${USER}@${HOST}"

# pipenv에서 --venv를 하면 현재 쓰고 있는 virtualenv가 어디에서 실행되고 있는지를 알 수 있다.
# 서버 접속 후 SERVER_DIR로 이동, pipenv --venv로 가상환경의 경로 가져오기
# CMD_CONNECT명령어를 실행한 다음에 나머지 두 명령어도 한번에 실행할 커멘드 두개를 가져온다. (&&)
# $다음 ()는 $다음의 명령어를 실행하고 실행된 결과를 문자열로 출력해준다.
VENV_PATH=$(${CMD_CONNECT} "cd ${SERVER_DIR} && pipenv --venv")

# 가상환경의 경로에 /bin/python을 붙여 서버에서 사용하는 python의 경로 만들기
PYTHON_PATH="${VENV_PATH}/bin/python"
echo ${PYTHON_PATH}

# runserver를 backgroud에서 실행해주는 커멘드(nohup)
# nohup을 사용하면 세션이 종료되어도(로그아웃을 해도) 계속해서 실행이 가능하다.
# 그냥 python이라고 쓰면 시스템에 있는 python이 실행되는데, 우리는 pipenv로 관리되고 있는
# 가상환경에 있는 python을 실행하도록 한다.
RUNSERVER_CMD="nohup ${PYTHON_PATH} manage.py runserver 0:8000"
echo ${RUNSERVER_CMD}

# 서버 접속후, 프로젝트의 'app'폴더까지 이동한 후 runserver명령어를 실행
${CMD_CONNECT} "cd ${PYTHON_PATH}/app && ${RUNSERVER_CMD}"

echo "End"
```

$ 표시뒤에 괄호치고 실행될 명령어를 치면, 그 명령어의 결과가(콘솔의 아웃풋) 변수에 나온다. SERVER_DIR로 이동한 다음에 pipenv --venv를 한 결과가 아래에 출력된다. 그리고 그 파이썬을 통해 명령어로 runserver를 시킨다.

만약 runserver가 켜져있다면 끄고 다시 시작할 수 있도록 종료하는 명령어를 추가한다.

우선 shell에서 ssh로 접속한 후 `ps -ax | grep runserver`를 통해서 현재 접속되어 있는 runserver를 확인해본다.

> ps -ax 는 현재 사용되고 있는 프로세스들을 보여준다. 그런데 그 중에서 runserver만 보고싶을떄 grep을 사용한다.

`ps -ax | grep runserver | awk '{print $1}'`를 통해 현재 실행되고 있는 runserver들의 pid를 확인하고 `kill $(ps -ax | grep runserver | awk '{print $1}')`를 해서 실행되고 있는 runserver를 삭제한다.

deploy.sh

```shell
# $HOME대신 ~은 안된다. HOME은 `echo HOME`을 해보면 나온다. < /Users/jihye >
IDENTITY_FILE="$HOME/.ssh/zehye.pem"
USER="ubuntu"
HOST="ec2-13-125-252-52.ap-northeast-2.compute.amazonaws.com"
PROJECT_DIR="$HOME/projects/deploy/ec2-deploy"
SERVER_DIR="/home/ubuntu/project"

# ssh로 서버에 접속하는 명령어/ ssh를 통해 HOST에 접속하는 것까지 실행
CMD_CONNECT="ssh -i ${IDENTITY_FILE} ${USER}@${HOST}"
echo "Start Deploy"

# 서버에서 실행중이던 runserver 프로세스들을 모두 종료
# ${CMD_CONNECT} "ps -ax | grep runserver"
${CMD_CONNECT} "pkill -9 -ef runserver"
# ${CMD_CONNECT} "ps -ax | grep runserver"
echo "- Kill runserver"

# 서버의 파일을 지움
${CMD_CONNECT} rm -rf ${SERVER_DIR}
echo "- Delete server files"

# 서버에 프로젝트 파일을 다시 업로드, -q(quiet)옵션을 통해서 올라가는 파일들을 보이지 않게 해준다.
scp -q -i ${IDENTITY_FILE} -r ${PROJECT_DIR} ${USER}@${HOST}:${SERVER_DIR}
echo "- Upload files"

# pipenv에서 --venv를 하면 현재 쓰고 있는 virtualenv가 어디에서 실행되고 있는지를 알 수 있다.
# 서버 접속 후 SERVER_DIR로 이동, pipenv --venv로 가상환경의 경로 가져오기
# CMD_CONNECT명령어를 실행한 다음에 나머지 두 명령어도 한번에 실행할 커멘드 두개를 가져온다. (&&)
# $다음 ()는 $다음의 명령어를 실행하고 실행된 결과를 문자열로 출력해준다.
VENV_PATH=$(${CMD_CONNECT} "cd ${SERVER_DIR} && pipenv --venv")

# 가상환경의 경로에 /bin/python을 붙여 서버에서 사용하는 python의 경로 만들기
PYTHON_PATH="${VENV_PATH}/bin/python"
echo "- Get Python path ${PYTHON_PATH}"

# runserver를 backgroud에서 실행해주는 커멘드(nohup)
# nohup을 사용하면 세션이 종료되어도(로그아웃을 해도) 계속해서 실행이 가능하다.
# 그냥 python이라고 쓰면 시스템에 있는 python이 실행되는데, 우리는 pipenv로 관리되고 있는
# 가상환경에 있는 python을 실행하도록 한다.
RUNSERVER_CMD="nohup ${PYTHON_PATH} manage.py runserver 0:8000 &>/dev/null &"

# 서버 접속후, 프로젝트의 'app'폴더까지 이동한 후 runserver명령어를 실행
${CMD_CONNECT} "cd ${PYTHON_PATH}/app && ${RUNSERVER_CMD}"
echo "- Excute runserver"

echo "- Deploy Complete"
```

이 모든게 잘 실행되고 있는지를 확인하고 싶다면 settings.py에서 '.amazonaws.com'를 주석처리 했을때와 하지 않았을 때의 deploy.sh의 서버 창 확인을 하면서 보면 된다. (allowedhost error가 뜨는지 안뜨는 지 확인)
