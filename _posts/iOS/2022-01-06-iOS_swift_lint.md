---
layout: post
title: iOS SwiftLint 적용하며 수정한 것들
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## SwiftLint


### Vertical Whitespace Violation: Limit vertical whitespace to a single empty line. Currently 2. (vertical_whitespace) 2946 (999+, 9)

코드 사이 한줄씩만 띄도록 수정

### Colon Violation: Colons should be next to the identifier when specifying a type and next to the key in dictionary literals. (colon)

타입 정의나 딕셔너리에서 `:`을 사용할때 앞은 붙이고 뒤는 한칸 스페이스를 넣어야한다.

```swift
let a: Int?
let b: [String: Int]
```

### Unused Closure Parameter Violation: Unused parameter "timer" in a closure should be replaced with _. (unused_closure_parameter)

사용하지 않는 클로저 파아미터들은 `_` 와일드카드 식별자로 변경

### Opening Brace Spacing Violation: Opening braces should be preceded by a single space and on the same line as the declaration. (opening_brace)  (741, 9)

함수 등에서 앞의 대괄호를 열때 `{`, 앞 쪽 공백이 한칸 들어가야 한다.

```swift
func setA() { }
```

### Control Statement Violation: if, for, guard, switch, while, and catch statements shouldn't unnecessarily wrap their conditionals or arguments in parentheses. (control_statement) (757, 8)

if 구문등의 조건 부분을 소괄호로 묶지 않아도 된다.

```swift
if (a > b) { }  // 변경 전
if a > b { }  // 변경 후
```

### Comma Spacing Violation: There should be no space before and one after any comma. (comma)

콤마 `,`를 사용한 경우, 그 앞은 붙고 그 뒤는 한칸 공백이 있어야 함

### Statement Position Violation: Else and catch should be on the same line, one space after the previous declaration. (statement_position)

else 구문은 이전 괄호와 동일한 줄에 표시하는 것을 권장

```swift
if {

} else {

}
```

### Returning Whitespace Violation: Return arrow and return type should be separated by a single space or on a separate line. (return_arrow_whitespace) (441, 8)

반환타입을 주는 화살표 `->`와 반환 타입 사이 공백이 한칸씩 필요
```swift
func setA() -> Int { }
```


### Redundant Optional Initialization Violation: Initializing an optional variable with nil is redundant. (redundant_optional_initialization)

옵셔널 변수라면 최초에 자동으로 nil로 초기화 되기 때문에 명시해주지 않아도 된다.

```swift
var frame: CGRect? = nil  // 적용 전
var dimFrame: CGRect?  // 적용 후
```
