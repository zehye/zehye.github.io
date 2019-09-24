---
layout: post
title: JSP & Servlet - jsp request, response
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## JSP request, response

사용자가 어떤 요청을 하면 그 요청을 담고있는 request객체와 응답을 할때 사용되는 데이터를 관리해주는 response객체가 있는데 servlet과 마찬가지로 jsp도 동일하게 request, response객체가 존재한다.

차이는 단지 이 객체가 servlet에서 작동되느냐 jsp에서 작동되느냐의 차이일 뿐이다.

### request 객체

- 사용자로부터 폼을 통해 데이터를 받음
- 폼에서 action을 통해 어떤 jsp파일로 데이터를 넘길지 지정
- method를 통해 사용자가 입력한 데이터를 post or get으로 받을지 지정

```html
<form action="first.jsp" method="get">
  user data
  <input type="submit" name="" value="">
</form>
```

```jsp
<%
  m_name = request.getParameter("m_name");
  m_pass = request.getParameter("m_pass");
  m_hobby = request.getParameterValues("m_hobby");
%>
```


### response 객체

- 뷰한테 응답하기 위해 사용하기 위한 객체
- 사용자로부터 들어온 요청을 어디로 응답할지를 정해줌

#### firstPage.jsp
```html
<body>
  First Page </br>
  <%
  response.sendRedirect("secondPage.jsp");
  %>
</body>
```

#### secondPage.jsp
```html
<body>
  Second Page
</body>
```

위 코드는 사용자로부터 firstPage에 요청이 들어오면 secondPage로 보내는 응답을 한다는 것

**즉, localhost:8090/firstPage.jsp가 입력되면 바로 localhost:8090/secondPage.jsp로 넘어가는 것을 볼 수 있다. **
