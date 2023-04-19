---
layout: post
title: JSP & Servlet - jsp implict object (jsp 내장객체-config, application, out, exception)
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## jsp 내장객체

request, response 객체 이외에 jsp에서 기본적으로 제공하는 객체

### config

web.xml에 데이터를 저장(명시)해놓고 getInitParameter()를 통해 jsp에서 데이터를 공유하는 방식

#### web.xml

- web.xml을 이용해 config(서블릿)에서 공유할 수 있는 데이터를 저장
- jsp는 컴파일 시점에 서블릿으로 변환되어 사용하기 때문에 jsp먼저 만들고 데이터를 공유함
  - web.xml을 통해 데이터를 공유하는 방식
  - 따라서 jspEx.jsp를 만들어 공유할것이기 때문에 web.xml에 서블릿 등록을 한다.

```xml
<servlet>
  <servlet-name>servletEx</servlet-name>
  <jsp-file>/jspEx.jsp</jsp-file>
  <init-param>
    <param-name>adminID</param-name>
    <param-value>admin</param-value>
  </init-param>
  <init-param>
    <param-name>adminPw</param-name>
    <param-value>1234</param-value>
  </init-param>
</servlet>
<servlet-mapping>
  <servlet-name>servletEx</servlet-name>
  <url-pattern>/jspEx.jsp</url-pattern>
</servlet-mapping>
```

- 서블릿을 자바파일이 아닌 jsp로 만들기 때문에 `jsp-file`태그 사용
- 서블릿에서 초기화 파라미터(`init-param`)를 만들어줘야함
- 개발자가 현재 작업하고 있는 페이지(jsp페이지)는 서블릿으로 변환되어 웹 컨테이너로부터 작업을 함
  - 사용자로부터 요청이 오면 서블릿으로 작업을 함
  - 이 서블릿을 web.xml에 작업(명시)해줬음!

#### jspEx.jsp

- `adminId, adminPw` 변수선언을 해줌
  - 데이터 값 가져온것을 저장하기 위해
- `config` 내장객체: jsp에서 별도로 명시하지 않아도 얼마든지 사용가능한 객체
  - web.xml에 명시해준 adminId, adminPw를 가져오는 것이 가능

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
   pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
  <head>
      <meta charset="EUC-KR">
      <title>Insert title here</title>
  </head>
  <body>
  <%!
  String adminId;
  String adminPw;
  %>

  <%
  adminId = config.getInitParameter("adminId");
  adminPw = config.getInitParameter("adminPw");
  %>

  <p> adminId: <%= adminId %> </p>
  <p> adminPw: <%= adminPw %> </p>
  </body>
</html>
```

실제로 브라우저를 통해 출력값을 확인해보면 web.xml에 초기화 파라미터(init-param)로 만든 admin, 1234가 정상적으로 출력하는 것을 확인할 수 있다.

위 코드에서 확인했듯이, config 객체를 통해 초기화 파라미터 데이터를 가져오기 위해서는 해당 서블릿 안에 `getInitParameter`로 명시가 되어있어야만 한다. (즉, 이 서블릿에서만 jspEx.jsp 에서만 데이터를 사용할 수 있다는 것!)


### application

web.xml에 `context-param`을 통해 프로젝트 전체, 즉 어플리케이션 전체에 데이터를 공유하는 방식

즉, web.xml에 데이터(파라미터)를 저장해놓고 `getInitParameter`를 통해 모든 jsp, 즉 하나의 서블릿이 아닌 어떤 서블릿이라도 어플리케이션 안에 있다면 데이터를 모두 가져다 쓸 수 있는 방법을 제공한다.

어플리케이션 전체에 적용해야하기 때문에 `context-param`이라는 태그를 이용해 name, value를 지정해주고 context-param을 이용해 언제나 모든 서블릿에서 어플리케이션 통째로 공유할 수 있는 데이터를 저장해놓고 사용한다.

**application객체로부터 어떤 객체를 가져올때는 반드시 String으로 변환해 데이터를 가져와야한다.**


#### web.xml

- 이미지의 경로를 지정해놓고 한곳으로 모을때 사용
- 어떤 서블릿에서도 이 이미지를 사용할 수 있도록 함
- 개발자가 공유하고자 하는 즉, 어플리케이션에서 전체적으로 사용하고 서블릿이 공유되어 쓸수 있는 데이터 저장

```xml
<context-param>
  <param-name>imgDir</param-name>
  <param-value>/upload/img</param-value>
