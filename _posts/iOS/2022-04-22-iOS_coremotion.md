---
layout: post
title: iOS CoreMotion 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## CoreMotion

걸음수 데이터를 가져오기 위한 방법은 두가지가 있습니다.

1. Coremotion
2. HealthKit

그치만 걸음수 데이터를 무엇을 위해, 어떻게 가져올지에 따라서 둘 중 한가지 방법을 선택해야 합니다. 

우선 Coremotion은 실시간 걸음 데이터를 제공합니다. <br>
일반적인 만보기 어플을 구현하기 위해서는 보통 coremotion을 사용합니다.

반면 Healthkit은 애플에서 제공해주는 건강앱에 표시되는 모든 건강데이터들을 그대로 가져옵니다.

즉, 실시간으로 걸음수 데이터를 수집할 목적이라면 coremotion을 사용하고 굳이 실시간 데이터가 필요하지않다면 healthkit을 사용해도 괜찮습니다. 

이번에는 실시간으로 걸음수 데이터를 가져오는 coremotion 사용 방법에 대해서 설명해보겠습니다.


### Info.plist

우선 걸음수 데이터를 가져오기 위해 유저에게 접근권한요청을 해야합니다.<br>
plist에 다음과 같은 설정을 추가해줍니다.

```swift
NSMotionUsageDescription = 걸음수 데이터 측정을 위해 데이터 접근 권한이 필요합니다.
```

앞서 HealthKit 때도 말씀드렸듯이 이때 접근권한 메시지는 꼭 디테일하게 잘 적어줍시다! 추후 리젝사유가 됩니다.


### code

```swift 
import UIKit
import CoreMotion

class HomeViewController: UIViewController {
    let activityManager = CMMotionActivityManager()
    let pedoMeter = CMPedometer()
    var stepData = 0

    override func viewDidLoad() {
        super.viewDidLoad()
        updateStep()
    }

    func updateStep() {
        if CMPedometer.isStepCountingAvailable() {
            self.pedoMeter.queryPedometerData(from: date.startOfDay(), to: date.endOfDay()) { (date, error) in
                if error == nil {
                    if let response = date {
                        DispatchQueue.main.async {
                            self.stepData = Int(truncating: response.numberOfSteps)
                            
                        }
                    }
                }
            }
        }
    }
}
```