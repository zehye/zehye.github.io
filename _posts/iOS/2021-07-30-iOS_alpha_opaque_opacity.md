---
layout: post
title: alpha, opaque, opacity의 차이란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## alpha, opaque, opacity

**1. alpha**: 0.0 ~ 1.0 범위의 값으로 0.0은 완전하게 투명, 1.0은 완전하게 불투명

**2. isOpaque**: view가 투명한지 불투명한지 결정하는 Bool값.
  - true: drawing 시스템이 view를 완전하게 불투명하게 처리 > 성능향상에 좋음
  - false: 투명하게 처리

**3. opacity**: 불투명도 0.0(투명) ~ 1.0(불투명)범위내에 있어야 하며, 해당 범위는 벗어나는 값은 최소값 또는 최대값으로 고정.
  - 기본값은 1.0이며 alpha와의 차이는 alpha는 UIView에, opacity는 CALayer의 프로퍼티이다.
