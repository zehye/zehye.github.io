---
layout: post
title: 패스트캠퍼스 메모정리(serializer, 배포)
category: etc
tags: [python, computer science, django]
comments: true
---

> 패스트캠퍼스 수업을 들으며 정리두었던 메모를 한 곳에 모아둔 곳입니다.

<hr>

## serializer

웹 API를 만들려면 우선 Snippet 클래스의 인스턴스를 json 같은 형태로 직렬화(serializing)하거나 반직렬화(deserializing)할 수 있어야 합니다.

Django REST 프레임워크에서는 Django 폼과 비슷한 방식으로 시리얼라이저를 작성합니다.


ModelSerializer 클래스가 마법을 부리는 도구는 아닙니다. 그저 시리얼라이저 클래스의 단축 버전일 뿐이죠.

필드를 자동으로 인식한다
create() 메서드와 update() 메서드가 이미 구현되어 있다.

```ipython
In [8]: snippet = Snippet(code='foo = "bar"\n')

In [9]: snippet.save()

In [10]: snippet = Snippet(code='print "hello"\n')

In [11]: snippet.save()

In [12]: serializer = SnippetSerializer(snippet)

In [14]: serializer.data
Out[14]: {'pk': 2, 'title': '', 'code': 'print "hello"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}

In [18]: pprint(serializer.data)
{'code': 'print "hello"\n',
'language': 'python',
'linenos': False,
'pk': 2,
'style': 'friendly',
'title': ''}

In [19]: pprint(snippet)
<Snippet: Snippet object (2)>

In [20]: pprint(dir(snippet))
['DoesNotExist',
'MultipleObjectsReturned',
'__class__',
'__delattr__',
'__dict__',
'__dir__',
'__doc__',

In [21]: content = JSONRenderer().render(serializer.data)

In [22]: content
Out[22]: b'{"pk":2,"title":"","code":"print \\"hello\\"\\n","linenos":false,"language":"python","style":"friendly"}'

In [23]: serializer.data
Out[23]: {'pk': 2, 'title': '', 'code': 'print "hello"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}

In [25]: from django.utils.six import BytesIO

In [26]: stream = BytesIO(content)

In [27]: data = JSONParser().parse(stream)

In [28]: data
Out[28]:
{'pk': 2,
'title': '',
'code': 'print "hello"\n',
'linenos': False,
'language': 'python',
'style': 'friendly'}

In [29]: serializer.data
Out[29]: {'pk': 2, 'title': '', 'code': 'print "hello"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}

In [30]: type(data)
Out[30]: dict

In [31]: type(serializer.data)
Out[31]: rest_framework.utils.serializer_helpers.ReturnDict



In [32]: serializer.data
Out[32]: {'pk': 2, 'title': '', 'code': 'print "hello"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}

In [33]: snippet2 = Snippet(**serializer.data)



In [9]: type(serializer)
Out[9]: snippets.serializers.SnippetSerializer

In [10]: from django.utils.six import BytesIO

In [11]: stream = BytesIO(content)

In [12]: data = JSONParser().parse(stream)

In [13]: serializer = SnippetSerializer(data=data)

In [14]: serializer.is_valid()
Out[14]: True

In [15]: serializer.validated_data
Out[15]:
OrderedDict([('title', ''),
('code', 'print "hello"'),
('linenos', False),
('language', 'python'),
('style', 'friendly')])

In [16]: data
Out[16]:
{'pk': 2,
'title': '',
'code': 'print "hello"\n',
'linenos': False,
'language': 'python',
'style': 'friendly'}

In [17]: serializer.save()
Out[17]: <Snippet: Snippet object (3)>

In [18]: Snippet.objects.create(**serializer.validated_data)
Out[18]: <Snippet: Snippet object (4)>
```

## get_object_or_404

```
Obj = get_object_or_404(Snippet.objects.all(), pk=)

Try:
	Snippet.objects.create(pk)
	return obj
Except DoesNotExist:
	return Http 404
```


## container_commands & files

container_commands

	무조건 배포 이전에 작동, leader_only옵션 존재

	-> leader에만 "특정파일"을 생성

files에 넣은 스크립트

	배포 이후에 작동하도록 할 수 있음, leader_only옵션이 없음

	-> 작동시에 "특정파일"이 있을 경우에만 작동

