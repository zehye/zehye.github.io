---
layout: post
title: 다이어리 앱 만들기 - 코코아팟 에러 해결하기
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

협업을 하던 중 아래와 같은 에러가 발생했다.

```
❯ pod install
Analyzing dependencies

――― MARKDOWN TEMPLATE ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――

### Command


/Applications/CocoaPods.app/Contents/Resources/bundle/bin/pod install


### Report

* What did you do?

* What did you expect to happen?

* What happened instead?


### Stack


   CocoaPods : 1.5.2
        Ruby : ruby 2.2.6p396 (2016-11-15 revision 56800) [x86_64-darwin15]
    RubyGems : 2.6.8
        Host : Mac OS X 10.15.5 (19F101)
       Xcode : 11.4.1 (11E503a)
         Git : git version 2.6.2
Ruby lib dir : /Applications/CocoaPods.app/Contents/Resources/bundle/lib
Repositories : master - https://github.com/CocoaPods/Specs.git @ 5d78ecf0af5854a0306fdb130cae7e5ebb95fcc5


### Plugins


cocoapods-check           : 1.0.0
cocoapods-deintegrate     : 1.0.2
cocoapods-plugins         : 1.0.0
cocoapods-plugins-install : 0.0.1
cocoapods-search          : 1.0.0
cocoapods-stats           : 1.0.0
cocoapods-trunk           : 1.3.0
cocoapods-try             : 1.1.0


### Podfile


# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'Diary' do
    # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
    use_frameworks!
    pod 'FSCalendar'
    pod 'CalendarLib'

    pod 'GrowingTextView', '0.7.2'
    # 키보드 매니저
    pod 'IQKeyboardManagerSwift'
end


### Error

RuntimeError - [Xcodeproj] Unknown object version.
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/xcodeproj-1.5.8/lib/xcodeproj/project.rb:218:in `initialize_from_file'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/xcodeproj-1.5.8/lib/xcodeproj/project.rb:103:in `open'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer/analyzer.rb:918:in `block (2 levels) in inspect_targets_to_integrate'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer/analyzer.rb:917:in `each'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer/analyzer.rb:917:in `block in inspect_targets_to_integrate'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/user_interface.rb:64:in `section'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer/analyzer.rb:912:in `inspect_targets_to_integrate'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer/analyzer.rb:78:in `analyze'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer.rb:243:in `analyze'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer.rb:154:in `block in resolve_dependencies'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/user_interface.rb:64:in `section'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer.rb:153:in `resolve_dependencies'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/installer.rb:116:in `install!'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/command/install.rb:41:in `run'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/claide-1.0.2/lib/claide/command.rb:334:in `run'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/lib/cocoapods/command.rb:52:in `run'
/Applications/CocoaPods.app/Contents/Resources/bundle/lib/ruby/gems/2.2.0/gems/cocoapods-1.5.2/bin/pod:55:in `<top (required)>'
/Applications/CocoaPods.app/Contents/Resources/bundle/bin/pod:22:in `load'
/Applications/CocoaPods.app/Contents/Resources/bundle/bin/pod:22:in `<main>'


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
 - Unknown object version -- 53
   https://github.com/CocoaPods/CocoaPods/issues/9815 [closed] [8 comments]
   22 May 2020

 - Pod Not Install
   https://github.com/CocoaPods/CocoaPods/issues/9698 [closed] [6 comments]
   a week ago

 - cannot execute pod install
   https://github.com/CocoaPods/CocoaPods/issues/9801 [closed] [6 comments]
   21 May 2020

and 54 more at:
https://github.com/cocoapods/cocoapods/search?q=%5BXcodeproj%5D%20Unknown%20object%20version.&type=Issues&utf8=✓

[!] [!] Xcodeproj doesn't know about the following attributes {"inputFileListPaths"=>["${PODS_ROOT}/Target Support Files/Pods-Diary/Pods-Diary-frameworks-${CONFIGURATION}-input-files.xcfilelist"], "outputFileListPaths"=>["${PODS_ROOT}/Target Support Files/Pods-Diary/Pods-Diary-frameworks-${CONFIGURATION}-output-files.xcfilelist"]} for the 'PBXShellScriptBuildPhase' isa.
If this attribute was generated by Xcode please file an issue: https://github.com/CocoaPods/Xcodeproj/issues/new
```

에러 내용이 상당히 장황해보이지만, 찾아보니 코코아팟 버전문제인 것 같았다.<br>
그래서 `sudo gem install cocoapods` > `pod setup`을 하였음에도 불구하고 `pod --version`을 통해 본 버전은 변하질 않더라.

그래서 그냥 해당 프로젝트에서 코코아팟을 삭제하기로 했다.


### 코코아팟 삭제하기

```
> sudo gem install cocoapods-deintegrate cocoapods-clean

Successfully installed cocoapods-deintegrate-1.0.4
Parsing documentation for cocoapods-deintegrate-1.0.4
Done installing documentation for cocoapods-deintegrate after 0 seconds
Successfully installed cocoapods-clean-0.0.1
Parsing documentation for cocoapods-clean-0.0.1
Done installing documentation for cocoapods-clean after 0 seconds
2 gems installed
```

```
> pod deintegrate

Deintegrating `Diary.xcodeproj`
Deleted 1 'Check Pods Manifest.lock' build phases.
Deleted 1 'Embed Pods Frameworks' build phases.
- Pods_Diary.framework
- Pods-Diary.debug.xcconfig
- Pods-Diary.release.xcconfig
Deleted 1 empty `Pods` groups from project.
Deleted 1 empty `Frameworks` groups from project.
Removing `Pods` directory.

Project has been deintegrated. No traces of CocoaPods left in project.
Note: The workspace referencing the Pods project still remains.
```

```
> pod cache clean --all
> rm Podfile
```


그러고 `pod --version`으로 확인해보니 업데이트된 버전의 코코아팟이 깔려있었다. <br>
이후 문제없이 install, update 진행 완료!
