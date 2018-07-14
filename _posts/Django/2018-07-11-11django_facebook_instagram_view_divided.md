---
layout: post
title: Django - Facebook Backends.py
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Facebook Backends

1.settings.py에 AUTHENTICATION_BACKENDS항목을 작성
- Django가 기본적으로 지원하는 Default값(ModelBackend)를 포함한
- 리스트를 작성(FacebookBackend로의 문자열)

2.facebook_login view를 수정
- user를 받아오는 과정에
- user = authenticate(request, code=code)를 사용

```python
import json

import requests
from django.conf import settings
from django.contrib.auth import get_user_model

User = get_user_model()


class FacebookBackends:
  def authenticate(self, request, code):
    """
    Facebook의 Authorization Code가 주어졌을 때 적절히 처리해서
    facebook의 user_id에 해당하는 User가 있으면 해당 User를 리턴
    없으면 생성해서 리턴
    :param request: View의 HttpResponse object
    :param code: Facebook Authorization code
    :return: User인스턴스
    """
    def get_access_token(code):
      """
      Authorization code를 사용해 액세스 토큰을 받아옴
      :param code: 유저의 페이스북 인증 후 전달되는 Authorization code
      :return: 액세스 토큰 문자열
      """
      # GET parameter의 'code'에 값이 전달됨(authentication code)
      # 전달받은 인증코드를 사용해서 액세스 토큰을 받음
      url = 'https://graph.facebook.com/v3.0/oauth/access_token'
      params = {
          'client_id': settings.FACEBOOK_APP_ID,
          'redirect_uri': 'http://localhost:8000/members/facebook-login/',
          'client_secret': settings.FACEBOOK_APP_SECRET_CODE,
          'code': code,
      }
      response = requests.get(url, params)
      # 파이썬에 내장된 json모듈을 사용해서, JSON형식의 텍스트를 파이선 Object로 변환
      # response_dict = json.loads(response.text)
      response_dict = response.json()
      access_token = response_dict['access_token']

      return access_token

    def debug_token(token):
      """
      주어진 token을 Facebook의 debug_token API Endpoint를 사용해 검사
      :param token: 액세스 토큰
      :return: JSON응답을 파싱한 파이썬 Object
      """  
      # 액세스 토큰을 debug
      # 결과에서 해당 토큰의 user_id(사용자 고유값)을 가져올 수 있다.
      url = "https://graph.facebook.com/debug_token"
      params = {
        'input_token': token,
        'access_token': '{}|{}'.format(
          settings.FACEBOOK_APP_ID,
          settings.FACEBOOK_APP_SECRET_CODE,
        )
      }
      response = requests.get(url, params)
      return response.json()

    def get_user_info(token, fields=None):
    # 동적으로 params의 'fields'값을 채울수 있도록 매개변수 및 함수 내 동작 변경
    # get_user_info('<token_value>') <- 기존의 5가지 값을 fields로
    # get_user_info('<token_value', ['id','name','first_name']) <- 주어진 3개만
    """
    주어진 token에 해당하는 Facebook User의 정보를 리턴
    'id,name,first_name,last_name,picture'
    :param token: Facebook User토큰
    :param fields: join()을 사용해 문자열을 만들 sequence 객체
    :return: JSON응답을 파싱한 파이썬 Object
    """
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
        'access_token': token,
      }
      response = requests.get(url, params)
      return response.json()

    def create_user_from_facebook_user_info(user_info):
      """
      Facebook GraphAPI의 'User'에 해당하는 응답인 user_info로부터
      id, first_name, last_name, picture항목을 사용해서
      Django의 User를 가져오거나 없는 경우 새로 만듬(get_or_create)
      :param user_info: Facebook GraphAPI - User의 응답
      :return: get_or_create의 결과 tuple (User instance, Bool(created))
      """
      # 받아온 정보 중 회원가입에 필요한 요소들 꺼내기
      facebook_user_id = user_info['id']
      facebook_name = user_info['name']
      last_name = user_info['last_name']
      url_img_profile = user_info['picture']['data']['url']
      # facebook_user_id가 username인 User를 기준으로 가져오거나 새로 생성
      return user, user_created = User.objects.get_or_create(
          username=facebook_user_id,
          default={
            'first_name': first_name,
            'last_name': last_name,
          }
        )

    # 여기에서의 code만 달라지는데 이때의 code는 함수 authenticate(code)가 가져온다.
    access_token = get_access_token(code)
    user_info = get_user_info(access_token)
    user, user_created = create_user_from_facebook_user_info(user_info)

    # 위의 3줄에 문제만 없으면 무조건 user를 리턴해준다.
    return user

  def get_user(self, user_id):
    """
    user_id(primary_key값)이 주어졌을 때 해당 User가 존재하면 반환하고 없으면 None을 반환
    :param user_id: User모델의 primary_key
    :return: primary_key에 해당하는 User가 존재하면 User인스턴스, 아니면 None
    """
    try:
      return User.objects.get(pk=user_id)
    except User.DoesNotExist:
      return None
```

### settings.py

```python
# Auth
AUTHENTICATION_BACKENDS = [
  # 기본값
  'django.contrib.auth.backends.ModelBackend',
  # 우리가 만든 값
  'members.backends.FacebookBackend',
]
```

### facebook_login.py

```python
from django.contrib.auth import login, get_user_model, authenticate
from django.shortcuts import redirect

User = get_user_model()

__all__ = (
  'facebook_login',
)

def facebook_login(request):
  code = requests.GET.get('code')
  user = authenticate(request, code=code)

  if user in not None:
    login(request, user)
    return redirect('index')
  return redirect('members:login')
```
