---
layout: post
title: Docker Elasticbeanstalk 소개
category: Docker
tags: [Docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Elasticbeanstalk

우리가 지금까지 배포를 할때에는, EC2에 가상서버를 할당받아 그 위에 직접 모든것을 셋팅하고 올렸는데, 아마존에 자동화해주는 것을 사용하는 방법이 있다.

이전까지의 EC2방법에서는 이런 흐름이었다.
```
EC2 (가장 Low한 방법, 낮은 방법이라는 뜻보다는 가장 많은 부분 우리가 셋팅해줘야 하는 부분이 많다는 뜻)
  S3
  RDS
```

```
Load Balancer (EC2들이 늘어날때 실제 트레픽을 받는 부분, 모든 Http request를 받아줌)
  Auto Scaling (과부하가 걸리면, 서버에 너무 많은 요청이 오게되면 시스템을 확장해주는 기능)
  (같은 EC2를 CPU가 늘어났을때, 여러개로 늘려준다. 하나의 EC2는 하나의 public IP를 가지고 있는데
    이를 통해 EC2가 늘어나게 되면 이 앞에 Load Balencer가 있어야 한다.)
    EC2
    EC2
```

Load Balancer가 request를 받으면 그 내용을 적절히 Auto Scaling된 EC2들에게 누가 어느정도의 CPU를 사용하고 있구나 라는 정보를 각각의 EC2들에게 분배를 해준다. 이렇게 요청이 따로 가도 상관없는 이유는 우리가 각 EC2에서 데이터를 전부 통합된 곳에서 사용하기 때문이다.

RDS, S3의 서버를 따로 주고 있기 때문에 어느 EC2를 처리하건 같은 응답을 받는다.

더 나아가
```
Route 53 (Domain, SSL인증서를 붙여준다.)
  Load Balancer (EC2들이 늘어날때 실제 트레픽을 받는 부분, 모든 Http request를 받아줌)
    Auto Scaling (과부하가 걸리면, 서버에 너무 많은 요청이 오게되면 시스템을 확장해주는 기능)
    (같은 EC2를 CPU가 늘어났을때, 여러개로 늘려준다. 하나의 EC2는 하나의 public IP를 가지고 있는데
      이를 통해 EC2가 늘어나게 되면 이 앞에 Load Balencer가 있어야 한다.)
      EC2 (Docker -> Docker container)
      EC2 (Docker -> Docker container)
                        RDS
                        S3
```

아래 방법은 ECS

```
Load Balancer
  Auto Scaling
    Docker
```

그런데 위의 작업들이 다 자동화되어있고 docker만 만들면 되는, EB(Elasticbeanstalk)방법이 있다.
```
Docker
```

<hr>

AWS Elastic Beanstalk는 Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker를 사용하여 Apache, Nginx, Passenger, IIS와 같은 친숙한 서버에서 개발된 웹 애플리케이션 및 서비스를 간편하게 배포하고 조정할 수 있는 서비스입니다.

사실 Elasticbeanstalk을 사용하면 자동으로 Nginx가 설치되어 나오므로 우리는 우리의 애플리케이션을 붙이는 설정만 하면 된다.

코드를 업로드하기만 하면 Elastic Beanstalk가 용량 프로비저닝, 로드 밸런싱, 자동 크기 조정부터 시작하여 애플리케이션 상태 모니터링에 이르기까지 배포를 자동으로 처리합니다. 이뿐만 아니라 애플리케이션을 실행하는 데 필요한 AWS 리소스를 완벽하게 제어할 수 있으며 언제든지 기본 리소스에 액세스할 수 있습니다.

Elastic Beanstalk는 추가 비용 없이 애플리케이션을 저장 및 실행하는 데 필요한 AWS 리소스에 대해서만 요금을 지불하면 됩니다.
