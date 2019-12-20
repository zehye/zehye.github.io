---
layout: post
title: Django - recursive relationship
category: Django
tags: [django]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## Many To Many

### Recursive Relationship

Recursive Relationship 이란 다른 모델의 인스턴스와 관계를 형성하는 것이 아닌 자기 자신의 인스턴스와 관계를 형성할 때 사용한다.<br>

예시를 위해 페이스북을 한번 떠올려보자.
페이스북의 유저를 관리하기 위해서 우선 `FacebookUser`라는 모델을 사용한다.<br>
회원가입을 마친 유저들의 데이터는 `FacebookUser`라는 모델의 형식에 맞게 형성되어 있을 것이다.

페이스북에서 친구를 추가하는 방식은 간단하다.<br>
내가 상대방에게 `친구추가`를 누르기만 하면 된다. 그리고 신기하게도 내가 친구를 추가했을 뿐인데, 상대방에게도 나라는 사람이 상대방의 친구 목록에 추가가 되어진다.

내가 상대방에게 `친구추가`를 누르는 순간, 나와 내 친구 사이의 관계가 형성이 된다. <br>
그리고 이때 나와 내 친구는 모두 FacebookUser라는 모델의 인스턴스이다. **즉 다른 모델이 아닌 자기 자신의 인스턴스끼리 관계를 형성하는 형태를 보인다.**<br>
이러한 관계를 Recursive relationship이라고 한다.

이러한 재귀적관계를 만드는 방법은 간단하다. 관계를 형성할 모델대신 `self`를 할당해주면 된다.

```python
class FacebookUser(models.Model):
    name = models.CharField(max_length=20)
    friends = models.ManyToManyField('self')

    def __str__(self):
        return self.name
```

### shell_plus에서 구현해보기

정확한 실습을 위해 makemigrations, migrate를 하는 것 잊지말자!

```python
u1 = FacebookUser.objects.create(name='user1')
u2 = FacebookUser.objects.create(name='user2')

u1.friends.add(u2)
u1.friends.all()

>>> <QuerySet [<FacebookUser: u2>
```
