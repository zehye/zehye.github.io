---
layout: post
title: AWS(Amazon Web Service) 종류와 특징
category: AWS
tags: [AWS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## AWS(Amazon Web Service)

- 인프라를 제공하는 역할을 하며, 웹 서비스를 해야하는 개발자들은 AWS에서 제공하는 인프라를 임대하여 사용
- 서버의 구매, 구축, 운영을 대행해주는 서비스
- 탄력적인 인프라 운영이 가능
  - 사용자 폭주 시 서버의 성능, 개수를 유동적으로 빠르게 변경 가능
- 사용한 만큼 과금하는 방식


### EC2(Elastic Compute Cloud)

- 1개의 독립적인 컴퓨터를 임대하는 것: 평범한 컴퓨터처럼 사용이 가능 (인스턴스)
- 물리적인 컴퓨터가 아닌 가상의 컴퓨터
- Linux, Window 운영체제를 제공함
- 웹서버 또는 애플리케이션 서버로 사용함


### S3(Simple Storage Service)

- 파일서버: 이미지나 동영상을 가지고 있다가 제공해줌
- EC2에서도 할 수 있지만, S3의 경우 특정 변경없이 무제한 저장이 가능(주로사용)
- 스케일은 아마존 인프라가 담당하기에 편리함
- 1byte ~ 5tbyte 까지의 단일 파일 저장이 가능


### RDS(Relational Database Service)

- DB를 쉽게 운영할 수 있게 해주는 서비스
- Mysql, Oracle 등등이 존재
- 백업, 리플리케이션과 같은 것을 아마존 인프라가 자동으로 제공


### EDB(Elastic Load Balacing)

- EC2로 유입되는 트래픽을 여러대의 EC2로 분산하는 기능
- 장애가 발생한 EC2를 감지해 자동으로 재베함
- Auto Scaling 기능을 이용해 EC2를 자동으로 생성 및 삭제함


이러한 AWS 서비스를 제어하는 방법에는 3가지가 있다.

1. AWS Management Consle
2. CLI (Command Line Interface)
3. SDK (Software Development Kit): 프로그래밍으로 제어할 수 있는 API 묶음
