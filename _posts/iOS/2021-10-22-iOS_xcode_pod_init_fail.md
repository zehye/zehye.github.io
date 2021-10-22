---
layout: post
title: iOS Xcode13에서 pod init 실패하는 경우 해결방법!
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Pod init의 실패???

Xcode13으로 프로젝트를 생성 후 `pod init`을 하려하니 아래와 같은 이상한 에러가 발생한다.

```vim
――― MARKDOWN TEMPLATE ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――

### Command

/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/bin/pod init


### Report

* What did you do?

* What did you expect to happen?

* What happened instead?


### Stack

   CocoaPods : 1.10.1
        Ruby : ruby 3.0.2p107 (2021-07-07 revision 0db68f0233) [arm64-darwin20]
    RubyGems : 3.2.22
        Host : macOS 11.4 (20F71)
       Xcode : 13.0 (13A233)
         Git : git version 2.32.0
Ruby lib dir : /opt/homebrew/Cellar/ruby/3.0.2/lib
Repositories : trunk - CDN - https://cdn.cocoapods.org/


### Plugins

cocoapods-deintegrate : 1.0.4
cocoapods-plugins     : 1.0.0
cocoapods-search      : 1.0.0
cocoapods-trunk       : 1.5.0
cocoapods-try         : 1.2.0

### Error

RuntimeError - [Xcodeproj] Unknown object version.
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/xcodeproj-1.19.0/lib/xcodeproj/project.rb:227:in `initialize_from_file'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/xcodeproj-1.19.0/lib/xcodeproj/project.rb:112:in `open'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/cocoapods-1.10.1/lib/cocoapods/command/init.rb:41:in `validate!'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/claide-1.0.3/lib/claide/command.rb:333:in `run'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/cocoapods-1.10.1/lib/cocoapods/command.rb:52:in `run'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/gems/cocoapods-1.10.1/bin/pod:55:in `<top (required)>'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/bin/pod:23:in `load'
/opt/homebrew/Cellar/cocoapods/1.10.1_1/libexec/bin/pod:23:in `<main>'

――― TEMPLATE END ――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――

[!] Oh no, an error occurred.

Search for existing GitHub issues similar to yours:
https://github.com/CocoaPods/CocoaPods/search?q=%5BXcodeproj%5D+Unknown+object+version.&type=Issues

If none exists, create a ticket, with the template displayed above, on:
https://github.com/CocoaPods/CocoaPods/issues/new

Be sure to first read the contributing guide for details on how to properly submit a ticket:
https://github.com/CocoaPods/CocoaPods/blob/master/CONTRIBUTING.md

Don't forget to anonymize any private data!

Looking for related issues on cocoapods/cocoapods...
 - RuntimeError - [Xcodeproj] Unknown object version.
   https://github.com/CocoaPods/CocoaPods/issues/10984 [closed] [12 comments]
   5 days ago

 - Unknown object version
   https://github.com/CocoaPods/CocoaPods/issues/10973 [closed] [10 comments]
   3 weeks ago

 - RuntimeError - [Xcodeproj] Unknown object version.
   https://github.com/CocoaPods/CocoaPods/issues/7458 [closed] [21 comments]
   2 weeks ago

and 70 more at:
https://github.com/cocoapods/cocoapods/search?q=%5BXcodeproj%5D%20Unknown%20object%20version.&type=Issues&utf8=✓
```

처음보는 엄청난 에러창에 조금 놀랐지만, 해당 에러는 Xcode13 버그인걸로 보인다. <br>
이를 해결하는 방법은 간단하다. 두가지 방법이 있는데...



### 첫번째 해결방법

터미널 콘솔에 아래 코드를 입력해준다.

```vim
sudo xcode-select -s /Application/Xcode-beta.app/Contents/Developer
```



### 두번째 해결방법

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/25.png" alt="" width="50%">
</figure>
</center>

위 사진처럼 `Project Document`의 `Project Format`을 변경해준다.

- Xcode 12.0-Compatible로 변경해주면 됩니다.


위 두 방법 중 하나를 선택해 `pod init`을 진행하면 해결완료!
