---
layout: post
title: Java - macOS 환경변수(PATH) 설정
category: Java
tags: [Java, setting]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## macOS Java 환경변수(PATH) 설정하기

- macOS 환경에서 JDK 기본 버전을 1.8로 설정하고 환경 변수 path를 잡아줘서 어떤 디렉터리 위치에서도 사용할 수 있도록 설정
  - (꼭, JDK 버전이 1.8이 아니더라도 자신에게 필요한 다른 버전도 전부 동일한 방식으로 설정하면 됨)

- Java 개발 툴(IDE)를 이용하여 공부하시는 분은 꼭 환경 변수를 설정하지 않아도 큰 문제 없음.

**설명하기에 앞서 "macOS High Sierra 버전 10.13" 운영체제에서 설정하였습니다.**


### 터미널을 실행 후 아래 명령어 실행

- cd /Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home
  - "jdk1.8.0_221.jdk"는 본인이 설정한 버전에 따라 명칭이 다를 수 있으며, 해당 명령어를 통해 설치 되어있는 JDK경로로 이동

JDK가 설치된 디렉터리로 이동하였으면 터미널에서 아래 명령어 입력
-  vi ~/.bash_profile

해당 창에서 아래 입력 저장 후 나가기
- export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home
- export PATH=${PATH}:$JAVA_HOME/bin

원래 창으로 돌아오고 나면
- source ~/.bash_profile

설정 완료!

### 정상적으로 설정되었는 지 확인하려면 아래 명령어 실행

아래 명령어 실행 시 설정한 JDK 경로가 출력되어야 함

- echo $PATH

- javac -version

- java -version

세가지 명령어를 확인했을때 정상적으로 출력된다면 완벽하게 환경변수 PATH 설정을 완료한 것!
