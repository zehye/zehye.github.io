---
layout: post
title: Java - 데이터은닉 (private, getter, setter)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 데이터 은닉

**외부로부터 데이터 변질을 보호하자**

프로그래밍을 하면서 데이터 은닉은 자주 나오는데, 객체가 가지고 있는 데이터를 외부로부터 변질되지 않게 보호하는 방법이다.

### 멤버변수의 private 설정

- 멤버변수(속성)은 주로 private 설정을 통해 외부로부터 데이터가 변질되는 것을 막음
  - 클래스 안에 있는 속성의 데이터가 외부로부터 변질되지 않도록 private 으로 설정


### getter와  setter

멤버변수를 private으로 설정을 하면 외부로부터 데이터 변경/접근이 불가능한데 (한번 초기설정이 되면 외부에서 접근 불가능) 이때 외부에서 메서드를 통해 접근하 수 있도록 설정할 수 있다.

### StydentClass

```java
public class Student {

	private String name;
	private int score;

	public Student(String n, int s) {
		this.name = n;
		this.score = s;
	}

	public void getInfo() {
		System.out.println(" -- getInfo() -- ");
		System.out.println(name);
		System.out.println(score);
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getScore() {
		return score;
	}

	public void setScore(int score) {

		this.score = score;
	}
}
```

#### MainClass

```java
public class MainClass {

	public static void main(String[] args) {

		Student student1 =new Student("박지혜", 90);
		student1.getInfo();

		student1.setScore(100);
		student1.getInfo();
	}
}
```

- setter: 전달받은 값을 내부에서 가공해 필드에 넣어준다.
- getter: 필드의 값을 숨긴채 내부에서 가공된 값을 꺼낼 수 있다. (속성의 데이터값을 리턴받는 메서드)


<hr>

#### 어짜피 getter, setter를 통해 public으로 값을 받을 건데 처음부터 메서드를 만들어서까지 한번 더 받는 이유가 뭘까?

- 안전장치를 만드는 것!

```java
public void setScore(int score) {

  if(score >50) this.score = score;
}
```

해당 조건문에만 데이터값이 변경이 될 수 있음

이러한 데이터은닉은 반드시 해야하는 것은 아니며 모든 속성에 적용되는 것도 아니고 그떄의 환경에 맞게 사용하면 된다.

<hr>
