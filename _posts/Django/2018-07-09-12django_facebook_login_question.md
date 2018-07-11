---
layout: post
title: Django - facebook login - questions
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### token과 user_id는 어떻게 가져왔을까?

토큰이라는 것은 결국, Authorization Server 즉, 우리한테는 페이스북에게서 Client Server(우리 사이트)로 user의 정보를 가져오고 싶은것인데 그런데 그 경우에 우리가 토큰을 받는다는 것은 Authorization code를 access_token으로 바꾸는데, 이 access_token에 인과된 권한에는 어떤것이 있냐면 기본값으로는 그 사람의 public_profile에만 접근을 못한다.

그런데 우리한테는 어찌됐든 그 토근이라도 있어야 사용자의 정보를 가져올 수 있는데 그 토큰을 가져오는 과정은 사용자가 로그인 버튼을 누를떄 바로 이루어지는 것처럼 보이는데, 그거와 같이 우리가 페이스북 사이트내에서 바로 요청을 걸기 때문에 바로 API 테스트가 가능한 토큰 권한을 웹상에서 준 것이다.

그렇다면 user_id는 debug_token을 하면 그 속에 user의 ID가 담겨있기 때문에 우리가 받아볼 수 있던것이다.

### content와 text의 type의 차이

content는 기본적으로 type이 bytes이고 text는 str인데 bytes와 str의 차이는 단순히 인코딩이 적용될 수 있는 문자열이냐(str), 아스키 코드로만 된 형태의 데이터이냐(bytes)의 차이이다.

### email을 scope로 가져오는 이유

우리가 기본적으로 사용자에 대한 정보 요청을 추가하지 않으면 가져올 수 있는 건 public_profile뿐이다. 그렇기 때문에 email을 추가하는 것이고 email이 public_profile에 속해있지 않은 이유는 모든 user가 email을 가지고 있는 건 아니니까 라는 추측을..

그러니까 email을 기본값으로 넣으면 큰 의미가 없을거라는 ..!

그리고 보통 이런 API에서 사용자 정보를 줄때에는 최소의 정보만을 제공하고 필요할때마다 요청을 하게끔 하는데, 그리고 어느정도 권한 이상이 되어 user에게 민감한 정보를 가져온다 싶을때에는 app을 미리 승인 받도록 한다.

내가 이러한 앱을 만들것이고 그렇기때문에 사용자의 이런 정보들이 필요하다는 인증을 하도록 한다.

실제로 페이스북은 되게 까다로운데, 실제 동작하는 모습에 대한 스크린샷, 영상으로 어떻게 나오는지를 다 보여줘야하고, 이를 통해 페이스북 로그인 버튼이 페이스북과 헷갈리지 않고, 사용자를 낚시하는 사이트가 아니라는 등등을 인증하여 모든 정보들이 다 부합해야 그 정보들을 승인받아 사용할 수 있게 된다.

### debug_token의 의미?
```python
url = "https://graph.facebook.com/debug_token"
params = {
  'input_token': access_token,
  'access_token': '{}|{}'.format(
    settings.FACEBOOK_APP_ID,
    settings.FACEBOOK_APP_SECRET_CODE,
  )
}
response = requests.get(url, params)
```
이 경우에서 우리는 서버에서 직접 access_token을 바로 가져오고 사용하는데, 이럴떄는 사실 access_token이 필요가 없다.

그래서 우리는 언제 이걸 사용하냐면, 이 access_token을 받아오는 과정은 front나 client가 할 수 있는데, 그쪽에서 aaccess_token을 만들고, 가지고 있다가 장기보관토큰이면 우리에게 토큰만 넘기면 되는데

그렇기 때문에 우리는 사용자인증을 안거치고 페이스북 로그인하는 과정을 앱이나 프론트엔드 사이트에서 하는데 그러면 우리에게 access_token이 넘어올때 이 토큰이 올바른지 아닌지를 알 수가 없다. - 유효기간, 유저가 맞는지

그때 debug_token을 사용해, 이 토큰이 유효한지, 토큰에 해당하는 user_id가 무엇인지 확인할 수 있다.
