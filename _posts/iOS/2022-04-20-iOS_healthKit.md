---
layout: post
title: iOS HealthKit 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## HealthKit

걸음수 데이터를 가져오기 위한 방법은 두가지가 있습니다.

1. Coremotion
2. HealthKit

그치만 걸음수 데이터를 무엇을 위해, 어떻게 가져올지에 따라서 둘 중 한가지 방법을 선택해야 합니다. 

우선 Coremotion은 실시간 걸음 데이터를 제공합니다. <br>
일반적인 만보기 어플을 구현하기 위해서는 보통 coremotion을 사용합니다.

반면 Healthkit은 애플에서 제공해주는 건강앱에 표시되는 모든 건강데이터들을 그대로 가져옵니다.

즉, 실시간으로 걸음수 데이터를 수집할 목적이라면 coremotion을 사용하고 굳이 실시간 데이터가 필요하지않다면 healthkit을 사용해도 괜찮습니다. 

이번에는 실시간으로 걸음수 데이터를 가져오는 HealthKit 사용 방법에 대해서 설명해보겠습니다.


### Info.plist

우선 걸음수 데이터를 가져오기 위해 유저에게 접근권한요청을 해야합니다.<br>
plist에 다음과 같은 설정을 추가해줍니다.

```swift
NSHealthShareUsageDescription = 걸음수 데이터 측정을 위해 데이터 접근 권한이 필요합니다.
NSHealthUpdateUsageDescription = 걸음수 데이터 측정을 위해 데이터 접근 권한이 필요합니다.
```

이때 저 메시지.. 꼭 주의해서 작성하세요!<br>
저는 나중에 수정해야지.. 하고 당장 개발할때 대충 작성하곤 까먹고.. 그대로 앱 심사를 올렸더니 문구 수정하라는 코멘트와 함께 리젝을 당했거든요..ㅠ흑

첨부터.. 조심조심 할수있는건 하고 지나가는게 좋겠구나! 라는 교휸을 얻었습니다 ㅎ


### code

```swift 
import UIKit
import HealthKit

class StepModel: NSObject {
    var step: Double = 0.0
    var date: Date = Date()
    override init() {
        super.init()
    }
    convenience init(step: Double, date: Date) {
        self.init()
        self.step = step
        self.date = date
    }
}

class HomeViewController: UIViewController {
    let healthStore = HKHealthStore()
    
    let typeToShare = HKObjectType.quantityType(forIdentifier: .stepCount)
    let typeToRead = HKObjectType.quantityType(forIdentifier: .stepCount)
    
    var stepDataList: [String] = []
    var stepList: [StepModel] = [StepModel]()

    override func viewDidLoad() {
        super.viewDidLoad()
        configure()
        retrieveData()
    }

    func configure() {
        print("configure")
        if !HKHealthStore.isHealthDataAvailable() {
            setHealthData()
        } else {
            requestAuthorization()
        }
    }
    
    // 권한 요청 
    func requestAuthorization() {
        print("requestAuthorization")
        self.healthStore.requestAuthorization(toShare: Set([typeToShare!]), read: Set([typeToRead!])) { success, error in
            if error != nil {
                print(error?.localizedDescription as Any)
            } else {
                if success {
                    print("권한 승인 완료")
                } else {
                    print("권한 승인 실패")
                }
            }
        }
    }
    
    func setHealthData() {
        print("retrieveData")
        let calender = Calendar.current
        var interval = DateComponents()
        interval.day = 1
        
        var anchorComponents = calender.dateComponents([.day, .month, .year], from: NSDate() as Date)
        anchorComponents.hour = 0
        let anchorDate = calender.date(from: anchorComponents)
        
        let stepQuery = HKStatisticsCollectionQuery(quantityType: HKObjectType.quantityType(forIdentifier: .stepCount)!, quantitySamplePredicate: nil, options: .cumulativeSum, anchorDate: anchorDate!, intervalComponents: interval as DateComponents)
        stepQuery.initialResultsHandler = { query, results, error in
            let endDate = NSDate()
            // 오늘로부터 30일간의 걸음수 데이터를 가져오도록 진행 
            let startDate = calender.date(byAdding: .day, value: -30, to: endDate as Date, wrappingComponents: false)
            if let myResults = results {
                myResults.enumerateStatistics(from: startDate!, to: endDate as Date) { statistics, stop in
                    if let quantity = statistics.sumQuantity() {
                        let date = statistics.startDate
                        let steps = quantity.doubleValue(for: HKUnit.count())
                        self.stepDataList.append("\(date): \(steps)")
                        let model = StepModel(step: steps, date: date)
                        self.stepList.append(model)
                        DispatchQueue.main.async {

                        }
                    }
                }
            }
        }
        healthStore.execute(stepQuery)
    }

    func saveStepCount(stepValue: Int, date: Date, completion: @escaping (Error?) -> Void) {
        guard let stepCountType = HKQuantityType.quantityType(forIdentifier: .stepCount) else { return }
        let stepCountUnit: HKUnit = HKUnit.count()
        let stepCountQuantity = HKQuantity(unit: stepCountUnit, doubleValue: Double(stepValue))
        
        let stepCountSample = HKQuantitySample(type: stepCountType, quantity: stepCountQuantity, start: date, end: date)
        
        self.healthStore.save(stepCountSample) { (success, error) in
            if let error = error {
                completion(error)
                print("Error Saving Steps count Sample: \(error.localizedDescription)")
            } else {
                completion(nil)
                print("Successfully saves step count sample")
            }
        }
    }
}
```