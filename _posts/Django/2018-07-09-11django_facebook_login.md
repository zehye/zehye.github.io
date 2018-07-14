---
layout: post
title: Django - facebook login
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Oauth 2.0

Oauth에서 가장 중요한 개념은 **Consumer(우리가 만든 어플리케이션,사이트), User, Service provider** 이다.

어떤 사이트에 User를 가입시킬때, 우리가 만든 인증문서를 사용하는게 아니라, 트위터, 페이스북 등이 만든 인증문서를 사용하는것을 의미한다.

Consumer가 트위터나 페이스북에 User의 정보를 가져오려면 우선 User의 해당 사이트에 대한 아이디와 패스워드를 가져올 수 있어야 한다. 그러나 이는 보안상 아주 큰 문제이다. (상식적으로 새로운 곳에 가입하기 위해 페이스북 연동 아이디를 가져오는데, 그 사이트의 개발자가 내 페이스북 아이디랑 비밀번호를 다 알고 있다고 하면 엄청난 소름이 아닌가?!) 더군다나 Consumer즉, 써드파티 중 어느 하나라도 피싱 사이트라고 한다면 정말 보안상의 큰 사고가 나는것이다.

그렇기 때문에 API에서도 User의 정보를 Consumer에게 주고싶지는 않을것이다. 그럼에도 불구하고 User는 API를 통해서 Consumer의 인증정보를 보내서 인증에 성공하고 싶어하는데, 이럴때 어떻게 해야하는 걸까?

User는 Consumer에 인증을 받기 위해 페이스북 로그인을 버튼을 누르고 그때 Consumer는 페이스북과의 연결을 이어준다. 즉, 페이스북 사이트로 연결을 이어주면 User는 Consumer를 통해 페이스북에 접근할 수 있게 된다.

이동을 해서 User가 로그인을 하면 페이스북은 User에게 'Consumer에게 어떠한 권한을 줄것이다'라고 물어보는데, 그때 확인을 누르면 페이스북은 Consumer에게 User에 대한 권한값을 넘겨준다. **(인증 토큰)**

> 이 인증토큰에는 해당 User에 해당하는 어떠한 권한들을 가지고 있다는 내용이 담겨있고, 그 받은 토큰을 가지고 페이스북에게 요청을 하게 된다. (현재 받은 토큰을 가지고 User의 로그인을 할 수 있게 도와달라는 요청)
그래서 페이스북은 해당 토큰이 User가 승인한 것과의 일치여부를 판단하고, 그래서 그 인증토큰의 User는 어떤 사람이다라는 것을 Consumer에게 알려준다.

그러면 우리는 Consumer는 그 토큰을 가지고, 이 User가 페이스북의 어떤 아이디를 쓰고 있구나라는 것을 알게되는데, 그것은 페이스북에 존재하는 User이름이고, **우리는 그 인증에 성공한 결과를 가지고 새로운 User를 가지고 새로운 db를 가지고 있어야한다.**

인증은 User가 페이스북의 인증을 했다는 것을 성공했다는 게 **Oauth** 가 하는일이고 그와는 별개로 인증에 성공을 했을 때, 특정 User에 대한 값은 우리쪽 데이터베이스에 담아놓아야 한다. 그래서 **우리쪽 데이터베이스에 담겨있는 User와 페이스북이 가지고 있는 User가 1대 1로 연결이 되어야 하는것** 이다.

> 즉, Consumer와 페이스북의 중개역할을 하는 것은 인증토큰이고 그 인증토큰을 받는 방법은 User가 우리 사이트(Consumer)를 통해서 페이스북에 로그인을 하는 것이다.

```
1. AnonymousUser가 페이스북 로그인 요청
2. 우리 사이트에서 페이스북 로그인 페이지로 redirect시켜줌 (우리 사이트가 해줌)
3. 페이스북 로그인이 완료되면 인증 토큰을 우리 사이트로 다시 redirect시켜줌 (페이스북이 해줌)
4. 받은 토큰을 페이스북 API를 사용해서 액세스 토큰으로 변환
5. 액세스 토큰으로 유저 정보를 요청
6. 받은 유저 정보를 사용해서 우리 사이트의 User를 생성
```

<center>
<figure>
<img src="/assets/post-img/django/oauth_flow.png" alt="">
<figcaption>ouath flow</figcaption>
</figure>
</center>

### Oauth 2.0이 되면서 개선된 사항

#### 용어변경

* Resource Owner : User, 사용자
* Resource Server : 우리 서버
* Authorization Server : 페이스북 API
* Client : Customer, 우리 서버

<hr>

## Facebook Login

