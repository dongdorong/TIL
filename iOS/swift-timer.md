# Timer
- 0 ++ 타이머
- Hours / Minutes / Seconds 로 제작하기 과제! [링크 참고](https://medium.com/ios-os-x-development/build-an-stopwatch-with-swift-3-0-c7040818a10f)
- Timer.scheduledTimer:: 정해진 시간이 지나면 메서드를 호출
	1. timeInterval:: 몇 초가 지나면 메서드를 호출할지 정의 - 1초로 정의
	2. target:: Timer가 어디에서 사용되는지 정의 - ViewController에서 사용되기 때문에 self
	3. selector:: 타이머가 1초 지나면 어떤 메서드를 부를지 정의 - countTime 정의
	4. userInfo:: 넘어감
	5. repeats:: selector가 정한 메서드가 불러지는 상태
		+ true:: 1초가 지날때마다 호출
		+ false:: 1번만 호출


```swift
import UIKit

class ViewController: UIViewController {
    @IBOutlet weak var countNum: UILabel!

    var seconds = 0
    
    var timer = Timer()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }ㄴ    
    @objc func countTime() {
        print("1 Second ++")
        seconds += 1
        countNum.text = "\(seconds)"
    }

    @IBAction func start(_ sender: UIButton) {
        timer = Timer.scheduledTimer(timeInterval: 1, target: self, selector: #selector(ViewController.countTime), userInfo: nil, repeats: true)
    
    }
    
    @IBAction func stop(_ sender: UIButton) {
        timer.invalidate()
        seconds = 0
        countNum.text = "\(seconds)"
        
    }
    

}
```
