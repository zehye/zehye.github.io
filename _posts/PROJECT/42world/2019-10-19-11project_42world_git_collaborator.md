---
layout: post
title: 세번째 프로젝트, 깃헙으로 협업!(헷갈리는부분 정리)
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

아래의 글을 참고하여 작성하였습니다.

- [GitHub를 이용하여 협력하기](https://developer.ibm.com/kr/developer-기술-포럼/2018/02/05/github-collaboration/)
- [GitHub로 협업하는 방법](https://gmlwjd9405.github.io/2017/10/27/how-to-collaborate-on-GitHub-1.html)


## github

오랜만에 다른 친구들과 협업하여 프로젝트를 진행하다보니, 처음 깃 초기셋팅부터 헷갈리는 부분이 수두룩했다. 나중에 시간이 지나도 나는 분명히 헷갈릴것이 분명하였기에 처음부터 차근차근 정리를 해놓기로!

우선 [GitHub](https://github.com/)은 회원에게 git 저장소를 제공하는 온라인 서비스 중 하나이다. 개인 회원에게 퍼블릭 저장소를 무료로 제공하며, 프라이빗 저장소의 경우에는 요금을 받고 있는데 요즘에는 프라이빗도 경우에따라 요금을 받지 않는 것도 있는것 같다. 퍼블릭 저장소인 경우, 외부에 공개되며 검색과 함께 소스를 볼 수 있다.


### Git Collaborators

깃으로는 협업이 매우 간편하게 이루어지는데, 그 중 하나의 방법으로 `Collaborators`가 있다.

저장소의 소유자는 필요한 경우 해당 저장소의 코드와 관리를 할 수 있는 다른 사용자를 Collaborators로 등록할 수 있다. `Settings` 탭 내 `Collaborators`항목을 선택하면 해당 저장소에서 같이 협업할 수 있는 사용자를 추가하고 관리할 수 있다. (사용자의 email주소 혹은 이름으로 검색이 가능하다.) 이렇게 협업을 요청하면 해당 사용자가 요청을 받아들이기를 기다리면 된다.

협업 요청을 받는 사용자는 알림이 뜨게 되고 알림을 들어가보면 어떤 사용자로부터 어떤 저장소에 대해 협업을 요청받았는지, 이를 수락할 것인지에 대한 내용을 보여준다. 이렇게 Collaborators로 등록된 사용자는 해당 저장소의 파일을 직접 수정할 수 있는 권한을 받게된다.


### Fork & Pull request

깃헙이 가진 재밋는 기능 중 하나는 `fork`라는 기능이다. 이는 해당 저장소에 등록된 협업 사용자가 아니더라도 해당 저장소를 `fork`하면 자신의 저장소로 복제가 가능한 기능이다.(블로그의 퍼오기와 비슷한 기능이다.) 이렇게 자신의 저장소로 가지고 온 후, 이를 수정하고 다시 fork한 원본 저장소에 반영해달라고 요청하는 기능이 있는데 이를 `Pull request`라고 한다. 이를 `PR`이라고도 부른다.


## Feature Branch Workflow

- 기능별 브랜치를 만들어서 작업
- 다수의 구성원이 메인코드 베이스(master)를 중심으로 새로은 기능 개발 가능
- 유연성이 큰 협업 방법
- 소규모 인원의 프로젝트에서 사용하는 협업 방법


### organization으로 협업하기

```
# 첫번째 방법
git clone [중앙 remote repository URL]

# 두번째 방법
git init
git remote add origin [중앙 remote repository URL]
git fetch origin master
```

- fetch: 원격 저장소의 데이터를 로컬에 가져오기만 하기
  - 단순히 원격 저장소의 내용을 확인만 하고 로컬 데이터와의 병합은 하고싶지 않을때 사용
- pull: 원격 저장소의 내용을 가져와 자동으로 병합 작업을 실행 `(fetch+merge)`


### Branch

```
# 첫번째 방법
git checkout -b [branch name]

# 두번째 방법
git branch [branch name]
git ckechout [branch name]
```

```
git commit -a -m "first commit"

git push -u origin [branch name]
# 혹은
git push -origin [branch name]

git checkout master
git pull origin master
```
