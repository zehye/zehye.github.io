---
layout: post
title: m1 맥북 초기 설정 정리해보기
category: etc
tags: [interview]
comments: true
---

> m1 맥북 구매 후 초기 설정해야할 내용을 정리해보았습니다.         
필요한 내용은 계속해서 추가해 나갈 예정이며 더 좋은 방법이 있거나 잘못된 부분이 있다면 알려주세요! :)

<hr>

### Homebrew 설치하기

m1 맥북 용 homebrew 설치 명령어

```
/bin/bash -c "$(curl -fsSL https://gist.githubusercontent.com/nrubin29/bea5aa83e8dfa91370fe83b62dad6dfa/raw/48f48f7fef21abb308e129a80b3214c2538fc611/homebrew_m1.sh)"
```

아래는 기존 homebrew 설치 명령어

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

만약 기존과 같이 위 코드로 homebrew를 설치하고 `brew help`와 같은 명령어를 터미널에 입력해보면 아래와 같은 문구가 나타날 것이다.

```
zsh: command not found: brew
```

터미널 커맨드 창에서 brew라는 명령어를 찾을 수 없다는 의미로 이럴때는 아래 명령어를 터미널에 입력!

```
eval $(/opt/homebrew/bin/brew shellenv)
```
그러면 `brew help`와 같은 명령어를 입력했을 때 아래와 같은 창이 나옴

<center>
<figure>
<img src="/assets/post-img/etc/3.png" alt="" width="50%">
</figure>
</center>


### brew cask

- brew ~ : 커맨드 라인 프로그램 (c, java, python 같은..)
- brew cask ~ : GUI 프로그램 (Safari, Chrome, Word 같은..)

1. brew update : 홈브류 최신버전으로 업데이트
2. brew upgrade [프로그램명]: 홈브류에 설치된 프로그램 최선버전으로 업데이트
3. brew search [프로그램명] : 홈브류를 통해 설치 가능한 프로그램 찾기

예전에는 `brew cask` 이런식으로 명령어를 사용해서 앱을 깔았는데, 이제는 이렇게 안해도 되는것 같다. (그래서 나는 그냥 진행했다....ㅎ) 사용하려면 `brew install --cask` 사용!

**brew cask 는 2.7.0 업데이트부터 명령이 비활성화 되었다. 2020-12-01 부터 사용 불가.**


### Git 설치

최신버전의 git으로 설치하는 명령어
```
brew install -s git
```

git이 제대로 깔려있는 지 확인하고 싶다면 터미널에 `git`이라고 쳐본다.<br>
아래와 같은 화면이 보인다면 잘 깔려있는 것

<center>
<figure>
<img src="/assets/post-img/etc/3.png" alt="" width="50%">
</figure>
</center>


### Mac SSH 생성 및 Github 등록

맥북에 SSH 키가 있는지 확인

```
cat ~/.ssh/id_rsa.pub
```
키가 있다면 해당 키가 터미널 창에 보일 것이다. 없다면 아래와 같은 메시지가 뜬다.

```
No suck file or directory
```

그러면 이제 생성해보자.

```
ssh-keygen
```

위 명령어를 입력하면 터미널에서 경로와 파일명을 설정하라는 메시지가 뜰것이고 나는 그냥 여기서 추천해준(지정해준) 대로 enter 누르며 진행헀다. 잘 생성되어있는지 다시 확인해보자.

```
cat id_rsa.pub
```

그러면 터미널창에서 내 SSH 키가 보일 것이고 이를 복사해놓은 상태로 Github으로 고고

1. Github 홈페이지에 들어간다
2. 오른쪽 상단 내 계정을 눌러 settings > SSH and GPG keys 탭 선택
3. New SSH key 클릭
4. 타이틀은 아무거나 상관없지만, 보통 사용하는 기기 이름을 적어줍니다.
5. 복사한 id_rsa.pub을 붙여넣기!
6. 저장


### git config

터미널에서 git configuration을 진행합니다.

```
git config --global user.name "지정할 이름"
git config --global user.email "지정할 이메일"
```


