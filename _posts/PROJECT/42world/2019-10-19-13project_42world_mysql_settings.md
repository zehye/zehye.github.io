---
layout: post
title: 세번째 프로젝트, mysql 기본 설정 및 user 및 database 생성하기
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>


## brew install mysql

```
brew update
brew search mysql
brew install mysql

brew list
```

### 설치 후 mysql 서버 접속

```
mysql.server start
mysql_secure_installation  # mysql 설정을 위한 명령어

NO/YES/YES/YES/YES/YES
```

- "Remove anonymous users? (Press y/Y for Yes. any other key for No)"
  - 사용자 설정을 묻는 질문
- "Disallow root login remotely? (Press y/Y for Yes, any other key for No)"
  - 다른 IP에서 root 아이디로 원격접속을 설정하는 질문
- "Remove test database and access to it? (Press y/Y for Yes, any other key for No)"
  - Test 데이터베이스를 설정하는 질문
- "Reload privilege tables now? (Press y/Y for Yes, any other key for No)"
  - 변경된 권한을 테이블에 적용하는 설정에 대한 질문
  - 해당 질문은 무조건 `YES`

```
mysql -uroot -p
status  # characterset utf8확인

exit
mysql.server stop
```


## mysql 삭제하기

```
sudo rm -rf /usr/local/var/mysql
sudo rm -rf /usr/local/bin/mysql*
sudo rm -rf /usr/local/Cellar/mysql
```


## mysql user create

```
mysql -uroot -p

create user '사용자'@'localhost' identified by '비밀번호';
```

### DB권한 부여하기

```
grant all privileges on *.* to '사용자'@'localhost';
grant all privileges on DB이름.* to '사용자'@'localhost';

flush privileges;
```

### 사용자 계정 삭제하기

```
drop user '사용자'@'localhost'
```

## mysql database create

```
create database <database>
```
