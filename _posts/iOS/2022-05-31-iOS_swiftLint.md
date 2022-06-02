---
layout: post
title: iOS SwiftLint 적용해보기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## SwiftLint 적용해보기 

이전에 글에서는 SwiftLint를 적용했을 때 발생하는 경고들에 대한 해결방안에 대해서만 정리를 했었습니다. > [iOS SwiftLint 적용하며 수정한 것들
](https://www.zehye.kr/ios/2022/01/06/iOS_swift_lint/)

이번글에서는 좀 더 근간이 되는.. SwiftLint 자체를 프로젝트에 어떻게 적용하는지? 에 대한 방법을 정리해보도록 하겠습니다. 


우선 프로젝트에 vi Podfile 한 후 SwiftLint를 추가해줍니다. > pod install 

```vim
pod 'SwiftLint'
```

그리고 프로젝트로 돌아와 `Target > build phase > + > new run script phase` 절차를 밟아줍니다.<br>
그리고 아래 사진처럼 run script에 해당 코드를 추가해 주세요.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/37.png" alt="" width="80%">
</figure>
</center>

```vim
${PODS_ROOT}/SwiftLint/swiftlint
```

그리고 빌드를 해줍니다...<br>
그러면 에러든.. 경고든... 뭔가가...우수수... 나타나게 될 것입니다..


이 swiftLint에는 정말 다양하고도 많고 많은 규칙들이 나오게 되는데요.<br>
이 모든것들을 제가 다 컨트롤 하지못하고 그저 시키는대로만 해야한다면.. 그건 또 너무 힘들겠죠?

그래서 사실 이 규칙들을 또 제가 컨트롤 할수도 있습니다.

하는 방법은 아래와 같습니다.

새로운 파일을 만듭니다. **Empty 파일**로 만들어주세요.<br>
여기서 중요한 뽀인트는 프로젝트에 파일을 추가해야 하는 것입니다. 프로젝트 > 앱 이름 폴더 안이 아니구요!<br>
프로젝트 자체에 추가!!!!!!!!!!!!!<br>
그리고 이름은 무조건 `.swiftlint.yml` 로 지어주세요. 무 족 권

그러면 경고창으로 `파일 이름 앞에 .을 붙이면 숨김 파일 되는데 너 쓸거야?` 라고 물어봅니다. 응~ 쓸거야(Use ".") 버튼 눌러줍니다.

그리고 이곳에서 각자 예외를 만들어줄 것들을 추가해줍니다.

```vim
# 파일 length
file_length: 1200

# 깊이
nesting:
  type_level: 3

# 한줄 길이
line_length: 300

# 함수 길이
function_body_length: 160

# body 길이
type_body_length: 500

# 함수 복잡성
cyclomatic_complexity: 60

# 변수명
identifier_name:
  min_length: 2

# 튜플 최소갯수
large_tuple: 3

# 타입길이
type_name:
  min_length: 2
  max_length:
    warning: 100
    error: 150

# 규칙제거 - 축약
disabled_rules:
- syntactic_sugar
- trailing_whitespace
- compiler_protocol_init

# 제외 경로
excluded:
  - Pods

```


저는 대략적으로 이렇게 짜놓았습니다. 

여기서 집고 넘어갈 부분은 `disabled_rules, excluded` 정도가 있겠네요.

#### 1. disabled_rules
 
swiftLint에서 디폴트로 적용되는 규칙들을 내 프로젝트에서는 아닌 상태로 만들어줍니다. 


#### 2. excluded

swiftLint가 검사하지 않을 영역을 지정해주는 것입니다. <br>
특정 파일에서 경고가 뜨는데, 별로 보고싶지 않다면 이 특정 폴더 혹은 파일을 제외시킬 수도 있게 됩니다.

저 같은 경우는 코코아팟 라이브러리들에서 나는 에러와 경고는 모두 무시할 수 있도록 지정해놓았습니다.

#### 3. included

추가로 included같은 경우는 유추 가능하듯 반드시 검사가 필요한 영역에 사용해볼 수 있겠습니다.<br>
근데 저는 이 included는 사용하지 않을 것 같네욤..ㅎ..