</context-param>
<context-param>
  <param-name>testServerIP</param-name>
  <param-value>127.0.0.1</param-value>
</context-param>
```

#### jspEx.jsp

- context-param에서 데이터를 가져올때는 `application` 내장객체를 사용
- 반복적으로 이야기했지만, context-param에 데이터를 저장하는 것은 어플리케이션에서 공통적으로 사용하는 정보들을 web.xml에 저장해놓고 공유해서 사용하는 것을 의미한다.

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
   pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
  <head>
      <meta charset="EUC-KR">
      <title>Insert title here</title>
  </head>
  <body>
    <%!
    String imgDir;
    String testServerIP;
    %>

    <%
    imgDir = application.getInitParameter("imgDir");
    testServerIP = application.getInitParameter("testServerIP");
    %>

    <p> imgDir: <%= imgDir %></p>
    <p> testServerIP: <%= testServerIP %></p>
  </body>
</html>
```

<hr>
## application객체와 config객체 비교

`config`객체: `init-param`으로 데이터를 저장후 `getInitParameter`로 해당 서블릿에서만 데이터가 공유 가능
`application`객체: `context-param`으로 데이터 저장후 `getInitParameter`로 어플리케이션 전체에 데이터를 공유 가능
<hr>

### getAttribute() & setAttribute()

application객체는 해당 프로젝트 전체를 가리킨다고 했는데 setAttribute()를 통해 속성을 저장하고 getAttribute()를 통해 속성을 가져오는 것 또한 가능하다.

#### jspEx.jsp

- application객체를 이용해 setAttribute()해줌
- 즉, connectedIP라는 이름으로 165.62.58.23이라는 데이터를 속성으로 어플리케이션 객체에 set!

```html
<%
application.setAttribute("connectedIP", "165.62.58.23");
application.setAttribute("connectedUser", "hong");
%>
```

#### jspExGet.jsp

- 모든 어플리케이션 전체에서 공유하는 것은 getAttribute으로 출력 가능함!

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
   pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="EUC-KR">
        <title>Insert title here</title>
    </head>
    <body>

		<%!
    String connectedIP;
    String connectedUser;
    %>

    <%
    connectedIP = application.getAttribute("connectedIP");
    contentType = application.getAttribute("connectedUser");
    %>

    <p>connectedIP: <%= connectedIP %></p>
    <p>connectedUser: <%= connectedUser %></p>

    </body>
</html>
```

### out

`out.print`를 제공해주는 것으로 html코딩하듯 `html`코드 출력이 가능

### jspEx.jsp

```html
<%
out.print("<h1>hello JAVA world<h1>");
out.print("<h2>hello JSP world</h2>");
%>
```

### exception

에러가 발생했을 때 에러처리를 해주는 객체

#### jspEx.jsp

- 페이지 지시어 설정
  - 페이지 지시어를 통해 에러페이지를 명시해줘야 함
  - 이 jsp페이지 내에서 에러가 발생하면 errorPage.jsp로 보내달라는 의미

- 아래 코드에서 exception을 발생시켜보자!

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
   pageEncoding="EUC-KR"%>
<%@ page errorPage="errorPage.jsp" %>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="EUC-KR">
        <title>Insert title here</title>
    </head>
    <body>

		<%!
    Sting str;
    %>

    <%
    out.print(str.toString());
		%>
    </body>
</html>
```

#### errorPage.jsp

- 페이지 지시어 설정
  - 디폴트는 `false`인데, exception페이지의 경우 `true`로 명시해줘야함!

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
   pageEncoding="EUC-KR"%>
<%@ page isErrorPage="true"%>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="EUC-KR">
        <title>Insert title here</title>
    </head>
    <body>

		<%
			response.setStatus(200);
      String msg = exception.getMessage();
    %>
    <h1> errorMessage: <%= msg %></h1>

    </body>
</html>
```
