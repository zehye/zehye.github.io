---
layout: post
title: string으로 넘어오는 date값 원하는대로 dateFormat 해보기 > iso8601DateFormatter사용하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 문제상황

- 서버에서 보내주는 date값의 형식이 우선 스트링값으로 넘어오고 있다. > **"2020-11-09T11:05:56.000Z"**
- 내가 원하는 것은 2020-11-09 딱 요부분! 그리고 dateFormat은 **yyyy. MM. dd** 이다.

Date extension을 통해 해결해보려 했으나 능력부족인지 뭔지 해결이 되질 않았다..<br>
좋은 방법이 있다면 알려주세요!

### 1. 일단 되게 하는 방법

```swift
let date: String = "2020-11-09T11:05:56.000Z"
let arr = date.components(separatedBy: "T") // T를 기준으로 나눠줌
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy. MM. dd"
let newDate = dateFormatter.date(from: arr[0])  // 첫번째 배열값을 가져와 원하는 포맷으로 변경

self.dateLbl.text = dateFormatter.string(from: newDate!)
```

이 방법은 누가봐도 그냥 정말 딱 되게만 가능하게하는 방법이다.


### 2. 정석: iso8601DateFormatter 사용

```swift
let dateString: String = data.createDate
let iso8601DateFormatter = ISO8601DateFormatter()
iso8601DateFormatter.formatOptions = [.withInternetDateTime, .withFractionalSeconds] // option형태의 포맷
let date = iso8601DateFormatter.date(from: dateString)

let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy. MM. dd"
self.dateLbl.text = dateFormatter.string(for: date!)
```

ios에서 시간 데이터를 관리하기 위해서는 Date라는 타입을 이용해 string을 date로 혹은 반대로 변환하기 위해 DateFormatter를 사용하는 것이 관습처럼 이루어졌는데, ios10부터는 **iso8601DateFormatter 클래스** 가 추가되어 iso8601 표준 포맷을 더 쉽게 다룰 수 있게 되었다고 한다.

특이한 점은 format형태가 문자열이 아닌 **option** 형태라는 것이다.


DateFormatter가 편한 부분도 있어서 계속해서 이를 사용해도 좋지만, 문자열로 format을 명시할 때 발생하는 실수를 줄일때는 위의 방법을 사용하는 것도 좋을 것 같다. 
