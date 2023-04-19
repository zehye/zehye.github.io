---
layout: post
title: JSP & Servlet - 서블릿의 생명주기 (init, service, destroy)
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Servlet Life Cycle

서블릿 객체는 사용자가 데이터를 요청하면 그때부터 일을 시작하고 해당 요청에 대한 데이터를 만들어 응답을 하면 서블릿의 역할을 다하게 된다. 그때 요청이 들어와 서블릿이 작업을 시작하고 응답을 통해 작업을 종료함으로써 과정이 이루어지는데, 이를 서블릿의 생명주기라고 한다.

- `@PostConstruct` : 서블릿이 시작하기전 준비하는 단계
- `init()` : 서블릿 생성단계
- `service` : 서블릿이 일하는 단계. 개발자가 구현한 기능을 작동하는 단계
- `destroy()` : 응답 후 서블릿이 컨테이너에서 소멸, 종료되는 단계
- `@PreDestroy` : 필요없어진 서블릿이 정리되는 단계

### 생명주기 관련 메서드

서블릿 단계별로 호출(콜백)메서드를 제공함으로써 각 단계별로 특정 작업을 더할 수 있음


```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throw ServletException, IOException {
  System.out.println(" -- doGet() --");
}

@PostConstruct
public void postConstruct() {
  System.out.println(" -- postConstruct() --");
}

@Override
public void init() throws ServletException {
  System.out.println(" -- init() --");
}

@Override
public void destroy() {
  System.out.println(" -- destroy() --");
}

@PreDestroy
public void preDestroy() {
  System.out.println(" -- preDestroy() --");
}
```

- init(): init메서드 오버라이드를 통해 개발자가 기술해주고 싶은 내용을 기술해줌으로써 그 기능이 컨테이너에서 작용
- service: service메서드를 사용하기보다 doGet에서 실제작업을 많이 함. 즉, 실제 서블릿이 하는 일은 doGet에서 기술
- destroy(): init과 같이 오버라이드해서 기능을 기술

만약 init()과 destroy()단계가 필요없고 실행단계만 필요하다면 굳이 두개의 메서드를 오버라이드 하지않고 doGet, doPost로만 작성해도 된다.
