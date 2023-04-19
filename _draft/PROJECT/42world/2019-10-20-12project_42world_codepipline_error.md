---
layout: post
title: 세번째 프로젝트, AWS Codebuild 사용하며 발생한 에러 해결해나가기
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## AWS CLI 설치과정에서의 에러

처음은 매우 단순하게 aws cli를 설치하려고 하는데 계속 이상한 에러가 갑자기 뜨기 시작했다. AWS 홈페이지 통해서 차근히 설치를 하려고하는데 발생하는 에러..

[AWS CLI 설치하기](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/cli-chap-install.html)

홈페이지에서 하라는대로 아래 명령어를 쳤더니

`pip3 install --upgrade --user awscli`


```
Requirement already up-to-date: awscli in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (1.16.263)
Requirement already satisfied, skipping upgrade: docutils<0.16,>=0.10 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (0.15.2)
Requirement already satisfied, skipping upgrade: botocore==1.12.253 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (1.12.253)
Requirement already satisfied, skipping upgrade: s3transfer<0.3.0,>=0.2.0 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (0.2.1)
Requirement already satisfied, skipping upgrade: rsa<=3.5.0,>=3.1.2 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (3.4.2)
Requirement already satisfied, skipping upgrade: colorama<0.4.2,>=0.2.5; python_version != "2.6" and python_version != "3.3" in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (0.4.1)
Requirement already satisfied, skipping upgrade: PyYAML<5.2,>=3.10; python_version != "2.6" and python_version != "3.3" in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from awscli) (5.1.2)
Requirement already satisfied, skipping upgrade: urllib3<1.26,>=1.20; python_version >= "3.4" in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from botocore==1.12.253->awscli) (1.25.6)
Requirement already satisfied, skipping upgrade: python-dateutil<3.0.0,>=2.1; python_version >= "2.7" in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from botocore==1.12.253->awscli) (2.8.0)
Requirement already satisfied, skipping upgrade: jmespath<1.0.0,>=0.7.1 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from botocore==1.12.253->awscli) (0.9.4)
Requirement already satisfied, skipping upgrade: pyasn1>=0.1.3 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from rsa<=3.5.0,>=3.1.2->awscli) (0.4.7)
Requirement already satisfied, skipping upgrade: six>=1.5 in /usr/local/var/pyenv/versions/3.7.4/lib/python3.7/site-packages (from python-dateutil<3.0.0,>=2.1; python_version >= "2.7"->botocore==1.12.253->awscli) (1.12.0)
```

이런 요상망측한 에러가 나타났다.

`sudo -H pip install awscli --upgrade --ignore-installed six`
