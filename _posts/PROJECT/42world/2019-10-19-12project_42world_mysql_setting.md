---
layout: post
title: 세번째 프로젝트, MySQL 설치 및 셋팅과정 (+MySQL Workbench)
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>


가희가 만들어 놓은 저장소를 클론해가져온 뒤 requirements.txt를 통해 각종 설치파일들을 설치하려고하니, mysqlclient만 계속해서 설치가 안되고 있었다. stackoverflow 등 구글링을 해서 찾아보니 사실 그냥 너무나도 단순한 이유로 되고 있지않았었다.

그냥 MySQL이 설치가 안되어있었던 것일 뿐


아래의 글을 참고하여 작성하였습니다.

- [MySQL 깨알 팁 모음!](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html)
- [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/09/mysql-workbench-guide.html)


## MySQL 설치하기 (최신버전)

`brew install mysql`


### MySQL 실행하기

`brew services start mysql`

`mysql.server start`


우리가 MySQL을 좀더 편하게 사용하기 위해서 `mySQL Workbench`라는 프로그램을 사용할 것인데, 이때 여기서 mySQL 서버를 켜줘야 얘 또한 실행이 가능하다.


## MySQL Workbench

[MySQL Workbench 다운](https://dev.mysql.com/downloads/workbench/)

해당 os에 맞게 설치하면 된다. 나같은 경우에는 macOS가 최신버전이 아니어서 최신의 MySQL Workbench을 다운받고나니 실행이 안되어 이전 버전으로 재 설치 후 실행하였다.


<center>
<figure>
<img src="/assets/post-img/mysql/mysql1.png" alt="" width="100%">
<figcaption>MySQL Workbench</figcaption>
</figure>
</center>


- Connection Name: 원하는 Connection name을 적어줌
- Hostname/port
  - Hostname: 127.0.0.1 또는 localhost
  - port: MySQL기본 포트 3306
- Username: 접속할 계정이름
- Password: 계정에 대한 password


만약 connection에 실패할 경우 MySQL server가 켜져있는지 확인하거나 password를 올바르게 적었는지 확인한다.
