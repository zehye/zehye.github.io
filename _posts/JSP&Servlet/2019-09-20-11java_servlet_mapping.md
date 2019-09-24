---
layout: post
title: JSP & Servlet - servlet 맵핑
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

일반적으로 웹 프로그램을 만들기 위해서 jsp를 사용하기도 하고 servlet을 사용하기도 하는데, 웹 프로그램을 만들기 위해 제일 좋은 방법은 jsp와 servlet을 섞어 만드는 것이다. jsp를 통해 뷰를 만들고 servlet을 통해 컨트롤러, 모델 등을 만드는 것이 가장 이상적이다.

## servlet mapping

서블릿을 외부에서 요청하기 쉽도록 특정 문자를 이용해 매핑하는 방법

개발자는 어떠한 목적을 가지고 순수 자바파일을 만드는데, 만든 그 파일을 웹 컨테이너에 넣어놓고 사용자가 브라우저를 통해 어떠한 요청(request)를 하면 수십개의 서블릿중에 사용자의 요청에 가장 적절한 서블릿이 작동/응답(response)를 해주게 된다.

일반적으로 서블릿은 수십/수백 혹은 그 이상개인데 이때 중요한 점은 브라우저가 정확히 어떤 서블릿에 요청을 하는지가 중요한 부분이다. 그렇기 때문에 해당 서블릿은 각각 고유하고 중복되지 않은 이름을 가져야하며 그 이름을 통해 브라우저에서 서블릿을 쉽게 찾을 수 있도록 해줘야한다.

### servlet full path

일반적으로 서블릿의 full path는 아래와 같다.

```
프로토콜/도메인/포트번호(생략가능)/프로그램루트(context path)/servlet/패키지명을 포함한 서블릿의 풀네임
http://localhost:8090/pjt001/servlet/com.servlet.ServletEx
```

근데 저 full path를 보면 누가봐도 너무 길고 복잡하며 보완에도 취약하다는것을 알 수 있다.(해당 서블릿이 어디에 있는지 도메인에서 다 확인이 가능함으로)

따라서 해당 path를 줄여 매핑을 하는데, 매핑을 한다는것은 간략한 `unique nickname`을 지정해주는 것으로 깊고 복잡하며 보완에까지 취약했던 full path를 간결하게 만들어줄 수 있게 되는 것이다.

### servlet mapping 방법 - web.xml / java annotation

### 1) web.xml - 배치지시자. 웹 환경설정을 담당하는 파일

서블릿 매핑 방법에 있어서 가장 고전적이고 선호하는 사람이 많은 방법


#### srv/com.srvlet/ServletEx.java


```html
<servlet>
  <servlet-name>ServletEx</servlet-name>
  <servlet-class>com.servlet.ServletEx</servlet-class>
</servlet>

<servlet-mapping>
  <servlet-name>ServletEx</servlet-name>
  <url-pattern>/SE</url-pattern>
</servlet-mapping>
```

// 서블릿 생성
* servlet-name: 임의의 닉네임을 만들어준다.
* servlet-class: 패키지명을 포함한 실제 자바파일의 이름을 풀패스로 적어줌

// 서블릿 매핑
* servlet-name: 위에서 정한 닉네임을 연결
* url-pattern: /SE로 매핑

즉, /SE로 호출을 하면 servletEx라는 서블릿을 호출한 것으로 응답

서블릿을 만들어놓고 사용자에게 요청을 해달라고 설정하는것은 web.xml에 들어가 서블릿을 정의하고 매핑까지 진행해주면 가능해진다.

### java annotation

좀더 현대적이고 요즘 많이 사용되는 추세

해당 서블릿 위에 `@WebServlet(/SE)`

```java
@WebServlet("/SE")
public class ServletEx extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    PrintWriter out = response.getWriter();
    out.print("<html>");
    out.print("<head><title>ServletEx</title></head>");
    out.print("<body>");
    out.print("Hello Servlet");
    out.print("</body>");
    out.print("</html>");
  }
}
```

즉, java annotation 다음에 request에서 원하는 값을 url mapping값으로 바로 지정해주는 것이 방법으로 요즘 많이 사용되는 추세이다.

즉, url mapping은 full path가 복잡하고 보안에 취약하기 때문에 이를 보완하기 위해 사용하는 것으로 이러한 url mapping을 잘 할수 있어야 사용자의 요청에 제대로된 응답을 할수 있다.
