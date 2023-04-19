---
layout: post
title: 다이어리 앱 만들기 - 기본 구조 만들기
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## 프로젝트 기본 구조

1주동안 정리된 상황

```
├── Diary
│   ├── Diary.xcdatamodeld
│   │   └── Diary.xcdatamodel
│   │       └── contents
│   ├── Managers
│   ├── Models
│   │   ├── BaseModel.swift
│   │   └── ModelDiary.swift
│   ├── Storyboards
│   │   ├── Base.lproj
│   │   │   ├── LaunchScreen.storyboard
│   │   │   └── Main.storyboard
│   │   ├── Common.storyboard
│   │   └── Diary.storyboard
│   ├── SupportingFiles
│   │   ├── AppDelegate.swift
│   │   ├── Assets.xcassets
│   │   │   ├── AppIcon.appiconset
│   │   │   │   └── Contents.json
│   │   │   └── Contents.json
│   │   ├── Defines.swift
│   │   ├── Fonts
│   │   ├── Info.plist
│   │   └── SceneDelegate.swift
│   └── ViewControllers
│       ├── BaseVC.swift
│       ├── Calendar
│       │   ├── CalendarVC.swift
│       │   └── CalendarVCswift
│       ├── Common
│       │   ├── Alert
│       │   │   └── AlertController.swift
│       │   └── TextInput
│       │       └── TextInputVC.swift
│       ├── Diary
│       │   ├── DiaryTableCell.swift
│       │   └── DiaryVC.swift
│       ├── FullCalendar
│       │   └── FullCalendarVC.swift
│       ├── Plan
│       │   └── PlanVC.swift
│       ├── SearchBarViewController.swift
│       └── ViewController.swift
```
