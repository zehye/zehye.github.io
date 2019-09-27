---
layout: post
title: JSP & Servlet - servlet 데이터 공유(servlet parameter, context parameter)
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Servlet 데이터 공유

config, application 객체들이 servlet에서는 어떻게 사용되는지 살펴보자

### Servlet parameter

web.xml에서 `init-param(초기화 파라미터)` 즉, 서블릿이 초기화될때 파라미터를 만들어주고 그것을 서블릿에서 서블릿이 만들어질때 `getInitParameter`를 통해 데이터를 가져와 사용이 가능하다.

서블릿에서 `init-param` 태그를 이용해 내가 이용하고자하는 데이터를 name, value 쌍으로 만들어주면 실제 서블릿(자바코드)에서는 `getServletConfig()`를 통해 config객체를 가져오고 그 config객체로부터 `getInitParameter`로 초기화파라미터로 생성한 데이터를 가져와 사용이 가능한 것이다.  

#### web.xml

- web.xml에서 url-mapping을 해줌
- 서블릿 생성후 `init-param`을 이용해 초기화파라미터 생성
  - 해당 서블릿이 웹 컨테이너에서 초기화될때 `adminId, adminPw(초기화데이터)` 즉, 공유될 데이터가 생기게 된것

```xml
<servlet>
  <servlet-name>servletEx</servlet-name>
  <servlet-class>com.servlet.servletEx</servlet-class>
</servlet>
<init-param>
  <param-name>adminId</param-name>
  <param-value>admin</param-value>
</init-param>
<init-param>
  <param-name>adminPw</param-name>
  <param-value>1234</param-value>
<init-param>
<servlet-mapping>
  <servlet-name>servletEx</servlet-name>
  <url-pattern>/se</url-pattern>
</servlet-mapping>
```

#### servletEx.java

- 저장된 데이터를 서블릿에서 가져와 사용해보자.
- `getServletConfig()`를 통해 `config`객체를 가져옴
- `getInitParameter()`를 통해 `adminID의 밸류값`에 해당하는 값을 가져옴
- 서블릿에서 출력하기 위해 `response`는 객체로부터 `getWriter()`를 통해 가져옴

- url-mapping값이 `/se`로 되어있으니 도메인에 `/se`를 붙이면 정상적으로 화면에 출력됨!

```java
public class servletEx extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    Sting adminId = getServletConfig().getInitParameter("adminId");
    Sting adminPw = getServletConfig().getInitParameter("adminPw");

    PrintWriter out = response.getWrtier();
    out.print("<p>adminID"+adminId+"</p>");
    out.print("<p>adminPw"+adminPw+"</p>");    
  }
}
```

### context parameter

#### web.xml

```xml
<context-parma>
  <param-name>imgDir</param-name>
  <param-value>/upload/img</param-value>
</context-param>
<context-param>
  <param-name>testServerIP</param-name>
  <param-value>127.0.0.1</param-value>
</context-param>
```

#### servletEx.java

```java
public class servletEx extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    Sting imgDir = getServletContext().getInitParameter("imgDir");
    String testServerIP = getServletContext().getInitParameter("testServerIP");

    PrintWriter out = response.getWriter();
    out.print("<p>imgDir"+imgDir+"</p>");
    out.print("<p>testServerIP"+testServerIP+</p>);
  }
}
```

### context attribute - getAttribute() & setAttribute()

set을 하기위해 `getServletContext()`를 통해 context를 가져오고 `setAttribute()`를 통해 속성을 저장한다. 즉, 접속되어 있는 IP는 몇번이다. -> `getServletContext().setAttribute("connectedIP", "165.62.38.23");`

이렇게 connectedIP를 context에 저장을 했고, 하나더 추가해서

`getServletContext().setAttribute("connectedUser", "hong");`를 함으로써 누가 접속했는지 하나더 저장해본다! 이렇게 하면 현재 context에는 두개의 속성이 저장되어있는건데 다른 서블릿에서 이 데이터들이 정상적으로 가져와지는지 확인해보자.

#### servletGet.java

- 새로운 서블릿 하나를 생성해본다.
- `getServletContext()`를 통해 저장한 데이터를 가져온다.
- 우리가 가져와야 할 데이터는 Sting이기에 형변환을 해준다.

```java
@WebServer("/seg")
public class servletEx extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    Sting connectedIP = (Sting)getServletContext().getAttribute("connectedIP");
    String connectedUser = (Sting)getServletContext().getAttribute("connectedUser");

    PrintWriter out = response.getWriter();
    out.print("<p>connectedIP"+connectedIP+"</p>");
    out.print("<p>connectedUser"+connectedUser+"</p>");
  }
}
```

`/se`로 가면 connectedIP, connectedUser 데이터가 context에 저장될 것이고 `/seg`에 가면 정상적으로 데이터가 저장되어 출력된 것을 확인할 수 있다.


<hr>
정리하자면, 서블릿에서 데이터를 공유하는 방법은 `servlet parameter`, `context parameter`가 있다.

- `servlet parameter`: 해당 서블릿에서만 데이터를 공유하는 방법
- `context parameter`: 어플리케이션 전체에 데이터를 공유하는 방법

더 나아가, context는 context parameter에 지정해놓고 데이터는 저장하는 방법과 get, set을 이용해 정보를 저장해놓고 공유하는 방식도 있다.

<hr>