### iterm2 설치

```
brew install iterm2
```
사실 개발환경에서는 에러 메시지가 영어로 나오는것이 훨씬 좋기때문에(검색에 용이...) 맥북 언어 설정 자체를 영어를 우선으로 두는게 좋다. 그럼에도 만약 한글을 사용하게 되어 혹시나 한글이 깨진다면 `⌘ + , -> profile -> text -> unicode -> form을 NFC로 변경`을 하면 된다고 한다.



### zsh, oh-my-zsh

```
// zsh 설치
brew install zsh

// oh-my-zsh 설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```


### zsh 관련 설치

```
// zsh-autosuggestions > 터미널 입력 시 history 기반 단어를 추천
brew install zsh-autosuggestions
```
설치 진행 후 ~/.zshrc에 아래 명령어를 추가

```
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

```
// zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git "$HOME/.zsh/zsh-syntax-highlighting"
```
설치 진행 후 ~/.zshrc에 아래 명령어를 추가

```
echo "\nsource $HOME/.zsh/zsh-syntax-highlighting/zsh-syntax- highlighting.zsh" >> "$HOME/.zshrc"
```

이렇게 설치를 하고나면 터미널을 나갔다 다시 들어와도 되고 `source ~/.zshrc`를 하면 설치한 내용들이 적용되어있는 것을 볼 수 있을 것이다.



### Node.js 설치

```
brew install node
```


### 이외 설치해볼 것들

- brew instlal google-chrome
- brew install vlc
- brew install atom
- brew install slack



### 맥 시간 설정 안되어있을 경우

`환경설정 > 날짜 및 시간 > 좌물쇠 풀고 > 시간대 설정 변경해주기`



### iterm 테마 꾸미기

플러그인 설치도 할 수 있지만 나는 이런거엔 관심이 별로 없기에..

`환경설정 들어가서 바꿔주기`


### iterm 상태바 보여주기

iterm configuration 에 들어가서 <br>
`profile > session > Status bar enabled 체크 > Configure Status Bar 선택해 원하는 항목 추가 저장하고 나와서 Appearance > Status bar location으로 상태바 위치 설정`



### 맥북 대략적인 초기 기본 설정

- **mission control** > Automatically rearrange Spaces based on most recent use > 체크 안함: 최근에 만진 화면 순서대로 미션 컨트롤 창 순서가 바뀌지 않도록 설정
- 언어는 무조건 **영어가 우선순위** 가 될 수 있도록 (선택사항이긴 함): locale 설정때문에 오류가 발생하는 걸 방지해주고 영어 오류 메시지가 구글 검색에 더 용이함
- 패스워드, 분실대비 스크린 메시지 설정, 디스크 암호화 등
- **Keyboard > text** > 자동변경 옵션, 입력한 단어 변경 등 위에서부터 3개 다 체크 해제
  - Correct spelling automatically 체크 해제
  - Capitalize words automatically 체크 해제
  - Add period with double-space 체크 해제
  - Use smart quotes and dashes 반드시 해제해주기!
- **Keyboard > shortcut** > Use Keyboard navigation to move focus between controls: 키보드로 예/아니오 설정할 수 있는건데 이건 선택사항임
- **TrickPad** > tap to click 선택!: 트릭패드 꾹 안누르고 터치만으로 클릭 가능하게 하는것
- **Accessibility** > Pointer Control > Trackpad options > Enable dragging > three finger drag: 세 손가락으로 드래그 가능하게 함 > 요건 신세계였음
  - 근데 이 방법을 사용하면 원래 세 손가락으로 미션컨트롤 이동을 했는데, 이젠 네손가락으로 이동을 하게 됨(선택은 자유!)
- **finder preference** > show all filename extensions 체크함: 모든 파일의 확장자를 보여줌




### cocoapods 설치

- 터미널복제 > 터미널2의 get info > use rosetta
- 터미널2에서 `sudo gem install ffi`
- iterm2로 돌아와서 `brew install cocoapods`
