---
layout: post
title: 패스트캠퍼스 메모정리(jekyll, git clone, error log, django)
category: etc
tags: [python, computer science, django]
comments: true
---

> 패스트캠퍼스 수업을 들으며 정리두었던 메모를 한 곳에 모아둔 곳입니다.

<hr>

## 프로그램

메모리를 올린다는 것은 데이터를 올린다는 것
-> 제대로 된 데이터를 올리기 위해 자료형을 알아야 한다.

데이터의 종류에 따라 type이 달라지고 이 데이터에 따라 메모리가 달라지는데,
메모리를 최대한 알뜰하게 사용하기 위해, 데이터의 타입에 따라 차지하는 메모리의 양이 다르다.

## 지킬 사진 올리는 코드

```
<center>
<figure>
<img src="/~/~/screenshot.png" alt="" width="100%">
<figcaption>~</figcaption>
</figure>
</center>
```

## git clone

```
git remote remove origin
-> git remote add origin <git clone 주소>

git fetch origin master
git merge origin/master
```

## docker에서 error log 확인하는 경로

```
Cat /var/log/uwsgi/error.log
```

## 세션으로 장바구니 유지하는 방법
> 참고사이트 [MUVA](http://muva.co.ke/blog/developing-shopping-cart-class-shop-products-django-2-0-python-3-6/)

## 장고를 통한 페이스북 로그인

1 페이스북 로그인 버튼 클릭
2 페이스북 페이지로 이동, 사용자가 로그인
3 application으로 redirect되며, 'code'값을 GET parameter로 받음
4 code를 access_token과 교환

4-1 클라이언트가 access_token을 사용해서 사용자 정보를 받아온 추가 정보를 전부 서버로 전송
4-2 클라이언트가 access_token만 서버로 전송

--------클라이언트---------

5-1 받아온 정보들로 application에 회원가입
5-2 access_token으로 유저의 페이스북 정보를 받아옴
이후 페이스북에서 받은 유저정보로 application에 회원가입

-> 서버에서는 Token을 돌려줌(DRF로그인 유지 토큰)

## 로그인 유지시키기

1 서버에서 받아온 Token이 쿠키의 'token'키값에 저장되어 있음
2 클라이언트가 처음 로드 될때 마다, 해당 키를 사용해 HTTP Header의 인증을 만든 상태로 유저 정보를 받아오는 API 실행
3-1 API호출에 성공하면 (token이 유효하면) 전송받은 User정보를 사용해 화면을 랜더링(@@로 로그인중)
3-2 API호출에 실패하면 (token이 유효하지 않으면) 로그인 버튼을 보여줌


## django migration

Python manage.py showmigration
-> migration을 했던 저장 테이블을 따로 두고 다 저장하고 있음을 볼 수 있다.

Db sqlite에서 django_migrations에 들어가서 보면 나와있다.(해당 테이블)
이 상태에서 python manage.py migrate <특정 어플리케이션 이름>
-> 해당 어플리케이션의 적용되지 않은 migration만 migrate한다.

Python manage.py migrate proxy 0001 을 하면 0001까지만 migrate만 되고 그 이후는 unmigration된다.

** document - making query

## django form

Class SighupForm(forms.Form):
	field1
	field2
	field3

Form = SignupForm(request.POST) <- bound form
Request.POST를 하면 딕셔너리로 field에는 해당하는 key가 들어올텐데

form.is_valid()

field의 메서드 (3번 실행됨) -> 필드에 대한 정제
	1 to_python()
	2 validate()
	3 run_validate()
		-> (1,2,3통합)Field.clean()
		-> 3개의 메서드를 정상적으로 통과하면 form에 있는 cleaded_date에
		해당 Feild key, value를 추가
		ex) form.cleaned_date['username'] = <입력한 값>

form의 메서드 -> form 자체에 대한 정제
	1 clean_<field_name>이 해당 메서드가 존재하는 수 만큼
		-> Field의 유형이 아닌, 특정 값에 대한 유효성 검증
		ex) 입력받은 username이 중복되는 User가 있으면 validationerror
		-> 검증에 성공하면 cleaned_data[field_name]의 값을 리턴해줌

	2 clean()
		-> 여러개의 Field값에 대해 유효성 검증이 필요할 때
		ex) password1, password2가 같은지
		-> 검증에 성공하면 cleaned_data dict가 될 객체를 반환

cleaded_data는 is_valid를 통과한 애들만 보여준다. (유효한 데이터)

## clean 함수

clean()은 장고가 제공하는 유효성 검사기로 잘못된 입력에 대해 `validation error`를 일으킨다.
