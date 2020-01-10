---
layout: post
title: Data Structure, Abstract data types
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Data Structure

linked list, stack, and queue... < 선형 자료구조


### Array for List

다른 언어에서는 Array이라고 하지만 파이썬에선 List라고 부른다.

뒤엉켜있는 무언가를 선형구조로 줄을 세워놓는 것<br>
선형적인 공간을 따라가기만 해도 원하는 결과를 얻을 수 있다.

구조화된 형태로 저장해야하고, 그 가장 간단한 방법이 list이다.


### Abstract data type(ADT, 자료구조의 추상화)

- 추상화: 현실에 있던 것을 간단화하여 표현한 것.
- 자료구조: 데이터에 대해 store, search, insert, delete 등의 일을 하는 것
- 자료구조의 추상화: 자료구조의 다양한 행위를 추상적으로 어떻게 돌아가는지 표현하는 것
즉, 구조 속에 데이터가 어떻게 저장되어있는지는 알지는 못하지만 속에 어떤 데이터가 있는지와 어떤 operator을 통해서 이 데이터가 영향을 받는지를 명시함으로써 데이터와 operation만은 정의를 하겠다는 것으로 어떤 데이터가 저장되고 실행되는지를 설명하는 것을 의미한다.
