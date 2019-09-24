---
layout: post
title: JSP & Servlet - servlet request, response
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Servlet request, Servlet Response

사용자 요청(request)과 웹 서버의 응답(response)을 담당하는 객체에 대해 학습

자바는 객체지향 프로그램이기 때문에 서버로부터 사용자가 브라우저를 통해 어떤 요청을 보낼 수 있다. (예로들자면 검색 사이트에서 검색어를 치고 검색결과를 요청한다든지, 아이디/패스워드 입력 후 로그인 혹은 회원가입을 요청한다든지 등) 이렇듯 서버로부터 어떤 데이터를 주고 받기를 원하는 과정속에서 자바는 객체 지향이기 때문에 이 모든 데이터들을 객체로 만들어 주고받는 과정을 갖는다.

이때 `요청을 request객체라고 하고 응답을 response객체`라고 한다.


### HttpServlet

HttpServlet는 추상클래스로 아래의 다이어그램을 살펴보자.
<center>
<figure>
<img src="/assets/post-img/java/servlet.png" alt="" width="100%" height="50%">
<figcaption>HttpServlet class 다이어그램</figcaption>
</figure>
</center>

- ServletEx: 개발자가 만든 서블릿으로 개발 목적에 맞게 코딩을 한다.
- HttpServlet: 개발자가 만든 서블릿에는 반드시 HttpServlet이 상속되어져 있어야한다.
- GenericServlet: HttpServlet이 상속받은 클래스
- Servlet, ServletConfig, Serializable: GenericServlet이 구현한 인터페이스들

하나의 서블릿에는 다양한 추상클래스들과 인터페이스들을 상속받고 있음을 볼 수 있는데, 이 이유는 우리가 현재 작업하고 있는 환경은 로컬이 아니고 웹서버이기 때문이다. 즉 웹서버(웹 어플리케이션 서버와의 통신, 프로토콜을 통한 통신)을 하기 위한 과정에서는 다양한 데이터들이 오고갈 수 있기 때문에 이 수많은 데이터들을 다루기 위해서는 다양한 기능들이 필요하다. 즉, 이미 그 다양한 기능들이 구현되어져있는 클래스 혹은 인터페이스들을 상속받아옴으로써 개발자는 쉽게 기능들을 사용할 수 있게 되는것이다.

**오로지 HttpServlet를 상속받아옴으로써 말이다!**

### HttpServletRequest, HttpServletResponse

```java
@WebServlet("/SE")
public class ServletEx extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  }

  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    doGet(request, response);
  }
}
```

이처럼 ServletEx는 HttpServlet를 상속받고 실제 기능을 수행하는 메서드에서는 두개의 파라미터를 받는다 (request, response)

이 request, reponse객체를 통해 사용자로부터 요청을 받고 들어온 요청에 관한 처리를 해준다.


### HttpServletRequest 주요 메서드

- request.getCookies();
- request.getSession();
- request.getAttribute(null);
- request.setAttribute(null, null);
- request.getParameter(null);
- request.getParameterNames();
- request.getParameterValues(null);

### HttpServletResponse 주요 메서드

- response.addCookie(null);
- response.getStatus();
- response.sendRedirect(null);
- response.getWriter();
- response.getOutStream();
