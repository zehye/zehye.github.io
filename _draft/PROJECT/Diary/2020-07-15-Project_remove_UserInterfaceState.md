---
layout: post
title: 다이어리 앱 만들기 - UserInterfaceState.xcuserstate 제거하기
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

프로젝트 중에 gst를 살펴보면 뭘 하지 않았음에도 불구하고 계속해서 파일의 상태를 저장하는 이상한 파일 하나를 볼 수 있을 것이다.<br>
이것때문에 계속해서 불필요한 커밋을 남겨야할 필요는 없으니 해당파일을 깃에서 삭제해주도록 하자.


```
git rm --cached [Project Name].xcworkspace/xcuserdata/[User Name].xcuserdatad/UserInterfaceState.xcuserstate
```

- Project Name: 본인의 해당 프로젝트 이름을 적어준다
- User Name: 본인 이름

어려워 보이겟지만 그냥 UserInterfaceState.xcuserstate 파일의 경로를 적어주는 것 뿐이다.<br>
탭 눌러가며 적어주면 쉽겠쥬?

그러고나서

```
git commit -m 'Removed file that shouldnt be tracked'
```

커밋 하나 날려주면 git status에서 더이상 해당 파일이 추적되지 않음을 볼 수 있을 것이다.


**+**

그런데도 만약 계속 gst에서 추적이 된다면 그냥 gitignore에 추가해주도록 한다.<br>
gitignore에 아래 추가

```
*.xcuserstate
```
