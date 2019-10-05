---
layout: post
title: JSP & Servlet - session(세션)
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>


## Session (세션)?

쿠키에서 설명했듯 http프로토콜의 단점 즉, 브라우저에서 어떤 요청을 하고 서버에서 응답을 하고나면 해당 연결을 해제해버리는 이 단점 때문에 장바구니의 상품정보, 로그인 정보 등이 날아갈 수 있으니 이 연결을 유지시키기 위해 쿠키를 사용한다. 쿠키는 클라이언트 브라우저에 생성되고 저장되어 사용하는것이지만, **세션은 서버에 생성하고 서버의 기존 연결정보를 저장함으로써 클라이언트와 서버가 지속적으로 연결을 유지할 수 있도록 도와준다.**

쿠키와 세션 모두 브라우저(클라이언트)와 서버의 연결을 유지시켜주는 수단이지만, 쿠키는 클라이언트 브라우저에 생성이 되고 저장이 된다면 세션은 서버에 생성이 되고(웹 컨테이너에 생성이 되고) 그 정보가 서버에 그대로 저장이 되는 방식이다.

공통적인 것은 http프로토콜이기에 한번 요청을 하고 응답을 하고나면 연결이 해제되는 것이고 이 재연결을 유지시켜주기 위해 과거의 연결을 다시한번 연결시켜주기 위한 수단으로 쿠키와 세션을 사용한다고 생각하면 된다.


## 세션 구현해보기

- 처음 요청이 들어오면 세션이 null인지 아닌지를 확인한다.
- null이라면 로그인 이력이 없었던 것으로 로그인 절차를 밟도록 유도한다.
- null이 아니라면 과거 로그인 이력이 있던 것으로 해당 로그인 정보를 다시 출력해준다.
- 로그아웃을 할때는 서버에 남아있는 세션은 더이상 필요없다는 의미로 invalidate()를 통해 지워준다.  

### login.jsp

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

  <form action="loginCon" method="post">
    ID: <input type="text" name="mID">
    PW: <input type="password" name="mPW">
    <input type="submit" value="login">
  </form>

</body>
</html>
```

### loginCon.java

- session객체는 httpSession이라는 인터페이스를 이용해 담도록 한다.
- httpSession을 통해 담은 session은 setAttribute를 통해 사용자가 입력한 ID를 밸류갑승로 한다.
- 즉, memberID라는 세션을 담은것을 의미한다.

```java
package com.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/loginCon")
public class LoginCon extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    PrintWriter out = response.getWriter();
    String mID = request.getParameter("mID");
    String mPW = request.getParameter("mPW");
    out.print(mID);
    out.print(mPW);

    HttpSession session = request.getSession();
    session.setAttribute("memberID", mID);

    response.sendRedirect("loginOK.jsp")
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

### loginOK.jsp

- setAttribute를 통해 담은 session을 getAttribute를 통해 저장
- 로그아웃도 만들어보자

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

<%
session = request.getSession();
out.print("memberID :" + session.getAttribute("memberID") + "<br>");
%>

<form action="logoutCon" method="post">
  <input type="submit" value="logout">
</form>

</body>
</html>
```

### logoutCon.java

- 세션을 날려버리는것은 invalidate()를 통해서 한다.

```java
package com.servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/logoutCon")
public class LogoutCon extends HttpServlet {


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    HttpSession session = request.getSession();
    session.invalidate();

    response.sendRedirect("login.jsp");

	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

### login.jsp

- 기존 세션이 있었는지 확인하는 코드를 추가해보자

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

  <%
  if(session.getAttribute("memberID") != null)
    response.sendRedirect("loginOK.jsp");
  %>

  <form action="loginCon" method="post">
    ID: <input type="text" name="mID">
    PW: <input type="password" name="mPW">
    <input type="submit" value="login">
  </form>

</body>
</html>
```

<hr>

세션과 쿠키는 http 프로토콜에서 클라이언트와 서버를 연결해주는 수단으로 많이 사용되며 요즘 추세는 세션이다. 왜냐하면 쿠키의 단점이 워낙 치명적이기 때문에! 그래서 최근은 쿠키보다 세션을 더 선호하는 경향이 있다!

<hr>
