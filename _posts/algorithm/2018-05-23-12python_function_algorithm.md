---
layout: post
title: python 함수 - 선택정렬, 순차검색
tags: [algorithm]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 순차검색, 선택정렬에 대해 설명합니다.
---

<hr>

### 알고리즘

* 순차검색

- 문자열과 키 문자 1개를 받는 함수 구현

- while문을 이용, 문자열에서 키 문자가 존재하는 index위치를 검사 후 해당 index를 리턴

- 찾지 못했을 경우 -1을 리턴

```python
def sequential_search(string, key):
    index = 0
    # 전체문자열을 순회하며 각 글자를 char에 할당
    for char in string:
        print(char)
        # 순회중인 문자(char)와 찾으려는 문자(key)가 같은 경우
        if char == key:
            # 메시지를 출력하고 해당 index를 리턴
            print('찾는 값이 나왔음, index : ', index)
            return index
        # 매 루프의 끝에서 index값을 1씩 증가
        index += 1

    return -1
# 전체 문자열을 순회했음에도 char와 key가 같은 경우가 없다면
# (같다면 return 으로 함수가 종료됨)
# -1 을 리턴하며 함수 종료
```

```python
def sequential_search(string, key):
    index = 0
    # 5글자 문자열의 경우
    # length = 5, max index = 4
    while index < len(string):
        # 주어진 문자열 (string)에서 인덱스 번째의 문자를
        # char변수에 할당
        char = string[index]
        print(f'index : {index}, 현재문자: {char}')
        if char == key:
            print('찾았음')
            return index
        index += 1
    return -1
```

* 선택정렬

- [9, 1, 6, 8, 4, 3, 2, 0, 5, 7] 를 정렬한다.

- 정렬과정

  - 리스트 중 최소값을 검색

  - 그 값을 맨 앞의 값과 교체

  - 나머지 리스트에서 위의 과정을 반복

- 해결방법

  - 알고리즘 진행과정 그려보기

  - 의사코드(Psuedo code) 작성

  - 실제 코드 작성

```python
ori2 = [2,4,-3,-5,3,1,6,0,8,9]

min = ori2[0]
for i in ori2:
    if i < min:
        min = i
    #print(f'현재 숫자 : {i}')
print(f'가장 작은 값 : {min}')
```

```python
ori = [9, 1, 6, 8, 4, 3, 2, 0, 5, 7]

for i in range(10):
    print(ori[i:])

# min함수 쓰지말고 각 loop의 리스트에서 가장 작은 값 출력
for i in range(len(ori) -1):
    #print(ori[i:])
    print(f'loop {i}, ori : {ori}')
    # 현재 loop의 첫 번째 값을 최소값으로 지정
    # (i는 상위 loop에서 1씩 증가)
    min = ori[i]
    min_index = i
    for inner_index, i in enumerate(ori[i:]):
        if i < min:
            min = i
            # 최소 값을 지정하면서 현재 loop의 최소값 index 갱신
            min_index = i + inner_index

        print('최소:', min , '최소 index : ', min_index)
        # 최소값을 찾았으므로 i번째 요소와 위치를 바꿔줌
        ori[i], ori[min_index] = ori[min_index], ori[i]

```

```python
ori = [9, 1, 6, 8, 0, 4, 3, 2, 5, 7]
ori_length = len(ori)

for i in range(ori_length - 1):
    min_index = i
    for inner_index in range(i, ori_length):
        if ori[inner_index] < ori[min_index]:
            min_index = inner_index

        if min_index != i:
            ori[i], ori[min_index] = ori[min_index], ori[i]
print(ori)
```

아직 정리가 더 필요하다.
