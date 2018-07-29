---
layout: post
title: IaaS, PaaS, SaaS
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## IaaS, PaaS, SaaS

IaaS, PaaS, SaaS는 클라우드 서비스의 종류로, 필요한 만큼 서비스 형태를 이용하는 것을 의미한다.

AWS에서는 2006년 출시되었을 때는 Iaas만 해당했지만 이제는 IaaS, PaaS, SaaS 모두 있다.

<center>
<figure>
<img src="/assets/post-img/docker/cloud-stack.png" alt="" width="100%">
<figcaption>ouath flow</figcaption>
</figure>
</center>

#### 1. On-Premises

대기업이나, 예전방식, 즉 하드웨어부터 다 셋팅하는 것으로 IDC에 컴퓨터를 넣고 네트워크를 넣고 저장소, 서버, 데이터 등을 다 처음부터 셋팅하는 것으로 장점은 내가 하드웨어를 직접 컨트롤 할 수 있고 가성비가 좋다는 점이다.

#### 2. Infrastructure(as a service) - IaaS

인프라 자체를 서비스 형태로 하는 것으로, OS를 깔기 직전까지 제공한다. (사실 OS까지 제공하는 것이다)

여기서 your manage는 뭐냐면, 우리가 썻던 EC2는 IaaS인데, 그 중 우리가 OS를 선택할 수 있었고 그 선택전에는 우리가 할 수 없었기 때문에 OS를 선택하고 데이터를 애플리케이션 관리하고 이를 동작하게 하기 위해 uWSGI, Nginx를 깔고 했던 모든 행위들이 다 우리의 manage였다.

#### 3. Platform (as a service) - PaaS

이제 우리가 쓸 Elasticbeanstalk, 애플리케이션 하나 올리면 (데이터와 함께) 나머지 아래의 활동은 우리가 할 필요가 없다.

여기서의 애플리케이션은 장고 애플리케이션을 의미하고, 정확히는 도커 이미지로 만드는 컨테이너(도커 이미지도 도커 애플리케이션이다)로 도커 이미지는 안에서 이미 셋팅을 다하면 docker run하면 한번에 다 실행되듯이 그래서 도커 애플리케이션이라고도 한다. (그 안에 장고 애플리케이션이 들어있는것)

#### 4. Software (as a service) - SaaS

우리가 할 게 없고, 구글 스프레드시트를 생각하면 되는데 분명 우리는 깐게 하나도 없는데 프로그램을 사용할 수 있다.
