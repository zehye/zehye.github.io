---
layout: post
title: Django - django-tutorial 5, (template-polls/index, retail, results)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-tutorial을 보고 정리했습니다.

<hr>

app/polls/tempates/polls/index.html, detail.html, results.html
config/setting.py에서


## index.html

```html
{raw}
{% if latest_question_list %}
    <ul>
        {% for question in latest_question_list %}
        <li>
            {% comment %}
            기존에는 위치인자로 설정을 해서 했는데, 이제는 이름을 지어줄 수 있으니까
            question_id = question.id 로 가능하다.
            여러개의 인자를 받을때 이런식으로 해결이 가능하다.
            {% endcomment %}

            <a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a>
        </li>
        {% endfor %}
    </ul>
{% else %}
    <p>No polls are available</p>
{% endif %}
{endraw}
```

## detail.html

```html
{% raw %}
<h1>{{ question.question_text }}</h1>

{% if error_message %}
<p><strong>{{ error_message }}</strong></p>
{% endif %}
<form action="{% url 'polls:vote' question_id=question.id %}" method="post">
    <!--post방식 요청이므로 csrf_token추가-->
    {% csrf_token %}

    <!--여기가 Question detail페이지이며, qeustion키에 해당하는 Question인스턴스가 할당되어 있음-->
    <!--현재 Question에 속하는 Choice목록은 question.choice_set.all로 QuerySet을 얻어낼 수 있으며,-->
    <!--for 구문으로 QuerySet을 순회한다.-->

    {% for choice in question.choice_set.all %}
    <!--view에서 context로 question이라는 애들 question 인스턴스로 전해주니까 for문을 돌 수 있다.-->

    <!--각 Loop(매 Loop의 아이템: Choice인스턴스)마다 radio input과 lable을 만들어줌-->
    <!--forloop.counter: 몇번째 loop인지 숫자로 세어준다 .-->

    <!--input 의 id와 label의 for는 서로 각자를 연결해준다.-->
    <!--input의 고유한 id값을 통해 label을 연결시켜 준다. (강의 다시)-->

    <!--루프를 돌때마다 하나의 초이스가 나오고 초이스라는 키 값에 벨류는 선택한 초이스 값이 나오게 됨-->
    <div>forloop.counter : {{ forloop.counter }}</div>
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
    <!--value는 보내는 값, -->
    <label for="choice{{ forloop.counter }}">choice.choice_text: {{ choice.choice_text }} | choice.id : {{ choice.id }}</label><br>
    {% endfor %}
{% endraw %}
    <!--전송버튼-->
    <button type="submit">Vote</button>

</form>

```

## results.html

```html
{% raw %}
<h1>{{ question.question_text }}</h1>
<ul>
    {% for choice in question.choice_set.all %}

        <li>{{ choice.choice_text }} ( votes: {{ choice.votes }} )</li>

    {% endfor %}
{% endraw %}
</ul>

```