## Nginx, uWSGI

Web Server (Nginx)
	- HTTP요청 수신 / 응답 송신 (프로토콜: HTTP)
	- 동적인 처리가 필요하면 Django로 넘겨야 하므로 WSGI사용

WSGI (uWSGI)
	- Python application과 WebServer간의 인터페이스 (프로토콜: Unix socket)

Python application (Django)
	- 특정 HTTP요청에 대한 동적응답 생성


## EC2를 사용한 배포

```
Route53 -> Domain, SSL인증서
Load Balancer <- HTTP Request
	Auto-Scaling
		EC2 (Docker -> Docker container)
		EC2 (Docker -> Docker container)		
						RDS
						S3

- ECS
Load Balancer
	Auto-Scaling
		Docker

- EB(ElasticBeanstalk)
Docker
```

## Nginx를 통한 서버관리

```
Browser
-> (HTTP) Docker
-> (HTTP[forward]) -> Nginx
-> (Nginx conf) -> (UNIX Socket) -> uWSGI
-> (uwsgi.ini) -> (WSGI module) -> Django

Nginx로 서버관리
운영 가능한 사이트 -> A, B, C, D, E (site available)
			링 		크
실제 운영할 사이트 -> A, B, C,   E (site-enabled)
```

## DEFAULT_FILE_STORAGE & STATIC_FILES_STORAGE

```
DEFAULT_FILE_STORAGE
	일반적인 파일 작업에 쓰이는 Storage backened
	-> 유저가 업로드한 파일은 S3에 저장

STATIC_FILES_STORAGE(FileSystemStorage상속)
	collectstatic에 쓰이는 Storage backend
	-> 프로젝트에 포함된 정적파일은 해당 서버에서 바로 제공
```

## file storage backend

```
File storage backend
	- Django File
		1 FileField, ImageField를 통해
		2 자신에게 정의된 Storage backend를 이용해서
		3 파일을 기록/삭제/읽음 등등의 작업을 함

	- FileSystemStorage
		하는일 : Django가 file을 다룰 때, 해당 File이 FileSystem의 File일 때의 인터페이스
		-> Django File <--> Python File

	- DropboxStorage (Custom)
		Django가 File을 다룰때, 해당 File이 Dropbox내의 File일 때의 인터페이스

	- S3Storage
		Django ... S3의 File일때


-> 실제로 어떻게 Django에서 파일을 다루면 곧바로 S3에 적용이 되는가?

1 Django에서 CustomBackend를 지정, 이 Backend는 S3와의 통신을 증계한다.
2 S3Backend는 S3가 제공하는 API를 사용해서 Django의 File이 제공하는 메서드들을 자동으로
Request/response처리 -> open(), write(), read()와 같은 메서드들을 지원

Ex) read('파일명')을 실행할 경우
	- Custom backend에서 S3에 연결하기 위한 AWS credentials을 검사
	- S3에 read할 권한이 있다면, boto3를 사용해서 session/client세팅
	- boto3인스턴스를 사용해서 S3가 제공하는 API의 get_file과 같은 기능 실행
	- 실행 결과 데이터를 다시 Django가 사용가능한 File object형태로 파싱
	- Django는 단순히 파일을 불러오 것처럼 편하게 사용

Django에 Django-storage를 깔고(우리가 해야하는 것)
이 안에 (boto3와 AWS)의 연결은 자동으로 이루어 진다.
------

File object: file system 혹은 memory를 사용한다는 전제조건에 있어 이 둘 중 하나에 액세스 할 수 있다.

------

- Python File(FileSystem of Memory상의 File object)
- Django File은 파일에 액세스 할 수 있도록 도와주는 프록시 객체
프록시 : 거쳐간다, 전달해주는, 대리사용 이라는 뜻을 가짐

프록시 객체 : 간접적으로 접속할 수 있도록 해주는 응용프로그램으로 실제 파일에 접근할 수 있도록 도와주는 것
Django는 실제파일이 아님에도 불구하고 이 객체에 접근할 수 있도록 도와주는 것
-> 내가 가지고 있는 backend storage를 통해서 접근할 수 있도록 함
```

## filesystem

```
name = user/gob.png

for path in settings.STATICFILES_DIRS:
	if os.path.exists(os.path.join(path, name)):
		return True
return False
```
