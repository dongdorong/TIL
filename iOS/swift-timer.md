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

<img src="/img/timer.png" width="300">


### 숫자가 ++ 카운트
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

### 제대로 Timer 만들기 중
- Hours / Minutes / Seconds 에 format과 제약 등등 잘 생각해서 제작해야함!! [링크 참고](https://www.youtube.com/watch?v=82SXeAmZwk8)


```swift
import UIKit

class ViewController: UIViewController {
    @IBOutlet weak var hoursCount: UILabel!
    @IBOutlet weak var minutesCount: UILabel!
    @IBOutlet weak var secondsCount: UILabel!
    
    var hours = 0
    var minutes = 0
    var seconds = 0

    var timer = Timer()
    
    var isTimerRunning = false
    
    var resumeTapped = false

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    // Timer 초기화
    func runTimer() {
        timer = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(ViewController.updateTimer), userInfo: nil, repeats: true)
    }
    
    @objc func updateTimer() {
        print("1 Second ++")
        seconds += 1
        secondsCount.text = "\(seconds)"
    }
    
    
    @IBAction func reset(_ sender: UIButton) {
        timer.invalidate()
        seconds = 0
        secondsCount.text = "\(seconds)"
    }
    
    @IBAction func start(_ sender: UIButton) {
        runTimer()
    }
    
    
    @IBAction func stop(_ sender: UIButton) {
        if self.resumeTapped == false {
            timer.invalidate()
            self.resumeTapped = true
        }else{
            runTimer()
            self.resumeTapped = false
        }
        
    }
    
//    func timeString(time: TimeInterval) -> String {
//        let hours = Int(time) / 3600
//        let minutes = Int(time) / 60 % 60
//        let seconds = Int(time) % 60
//
//        return String(format: "%02i:%02i:%02i", hours, minutes, seconds)
//    }
    

}
```
