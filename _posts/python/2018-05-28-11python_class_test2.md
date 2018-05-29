---
layout: post
title: python class 연습문제 풀이2
category: python
tags: [python, class]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 클래스 실습문제를 다시 정리해봅니다.

<hr>
```
도서 관리 프로그램
    Library, Book, User클래스 구현
        프로그램 시작시 도서 5권 정도를 가진 상태로 시작

    Library
        attrs
            name: 도서관명
            book_list: 도서 목록 (Book인스턴스의 목록)
        methods
            add_book
            remove_book
        property
            info: 가지고 있는 도서 목록을 보기좋은 텍스트로 출력 (빌려간 도서는 출력 안해도 됨)

    Book
        attrs
            title: 제목
            location: 현재 자신이 어떤 Library 또는 User에게 있는지를 출력
        property
            is_borrowed: 대출되었는지 (location이 User인지 Library인지 확인)

    User
        attrs
            name: 이름
            book_list: 가지고 있는 도서 목록
        methods
            borrow_book(library, book_name): library로부터 book을 가져옴
            return_book(library, book_name): library에 book을 다시 건네줌
```

```python
class Library:
    def __init__(self, name, book_list):
        self.name = name
        self.book_list = []

    def add_book(self, book):
        if book.title in self.book_list:
            print('현재 {}책은 {}에 있습니다.'.format(book.title,self.name))
        else:
            (self.book_list).append(title)

    def remove_book(self, book):
        if book.title not in self.book_list:
            print('현재 {}은 {}에 없습니다'.format(book.title,self.name))
        else:
            (self.book_list).remove(title)

    @property
    def info(self):
        if len(self.book_list)>0:
            print('현재 {}에 보유하고 있는 책은 {}권입니다.'.format(self.name, len(self.book_list)))
        else:
            print('현재 {}가 보유하고 있는 책은 없습니다'.format(self.name))


class Book:
    def __init__(self, title, location):
        self.title = title
        self.location = location

    @property
    def is_borrowed(self):
        if self.location == 'Library':
            print('현재 {}는 도서관에 있습니다.'.format(title))

        else:
            print('현재 {}는 다른 사람이 대출중입니다.'.format(title))


class User:
    def __init__(name, book_list):
        self.name = name
        self.book_list = []

    def borrow_book(self, library, book):
        if self.title in self.location:
            (self.book_list).append(self.title)
            print('{}에 {}책이 있어 빌려왔습니다'.format(self.location, self.title))

        else:
            print('지금 {}에 내가 읽고 싶은 {}책은 없다'.format(self.location, self.title))

    def return_book(self, library, book):
        print('{}책을 다 읽어서 반납했다'.format(self.title))

    @property
    def mine_book(self):
        if self.title in self.book_list:
            print('{}책을 {}에 반납하였습니다.'.format(self.title, self.location))
        else:
            print('아직 내게는 {}책이 있습니다.'.format(self.title))
```


```Python
class Library:
  def __init__(self, name, book_list):
    self.name = name
    self.book_list = book_list

  def add_book(self):


  def remove_book(self):
    pass

  @property
  def info(self):
    return ''


class Book:
  def __init__(self, title, location):
    self.title = title
    self.location = location

  @property
  def is_borrowed(self):
    return ''


class User:
  def __init__(self, name, book_list):
    self.name = name
    self.book_list = book_list

  def borrow_book(library, book_name):
    pass

  def return_book(library, book_name):
    pass
