---
layout: post
title: JSP & Servlet - form 데이터 처리(get, post)
category: JSP&Servlet
tags: [JSP&Servlet]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Form 태그

사용자가 브라우저(스마트폰, pc 등)에서 서버(웹 컨테이너)로 데이터를 보낼때
- 로그인
- 회원가입
- 설문지 등과 같이 폼을 통해 정보를 입력하고 전송

사용자가 입력한 다양한 정보(데이터)는 request객체로 뭉쳐져 웹 어플리케이션 서버(웹 컨테이너서버)로 날아감

즉, 브라우저에서 사용자가 작성한 폼 데이터들은 request객체들로 묶여 서버쪽으로 전달이 된다.

이때 서버에서 데이터를 받을때 두개의 메서드가 존재한다 -> `doGet, doPost`

### doGet

form태그 속성에는 `action, method` 두개가 있는데, 이때 메서드의 속성방식을 `get`으로 하면 사용자로부터 전달되는 데이터는 `doGet`메서그가 받게된다. 즉, 폼태그에서 메서드 속성에 `get`을 적으면 유저 데이터가 서버로 전송될때 서버에서 `doGet`메서드가 해당 데이터를 받는다는 것이다.

### doPost

doGet메서드와 마찬가지로 form에서 메서드를 `post`로 설정을 하면, 브라우저에서 넘어오는 유저의 정보 및 데이터를 `doPost`메서드로 받는다는 것을 의미한다.


## doGet과 doPost의 차이점?

- get방식의 경우 유저의 데이터가 웹 브라우저 url에 그대로 노출되어 서버로 전송이 된다.
  - 즉, 유저가 폼에서 입력한 정보가 url에 그대로 나열되는 것을 의미
  - `예) http://localhost:8090/mSignUP?m_name=jihye&m_password=1234`
  - 사용자가 입력한 정보가 그대로 노출됨으로써 외부에서 누군가 데이터를 가로챌 가능성이 높음으로 보안에 취약
  - 그리고 텍스트의 길이가 굉장히 지저분해지고, 길이의 한계가 있어 너무 많은 정보를 날리기에는 한계가 존재

- post방식의 경우 url이 굉장이 심플하게 넘어간다(=서블릿 매핑한 정보만 나옴)
  - `예) http://localhost:8090/mSignUP`
  - 따라서 보안에 굉장히 강함
  - 사용자의 데이터가 헤더파일에 암호화되어 전송(데이터가 http request에 포함되어 웹서버로 전송)
  - 따라서 로그인, 회원가입, 설문조사 등은 모두 post방식으로 전송된다.

만약 우리가 폼에서 속성을 지정해주지 않는다면 디폴트는 `get`방식이다.


### 예시

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<form action="mSignUp" method="post">
		name : <input type="text" name="m_name"> </br>
		password : <input type="password" name="m_pass"></br>
		gender : Man<input type="radio" name="m_gender" value="M" checked="checked">, Woman<input type="radio" name="m_gender" value="W"></br>
		hobby : Sport<input type="checkbox" name="m_hobby" value="sport">,
				Cooking<input type="checkbox" name="m_hobby" value="cooking">,
				Reading<input type="checkbox" name="m_hobby" value="reading">,
				Travel<input type="checkbox" name="m_hobby" value="travel"></br>
		residence : <select name="m_residence">
						<option value="seoul" selected="selected">Seoul</option>
						<option value="gyeonggi">Gyeonggi</option>
						<option value="chungcheong">Chungcheong</option>
						<option value="jeonra">Jeonra</option>
						<option value="jeju">Jeju</option>
						<option value="gyeongsang">Gyeongsang</option>
						<option value="gangwon">Gangwon</option>
					</select></br>
					<input type="submit" value="sign up">
	</form>

</body>
</html>
```

```java
@WebServlet("/mSignUp")
public class MemSignUp extends HttpServlet {

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		System.out.println(" -- doGet() -- ");

		String m_name = request.getParameter("m_name");
		String m_pass = request.getParameter("m_pass");
		String m_gender = request.getParameter("m_gender");
		String[] m_hobbys = request.getParameterValues("m_hobby");
		String m_residence = request.getParameter("m_residence");

		System.out.println("m_name : " + m_name);
		System.out.println("m_pass : " + m_pass);
		System.out.println("m_gender : " + m_gender);
		System.out.println("m_hobbys : " + Arrays.toString(m_hobbys));
		System.out.println("m_residence : " + m_residence);

		Enumeration<String> names = request.getParameterNames();
		while (names.hasMoreElements()) {
			String name = (String) names.nextElement();
			System.out.println("name : " + name);
		}

	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		System.out.println(" -- doPost() -- ");

		doGet(request, response);
	}

}
```

## 위 코드 정리

- mSignUP으로 매핑되어있는 서블릿을 찾아간 뒤 폼에서 지정해준 메서드가 post이기 때문에 doPost()로 찾아감
- doPost()의 파라미터에는 request, response가 있는데 이때 request객체에는 사용자가 입력한 정보들이 헤더파일(http헤더파일)에 암호화되어 들어가 있다.
- 이때 유저가 전송버튼을 누르는 순간 입력된 모든 정보들이 doPost()로 들어오게 되고
- 해당 코드에서 doPost()에서 doGet으로 보내기 때문에 doGet()으로 보내짐
  - 코드를 한쪽으로 몰아넣는 방식으로 자주 사용되는 방식이다.

- 서버에서 사용자가 입력한 정보를 받을때 doGet()혹은 doPost()로 받는데, 이 정보들은 request객체안에 들어가 있고 이 객채의 메서드 중에서 `getParameter`가 있다.
  - getParameter는 request객체에서 값 하나만을 뽑아올때 사용한다.
  - 즉, 위 코드에서 보자면 getParameter를 이용해 m_name의 값을 뽑아오는 것이다.

- 값이 여러개인경우 여러개의 값을 뽑아오기 위해서는 `getParameterValues`를 사용한다.
  - getParameterValues를 사용하는 경우 해당 값들은 배열로 반환된다.

- 추가적으로 서버 입장에서 프론트에서 어떤 값들이 들어오는지(어떤 파라미터들이 모두 넘어오는지) 궁금할 경우 `getParameterNames`를 사용한다.
  - 즉, 입력한 값이 아닌 속성값이 모두 넘어오게 된다.
