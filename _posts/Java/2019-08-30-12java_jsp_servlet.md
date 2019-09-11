---
layout: post
title: JSP & Servlet - JSP와 Servlet 간단 맛보기
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## JSP (Java Server Page)

개발자(jsp파일 생성) -> jsp.java -> jsp.class -> .obj -> html로 사용자에게 응답

실제로 개발자가 해야하는 일은 jsp 파일을 만들어주는거에만 그치지만(jsp파일을 만들어 서버에 올려놓으면 알아서 컴파일, 링크 작업을 통해 사용자에게 html파일로 응답을 해준다.) 이 jsp파일이 사용자에게 전달되기까지에는 여러 과정을 거치게 된다.

우리가 흔히 말하는 웹서버, 즉 웹어플리케이션 서버(WAS)안에는 웹 컨테이너가 있다.

이 웹 컨테이너가 하는일은 아래와 같다.

- 개발자가 만든 jsp파일을 가지고 기계가 이해할 수 있는 jsp.java파일을 만들며
- 이 jsp.java파일을 컴파일을 통해 jsp.class 파일을 만든다.
- 이 jsp.class파일을 링크작업을 통해 obj파일로 만든 다음
- 실제 서버에서 구동이 되어(JVM, 자바 환경내에서 실행) 특정한 해당 페이지의 작업이 이루어지고
- 작업이 이루어진 결과물은 사용자에게 응답이 될때 html 파일로 보여지게 한다.


### JSP파일 작성

IntelliJ에서 jsp 파일을 작성하는 것은 html 기본하에 jsp 문법을 추가한 정도이다. html파일과 jsp파일 별도로 나누는 별개의 문서가 아니라 html파일 문서의 확장자를 jsp파일로 변경하면 된다. 즉, html 파일에서 jsp 문법을 추가해주면 웹 컨테이너에서 컴파일과 링크작업 모두 이루어진다.

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
    안녕하세용~~
  </body>
</html>
```

<html>에서 </html>까지는 기본 html 문법과 똑같은 것을 볼 수 있다. 이 부분을 제외한 맨 윗줄은 페이지 지시어인데, 웹 컨테이너에서 컴파일되고 링크되는 동적파일이라고 웹 컨테이너에게 '이 파일은 어떤 파일입니다'하고 지시를 해주는 지시어이다. (jsp 문법임)

**이 페이지가 어떤 성격을 갖고있다는 것을 웹 컨테이너에게 알려주는 jsp 문법의 지시어**

그런데 우리가 보는 화면 이렇게 jsp문법과 html 문법이 섞여있지만, 실제 사용자에게 보여지는 화면은 html 파일로 나오는 것을 확인할 수 있다 (-> 개발자 도구 확인, mac = cmd+opt+i)

즉, 사용자에게는 jsp파일을 보여줄 필요가 없고 응답만 해서 사용자에게는 결과물(정적인 파일)만 보여주면 된다.


그리고 jsp파일을 수정할 경우 수정 후 서버를 재 실행하지 않아도 바로바로 브라우저에서 수정이 된것을 확인이 가능하다.


## Servlet

jsp와는 다르게 servlet은 순수 자바 파일만을 이용한다.

- 사용자는 브라우저를 통해 서버에 요청을 한다.
- 웹컨테이너에는 개발자가 만든 java 파일을 가지고 컴파일을 하여 class 파일을 만든다.
- 해당 class 파일은 링크과정을 거쳐 obj파일로 만들어진 다음
- 사용자에게 응답을 한다.

이때 개발자는 자바 파일만을 만들어주면 되는데, 이 자바파일을 만듦으로써 자동으로 웹 컨테이너에서 컴파일을 해주고 obj파일을 만들어 사용자에게 온 요청으로부터 응답을 해주는 것이다.

jsp에서는 jsp 파일을 만들어 html 안에 jsp 문법을 넣었다면 servlet에서는 순수 자바 파일만을 이용한다는 것이 큰 차이점이다.

```java
/**
 * Servlet implementation class HelloServlet
 */
@WebServlet("/Hello")
public class HelloServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub

		PrintWriter out = response.getWriter();
		//out.append("Served at: ").append(request.getContextPath());
		out.print("<html>");
		out.print("<head>");
		out.print("</head>");
		out.print("<body>");
		out.print("Hello Servlet~");
		out.print("</body>");
		out.print("</html>");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```
