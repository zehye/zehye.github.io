---
layout: post
title: iOS MKMagneticProgress를 사용해 circleProgressBar 만들어보기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

반원 짜리 모양을 가진 프로그래스 뷰를 만들고 싶었습니다.<br>
앞서 헀던 방식으로 직접 뷰를 그릴수도 있지만, 이번에는 라이브러리를 통해서 만들어보려고 합니다.


## MKMagneticProgress를 사용해 circleProgressBar 만들어보기 

[MKMagneticProgress](https://github.com/malkouz/MKMagneticProgress)

```swift 
import MKMagneticProgress

class HomeTableViewCell: UITableViewCell {
    @IBOutlet weak var circleProgressBar: MKMagneticProgress!

    override func awakeFromNib() {
        super.awakeFromNib()

        setProgressViewUI()
    }
    
    func setProgressViewUI() {
        circleProgressBar.setProgress(progress: 0)

        circleProgressBar.progressShapeColor = UIColor(named: "7A7BDA")!
        circleProgressBar.backgroundShapeColor = (UIColor(named: "FFFFFF")?.withAlphaComponent(0.1))!
        circleProgressBar.titleColor = UIColor.red
        circleProgressBar.percentColor = UIColor.white
        
        
        circleProgressBar.lineWidth = 16
        circleProgressBar.orientation = .bottom
        circleProgressBar.lineCap = .round
    }
}
```

무척 간단합니다.<br>
여기서 중요한 점은 setProgress에서 progress로 받는 숫자는 0~1사이의 숫자입니다.

전체를 1로 보고 그 사이의 숫자들을 표현해주고 있는 것을 의미합니다. <br>
따라서 해당하는 progress에 유의미한 그래프를 보여주고 싶다면 반드시 0과 1사이의 숫자로 표현해서 보여줘야함을 잊지말아야 합니다! 


