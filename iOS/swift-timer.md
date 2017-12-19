# Timer
- 0 ++ 타이머
- Hours / Minutes / Seconds 로 제작하기 과제! syntax: [링크 참고](https://medium.com/ios-os-x-development/build-an-stopwatch-with-swift-3-0-c7040818a10f)
- timeInterval 초 단위


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
    }
    
    func updateTimer() {
        timer = Timer.scheduledTimer(timeInterval: 1, target: self, selector: (#selector(setter: ViewController.countNum)), userInfo: nil, repeats: true)
    }
    
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