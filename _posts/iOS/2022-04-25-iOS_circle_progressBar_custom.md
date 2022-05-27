---
layout: post
title: iOS UIBezierPath를 사용해 custom circleProgressBar 만들어보기
category: iOS
tags: [iOS, UIBezierPath, circleProgressBar]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UIBezierPath를 사용해 custom circleProgressBar 만들어보기

우선 view 파일을 하나를 만들어줍니다. 

```swift 
import UIKit

class CircleProgressBarView: UIView {
    // 전체 원을 그려줄 layer
    private var circleLayer = CAShapeLayer()
    // 프로그래스 원을 그려줄 layer
    private var progressLayer = CAShapeLayer()
    
    // 원의 시작과 끝을 그려줄 변수 
    private var startPoint = CGFloat(-Double.pi / 2)
    private var endPoint = CGFloat(3 * Double.pi / 2)
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        createCirclePath()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
    }

    func createCirclePath() {
        let circlePath = UIBezierPath(arcCenter: CGPoint(x: frame.size.width / 2.0, y: frame.size.height / 2.0), radius: 120, startAngle: startPoint, endAngle: endPoint, clockwise: true)

        // 전체 원 setUI
        circleLayer.path = circlePath.cgPath
        circleLayer.fillColor = UIColor.clear.cgColor
        circleLayer.lineCap = .round
        circleLayer.lineWidth = 10.0
        circleLayer.strokeStart = 0.0
        circleLayer.strokeEnd = 1.0
        circleLayer.strokeColor = UIColor.lightGray.withAlphaComponent(0.2).cgColor
        
        layer.addSublayer(circleLayer)
        
        // 프로그래스 원 UI
        progressLayer.path = circlePath.cgPath
        progressLayer.fillColor = UIColor.clear.cgColor
        progressLayer.lineCap = .round
        progressLayer.lineWidth = 10.0
        progressLayer.strokeStart = 0
        progressLayer.strokeEnd = 0
        progressLayer.strokeColor = UIColor(named: "7A7BDA")?.cgColor
        
        layer.addSublayer(progressLayer)
    
    }

    // 프로그래스 원에 애니메이션을 주고싶다면 사용 
    func progressAnimation(duration: TimeInterval, value: Float, maxValue: Float) {
        let circleProgressAnimation = CABasicAnimation(keyPath: "strokeEnd")
    
        circleProgressAnimation.duration = duration
        circleProgressAnimation.fromValue = 0
        
        progressLayer.strokeStart = 0
        progressLayer.strokeEnd = CGFloat(value/maxValue)
        progressLayer.add(circleProgressAnimation, forKey: "progressAnim")
    }
}
```

일반적으로 progressAnimation함수에는 maxValue가 들어가있지 않은 예제만 있을 것이다.<br>
근데 저는 전체 원의 최대값을 만들어 그 최대값 대비 value를 넣어줄 예정이기에 인자를 하나 더 받았습니다.ㅎㅎ

이렇게 뷰를 만들었다면 이제 이 해당 뷰를 사용하는 곳에서 사용을 해줘야겠죠?

저는 테이블뷰 셀에 해당 뷰를 만들어주고 있는데, 그러다 보니 뷰의 .center가 잘 맞지 않는 이슈가 있었습니다.<br>
이를 해결하기 위해 snapKit을 사용해 뷰의 위치를 다시 잡아주었습니다. 


### HomeTableViewCell 

```swift 
import UIKit
import SnapKit

class ProgressTableViewCell: UITableViewCell {
    @IBOutlet weak var containerView: UIView!
    
    var circleProgressView: CircleProgressBarView!
    var circleDuration: TimeInterval = 2

    var stepData = Int()

    override func awakeFromNib() {
        super.awakeFromNib()

        setupCircleProgressBarView()
    }

    func setupCircleProgressBarView() {
        circleProgressView = CircleProgressBarView()
        self.containerView.addSubview(circleProgressView)

        circleProgressView.snp.makeConstraints { make in
            make.centerX.centerY.equalTo(self.containerView)
            make.top.equalTo(self.containerView).inset(165)
        }
        
        self.circleDuration = 2
        self.circleProgressView.progressAnimation(duration: circleDuration, value: Float(self.stepData), maxValue: 10000)
        
        self.containerView.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(self.handleTap)))
        self.circleProgressView.layoutIfNeeded()
    }
    
    @objc func handleTap() {
        self.circleDuration = 2
        self.circleProgressView.progressAnimation(duration: circleDuration, value: Float(self.stepData), maxValue: 10000)
    }
}
```


