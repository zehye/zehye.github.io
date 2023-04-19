---
layout: post
title: Django - django-tutorial 3, (views.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-tutorial을 보고 정리했습니다.

<hr>
## views.py

투표하는 view functions을 만들어 본다.

```python
from django.http import HttpResponse, Http404, HttpResponseRedirect
from django.shortcuts import render, get_object_or_404, redirect
from django.template import loader
from django.urls import reverse
from django.views import generic

from .models import Question, Choice


def index(request):
  # latest_question_list는 Question 클래스를 '-pub_date'의 역순으로 정렬한 변수
  latest_question_list = Question.objects.order_by('-pub_date')[:5]
  context = {
    'latest_question_list': latest_question_list,
  }
  # polls 템플릿 폴더 안에 있는 index.html파일을 rendering
  return render(request, 'polls/index.html', context)

def custom_get_object_or_404(model, **kwargs):
  # 1번째 인자로 특정 Model 클래스를 받는다.
  # 최소 1개 이상의 키워드 인자를 받아서, 받은 인자들을 사용해 주어진 Model 클래스의 get()메서드를 실행
  try:
    # 존재하면 해당 인스턴스를 리턴
    return model.objects.get(**kwargs)
  except model.DoesNotExist:
    # 없으면 raise Http404를 실행(404오류 속 메시지는 임의로 알아서 지정하면 됨)
    raise Http404()

# 404에러 일으키기
def detail(request, question_id):
  question = get_object_or_404(Question, id=question_id)
  context = {
    'question': question,
  }
  return render(request, 'polls/detail.html', context)
  # 위 함수를 try-except구문으로 만들면,
  # try:
  #   question = Question.objects.get(id=question_id)
  # except Question.DoesNotExist:
  #   raise Http404()

def results(request, question_id):
  question = Question.objects.get(id=question_id)
  context = {
    'question': question,
  }
  return render(request, 'polls/result.html', context)

def vote(request, question_id):
  # 디버그로
  # print('request.GET:', request.GET)
  # print('request.POST:', request.POST) 확인

  # 선택한 Choice의 choice_text와 id값을 갖는 문자열 생성
  # 해당 문자열을 HttpResponse로 전달
  # 예, choice_text: 민아, id: 4, 현재 Choice 의 votes: ~

  # choice_id가 request.POST의 choice에서 나타나는 것을 print를 통해 알 수 있다.
  choice_id = request.POST['choice']
  question = Question.objects.get(id=question_id)
  choice = Choice.objects.get(id=choice_id)
  question_text = question.question_text

  choice_text = choice.choice_text
  choice.vote += 1
  # save필수!
  choice.save()

  return redirect('polls/result.html', question_id)
```
