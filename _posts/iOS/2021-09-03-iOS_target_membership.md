---
layout: post
title: iOS Target에 대한 정의, 분리하는 방법, 분리하여 사용했을 때 마주했던 이슈(Target Membership)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## Target

기본적으로 회사에서는 여러 타겟을 나눠서 사용을 하고 있을것이고 이렇게 만들어진 타겟은 빌드할 때 각자 빌드하는 타겟이 설정됩니다.

타겟에 대한 문서 설명 먼저 봐보자!


`A target specifies a product to build and contains the instructions for building the product from a set of files in a project or workspace. A target defines a single product; it organizes the inputs into the build system—the source files and instructions for processing those source files—required to build that product. Projects can contain one or more targets, each of which produces one product.`

이 문서를 직역해보자면, 타겟은 Xcode 프로젝트를 빌드할 때 빌드 과정에서 어떤 리소스, 소스파일들을 포함할 지 설정할 수 있고 빌드 과정의 순서 등 프로젝트의 큰 설정을 지정할 수 있는 것을 의미하는 것 같다. 그리고 이때 중요한 점은 하나의 타겟이 하나의 프로덕트인 것으로 즉, 타겟에 따라 하나의 프로젝트에서 여러 버전으로 프로덕트를 분리할 수 있는 것이다. 그렇기 때문에 기본적으로 회사에서는 Qa, Dev, DevTest 이런 식으로 각각의 타겟이 분리되어 있을 것이다.

즉 간단하게 타겟의 특징을 설명하자면,

1. 하나의 타겟은 하나의 프로덕트이다.
2. 하나의 프로젝트는 여러개의 타겟(프로덕트)로 이루어질 수 있다.
3. 타겟별로 빌드 설정을 다르게 할 수 있다.





### Target 분리하기

타겟 분리하는 실습을 해보기 위해 `TargetProject`라는 새 프로젝트를 생성해보았다.

타겟을 분리하는 방법은 아래와 같다.

#### 1. Target 우클릭 > Duplicate 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target2.png" alt="" width="50%">
</figure>
</center>

그러면 아래 이미지와 같이 복사본 타겟이 하나 생성됩니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target3.png" alt="" width="50%">
</figure>
</center>

#### 2. 복사된 버전을 원하는 이름으로 변경

이후, 이 복사된 버전을 원하는 이름으로 설정해줍니다.<br>
저는 아래와 같이 `TargetProjectDev`로 이름을 변경해 주었습니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target4.png" alt="" width="50%">
</figure>
</center>


#### 3. 빌드 환경에서 복사된 target 있는지 확인

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target5.png" alt="" width="50%">
</figure>
</center>


#### 4. Info.plist 설정

새롭게 Target을 생성했을때 Info.plist 역시 복사가 되어 자동으로 생성됩니다. <br>
그러나 이름은 아래와 같이 `TargetProject copy-info.plist`로 되어있죠. 원하는 이름으로 수정해봅니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target6.png" alt="" width="50%">
</figure>
</center>

그리고 수정한 이름을 Xcode 프로젝트에서도 인식을 해줘야 하기에 **Target > Build settings > Packaging** 에서 변경한 이름으로 똑같이 변경해줍니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target7.png" alt="" width="50%">
</figure>
</center>


이렇게 완료한다면 이제 각각 타겟에 대한 설정은 어느정도 완료되었습니다. <br>
이후부터는 이제 각자의 프로젝트 개발 상황에 맞게 설정을 해주면 됩니다.

이렇게 나뉜 타겟이 제대로 설정되었는지 확인해 봅시다.


#### 5. Target 이름으로 구분해 빌드시 간단한 로그 확인해보기

만들어준 타겟의 **Build settings > Swift Compiler - Custom Flags** 에서 전처리문에서 구분하고 싶은 이름으로 변경해줍니다.<br>
주의할 점은 꼭 `-D`라는 문장을 전에 입력해 주어야 인식이 가능하다는 점입니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target8.png" alt="" width="50%">
</figure>
</center>

이제 프로젝트로 돌아가 확인하고 싶은 뷰컨트롤러에서 다음과 같은 코드를 입력해주세요.

```swift
#if DEV
print("I'm Dev Target")
#else
print("I'm not Dev Target")
#endif
```

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target9.png" alt="" width="50%">
</figure>
</center>

그러면 상황에 따라 이렇게 로그가 찍히는 것을 볼 수 있을 것 입니다!



<br/>

### Target을 분리하여 사용했을 때 마주했던 이슈

타겟을 분리해서 사용하던 중 A라는 뷰 컨트롤러를 생성해 코드를 진행하였고 이 A 뷰 컨트롤러를 B 뷰 컨트롤러에서 불러오려고 하였습니다. 간단한 예시 코드입니다.

#### A viewController

```swift
final class PracticeViewController: UIViewController {
    static func instance() -> PracticeViewController? {
        return PracticeViewController()
    }
}
```


#### B viewController

```swift
func pushPraticeVC() {
  guard let viewController = PracticeViewController.instance() else { return }
  self.navigationController?.pushViewController(viewController, animated: true)
}
```

그런데 이때 이와같은 에러가 발생하였습니다.


```
Cannot find 'PracticeViewController' in scope
```

아무리 찾아봐도 이유를 찾을 수 없었습니다. <br>
그러는 와중에 해당 아티클을 보았습니다.


[Cannot find type in scope after re-generating Core Data models](https://www.reddit.com/r/swift/comments/kvug0s/cannot_find_type_in_scope_after_regenerating_core/)

문제는 이와 같습니다.

A 뷰컨트롤러의 Target Membership과 B 뷰컨트롤러의 Target Membership이 달랐습니다.<br>
그러니 B 뷰컨트롤러에서는 A 뷰컨트롤러에 접근을 할 수 없었던 것이죠.

아티클의 답변에서도 나왔듯, 보통을 뷰 컨트롤러를 만들때 자동으로 target Membership이 설정되는데, 이 새로운 파일에서는 이와같은 동작이 되지 않은 것입니다. <br>
이를 해결하기 위해서는 아래와 같이 **Target Membership** 에서 직접 설정해주면 간단하게 해결 됩니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Target/target10.png" alt="" width="50%">
</figure>
</center>



따라서 타겟을 분리해 사용할 때에는 이렇게 Target Membership을 한번씩 확인해 주는 습관!! 꼭 가지자! 
