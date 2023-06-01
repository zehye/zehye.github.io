---
layout: post
title: Data Structure, Hash Table
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Hash Table

a large size of tables consisting of keys and values

- hash function > 특정 키를 입력하면 value 리턴
- key: a unique identifier of a value, a parameter to find its array index through hash functions
- value: a stored vlaue

### 특성

1. key는 무조건 index와 연결
2. 하나의 index는 여러개의 key와 연결 가능


#### Perfect hash function

<center>
<figure>
<img src="/assets/post-img/DataStructure/64.png" alt="" width="50%">
</figure>
</center>

- 가상의 hash function
- 가정상황: unlimited space & 한정된 input size
- a bijective(일대일 대응함수) hash function을 만들 수 있음

**Good hash function**

<center>
<figure>
<img src="/assets/post-img/DataStructure/65.png" alt="" width="50%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/DataStructure/66.png" alt="" width="50%">
</figure>
</center>

- uniform distribution (연속균등분포): 분포가 특정 범위내에서 균등하게 나타있는 경우를 의미
- 왜 좋은가 > 충돌이 적어지기 때문
- 왜 어려운가 > 랜덤하게 배정하는게 쉽지 않음
- Surjective Function: 공역의 모든 원소가 선택되었단는 말 > Y에 있는 원소에 대응되는 x가 존재
- Injective Function: 정의역의 원소하나는 단 하나이 공역의 원소에 대응된다는 것


### Example of Hash Function

#### Modulo based hash function

- divide a numeric key  with a number
- number = the size of the hash table
- Example: Index = Key MOD Size
- 자주 사용하는 이유
  - 장점: 다양한 범위가 나옴
  - 문제

<center>
<figure>
<img src="/assets/post-img/DataStructure/64.png" alt="" width="50%">
</figure>
</center>

주민번호를 사용하는 경우 나누는 숫자가 작은때 앞에 중요한 정보를 사용하지 않고 일부만 사용하는 문제가 발생함 > **Quotient is not used**

#### Square base hash function

- mid-square hash function
    1. square the key value
    2. take the N digit in the middel
    3. the selected digits to the modulo operation
- example
    - KEY =  123, Table size=100, Mid-square digit=3
    - f(123) = MidSquare(123) mod 100 = 512  mod 100 = 12
- 왜 좋은가 > 충돌횟수를 줄일수 있음

## Digit analysis based hash function

- Digit analysis
    - ex) checksum hash function
- key = 8011171234567, Table size =100
- f(8011171234567) = (8+0+1..+7) mod 100 = 46
- 주어진 모든 숫자를 이용하는 점에서 좋음