자 이제, 페이스북에서 로그인을 하는 방법을 알아보도록 하자.
[facebook for developers](https://developers.facebook.com)

해당 사이트에 들어가 `새 앱 ID만들기`를 클릭한 뒤, 이름과 이메일을 설정한다. 설정한 뒤 나오는 페이지에서 `페이스북 로그인 - 설정`을 클릭!

설정에서 보이는 빠른 설정에 웹이 보이지만, 해당 웹은 자바스크립트이므로 우리는 파이썬을 통해 웹 서버를 만들것이기 때문에 기타에서 본다.

`로그인 플로 직접 빌드`를 사용할 것이지만 일단 우리는 문서를 보며 따라가도록 한다.

> [로그인 플로 직접 빌드](https://developers.facebook.com/docs/facebook-login/manually-build-a-login-flow)

### 사용자 로그인 유도

사용자가 앱에 로그인하지 않았거나 Facebook에 로그인하지 않은 경우 로그인 대화 상자를 사용하여 둘 다를 수행하도록 메시지를 표시할 수 있다. Facebook에 로그인하지 않은 경우 로그인하도록 메시지를 표시한 다음 앱에 로그인을 진행한다. 이는 자동으로 감지되므로 이 동작을 활성화하기 위해 어떠한 추가 작업도 수행할 필요가 없다.

#### 로그인 대화 상자 호출 및 리디렉션 URL 설정
```python
https://www.facebook.com/v3.0/dialog/oauth?
  client_id={app-id}
  &redirect_uri={redirect-uri}
  &state={state-param}
```

이를 위해서 일단 `settings.py`에서 `FACEBOOK_APP_ID`와 `FACEBOOK_APP_SECRET_CODE`를 작성해준다. **( > 내앱 > 설정 에서 확인 )**

우선 client_id는 위에서 설정한 facebook_app_id를 의미한다.

그리고 redirect_uri는 요청한 정보를 받아서 가지고 올 url을 의미한다. 위 주소에 dialog는 페이스북으로 넘어가는 링크를 의미하는데, 넘어간 페이스북에서 사용자가 로그인을 하면 어느 사이트에서 사용자의 정보를 쓰려고 한다는 정보가 넘어가고 그때 사용자가 확인을 누르고 우리사이트로 넘어와야 그 정보를 우리가 쓸 수 있다. 그때 돌아올 주소를 적는 것이 redirect_uri이다.

```html
<div>
	<form action="" method="POST">
		<input type="text" name="username">
		<input type="password" name="password">
		<button type="submit" class="btn btn-info">로그인</button>
	</form>

	<a href="https://www.facebook.com/v3.0/dialog/oauth?
	client_id= #FACEBOOK_APP_ID
  &redirect_uri=http://localhost:8000/members/facebook-login/"
  class="btn btn-primary">Facebook 로그인</a>
</div>
```

해당 형태를 나타낼 view만들기

```python
def facebook_login(request):
  code = request.GET['code']
  return HttpResponse(code)
```

이 view와 urls를 연결(주소는 redirect_uri에 있는 주소)

```python

app_name = 'members'
urlpatterns = [
    path('facebook-login/', views.facebook_login, name='facebook-login'),
]
```
그러면 하얀 화면에 인증 코드가 등장할 것이다. 그러면 이 코드를 이용해서 토큰을 얻어내야 한다.

#### 엑세스 토큰의 코드 교환

```python
GET https://graph.facebook.com/v3.0/oauth/access_token?
   client_id={app-id}
   &redirect_uri={redirect-uri}
   &client_secret={app-secret}
   &code={code-parameter}
```
client_secret은 `FACEBOOK_APP_SECRET_CODE`이고 code는 우리가 위에서 받은 코드를 의미한다.

그러면 이때 view에서 엑세스 코드(엑세스토큰, 진짜 사용자의 정보를 가지고 있는 것 - 비밀 통신이 이루어져야 한다.) 교환 앤드포인트에 HTTP GET요청 후, 결과의 response.text값을 HttpResponse에 출력해본다.

HTTP GET요청은 `requests` (코드 안에서의 GET요청)를 통해서 얻을 수 있다.

```python
def facebook_login(request):
  code = request.GET['code']

  url = 'https://graph.facebook.com/v3.0/oauth/access_token'
  params = {
      'client_id': settings.FACEBOOK_APP_ID,
      'redirect_uri': 'http://localhost:8000/members/facebook-login/',
      'client_secret': settings.FACEBOOK_APP_SECRET_CODE,
      'code': code,
  }
  response = requests.get(url, params)
  return HttpResponse(response.text)
```

이때 출력되는 데이터는 `json`데이터이다.

```
JSON은 JaveScript Object Notation의 약자로 자바스크립트에서 객체를 나타내기 위한 표기법을 뜻한다.
데이터를 전달하고 받을때의 표준으로 key, value로 보내는 데 이런 데이터를 전달하고 받을때의 loads가 적다.
플랫폼 상관없이 받아서 처리하는게 쉬워서 표준이 되었다.

어느 언어에서 받아도 쉽게 파싱이 가능하고, 기본적으로 문자열로 전달을 받아서 그 받은 문자열을 파이썬 객체로 다시 변환이 가능하다.

파이썬의 딕셔너리와 비슷하지만, 파이썬 객체 형식으로 바꿔줘야 한다.
```
결과값을 확인해본다.

```python
def facebook_login(request):
  code = request.GET['code']

  url = 'https://graph.facebook.com/v3.0/oauth/access_token'
  params = {
      'client_id': settings.FACEBOOK_APP_ID,
      'redirect_uri': 'http://localhost:8000/members/facebook-login/',
      'client_secret': settings.FACEBOOK_APP_SECRET_CODE,
      'code': code,
  }
  response = requests.get(url, params)
  # 파이썬에 내장된 json모듈을 사용해서 JSON형식의 텍스트를 파이썬 Object로 변환
  response_dict = json.loads(response.text)

  # 위와 같은 결과
  response_dict = response.json()
  # print(response_dict)
  # 여기서 access_token값만 꺼내서 HttpResponse로 출력
  return HttpResponse(response_dict['access_token'])
```

### user_id를 debug를 통해 가져오기

이제 받아온 access_token을 가지고 User를 특정화할 수 있다. 이 토큰을 이용하면 User의 페이스북 ID를 알아낼 수 있는데, 해당 User가 우리 어플리케이션을 통해서 페이스북에 인증을 하면 알아낼 수 있는 고유값이다.

이때 말하는 ID값은 그사람의 진짜 페이스북 ID나 이메일이 아니라, **사람을 구분하기 위한 유니크한 값** 을 의미한다. 근데 그 유니크한 값은 애플리케이션마다 다르게 나온다.

그러면 이제 debug_token에 요청을 보내고 그 결과의 'data'값을 HttpResponse로 출력해보자.

> input_token은 'access_token'이며 access_token은 {client_id}|{client_secret}

```python
def facebook_login(request):
  code = request.GET['code']

  url = 'https://graph.facebook.com/v3.0/oauth/access_token'
  params = {
      'client_id': settings.FACEBOOK_APP_ID,
      'redirect_uri': 'http://localhost:8000/members/facebook-login/',
      'client_secret': settings.FACEBOOK_APP_SECRET_CODE,
      'code': code,
  }
  response = requests.get(url, params)
  response_dict = json.loads(response.text)

  # 위와 같은 결과
  response_dict = response.json()
  access_token = response_dict['access_token']

  # 액세스 토큰을 debug
  # 결과에서 해당 토큰의 user_id(사용자 고유값)을 가져올 수 있다.
  url = "https://graph.facebook.com/debug_token"
  params = {
    'input_token': access_token,
    'access_token': '{}|{}'.format(
      settings.FACEBOOK_APP_ID,
      settings.FACEBOOK_APP_SECRET_CODE,
    )
  }
  response = requests.get(url, params)
  return HttpResponse(response.text)
```
코드가 점점 길어지고 있다..

위 토큰으로 얻은 user_id는 절대 다른 사람들과 겹치지 않으니까, 이제 우리는 이걸 사용할 수 있다. 그런데 더 나아가 우리가 public data를 넘어서 더 많은 정보를 가져오고 싶다면 `scope`를 사용하면 된다.

html:
```html
<div>
	<form action="" method="POST">
		<input type="text" name="username">
		<input type="password" name="password">
		<button type="submit" class="btn btn-info">로그인</button>
	</form>

	<a href="https://www.facebook.com/v3.0/dialog/oauth?
	client_id=788559651351575
  &redirect_uri=http://localhost:8000/members/facebook-login/
	&scope=email,public_profile" class="btn btn-primary">Facebook 로그인</a>
</div>
```

이제 우리가 받아온 access_token을 통해 페이스북의 기능을 가져와보자.

### Graph API

[Graph API explorer](https://developers.facebook.com/tools/explorer)

우리가 받은 토큰을 가지고 미리 가져올 API를 테스트해 볼 수 있다.
> Graph API Explorer > 토큰 받기 > 다 지우고 액세스 토큰받기 > me?fields=id,name,first_name,last_name,picture


이제 액세스토큰을 통해 가져온 데이터를 통해서 우리가 무엇을 해야하냐면, id값을 가지고 user를 특정하면 되니까 username이 이 id값이면 된다. 그리고 나머지 값으로 회원을 만들어주면 된다.

그때 이 요청을 어디로 보내야하냐면,  

```python
def facebook_login(request):
  # GET parameter의 'code'에 값이 전달됨(authentication code)
  # 전달받은 인증코드를 사용해서 액세스 토큰을 받음
  code = request.GET['code']

  url = 'https://graph.facebook.com/v3.0/oauth/access_token'
  params = {
      'client_id': settings.FACEBOOK_APP_ID,
      'redirect_uri': 'http://localhost:8000/members/facebook-login/',
      'client_secret': settings.FACEBOOK_APP_SECRET_CODE,
      'code': code,
  }
  response = requests.get(url, params)
  response_dict = json.loads(response.text)

  # 위와 같은 결과
  response_dict = response.json()
  access_token = response_dict['access_token']

  # 액세스 토큰을 debug
  # 결과에서 해당 토큰의 user_id(사용자 고유값)을 가져올 수 있다.
  url = "https://graph.facebook.com/debug_token"
  params = {
    'input_token': access_token,
    'access_token': '{}|{}'.format(
      settings.FACEBOOK_APP_ID,
      settings.FACEBOOK_APP_SECRET_CODE,
    )
  }
  response = requests.get(url, params)


  # GraphAPI의 'me'를 이용해서 Facebook User정보 받아오기
  url = 'https://graph.facebook.com/v3.0/me'
  params = {
    'fields' : ','.join([
      'id',
      'name',
      'first_name',
      'last_name',
      'picture',
    ])
    # 'fields': 'id,name,first_name,last_name,picture',
    'access_token': access_token,
  }
  response = requests.get(url, params)
  response_dict = response.json()

  # 받아온 정보 중 회원가입에 필요한 요소들 꺼내기
  facebook_user_id = response_dict['id']
  facebook_name = response_dict['name']
  last_name = response_dict['last_name']
  url_img_profile = response_dict['picture']['data']['url']

  user, user_created = User.objects.get_or_create(username=facebook_user_id)
  # 유저가 새로 생성되었다면
  if user_created:
    user.first_name = first_name
    user.last_name = last_name
    user.save()
  login(request, user)
  return redirect('index')
```
위에서 get_or_create는 ()에 있는 값이 없으면 true, 이미 있으면 false를 나타낸다.

그래서 뒤에 나오는 if는 user가 만들어졌다면이 true를 나타낸다. 그런데 잠깐,

get_or_create를 이용해서 아래 if문이 나오지 않고서도 바로 처리할 수 있도록 해보자.

```python
# facebook_user_id가 username인 User를 기준으로 가져오거나 새로 생성
user, user_created = User.objects.get_or_create(
  username=facebook_user_id,
  default={
      'first_name': first_name,
      'last_name': last_name,
    },
  )
  # 생성한 유저로 로그인
  login(request, user)
  return redirect('index')
```
default의 의미는 무엇일까?

get_or_create을 할때, get을 할때 `username=facebook_user_id`을 기준으로 가져오고, create할때는 `default` 값들을 넣어준다는 의미로 get에 다 넣어주지 않는이유는 사실 username만이 사용자의 고유한 값이고 default값은 사용자의 입력값에 따라 달라질 수 있는 값이다.

사용자가 만약 이메일이나 프로필 이미지를 바꾸면 default값은 바뀌는데, 그 값이 항상 고유한 값이 아니기 때문에 get을 통해 가져오는 것이 아니라 create을 해줘야 한다. 그때 쓰는 것이 default라는 입력값이다.

> 우리는 근데 지금, 정보를 받아와서 바로 로그인이 되도록 했는데 만약 바로 로그인이 아니라 사용자가 정보를 수정한 다음 로그인을 하게 하려면 어떻게 해야할까?

우리가 받아온 데이터
```python
facebook_user_id = response_dict['id']
facebook_name = response_dict['name']
last_name = response_dict['last_name']
url_img_prifile = response_dict['picture']['data']['url']
```

이 데이터들을 이용해 다시 회원가입을 하는 창으로 보낸다. (context에 넣어서) 그러면 그 context안에서 이 사람은 페이스북 로그인이다 라고 하고 이미 있던 필드는 채워주고 없던 필드만 받아서 다시 요청을 받았을때 다른 회원가입이 되어야 하는 것, 즉 템플릿이 두개로 나뉘어져 있어야 한다.

username 회원가입과 페이스북 회원가입 템플릿을 두개로 나누는데, 페이스북 로그인은 위의 과정을 다 거쳐온 뒤 기존에 있는 user가 없는 상태에서 정보를 context를 통해 받아와 템플릿으로 넘겨 미리 렌더링을 보내놓는다.

말이 어려운데, 그냥 간단히 생각해보면 우리가 페이스북 정보를 받아와 로그인을 할때, 회원가입창에는 이미 내가 페이스북에 등록했던 정보들은 채워져 있고 나머지 정보들만 기입해달라는 창을 보듯이 그 템플릿을 만들어야한다는 의미이다.

즉 바로 회원가입 로그인이 되도록 하는게 아니라 만들어놓은 템플릿으로 이동을 시킨 뒤 다시 회원가입을 시켜야 하는 것!
