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


# 제대로 Timer
- Minutes / Seconds 에 format과 제약 등등 잘 생각해서 제작!! [링크 참고](https://www.youtube.com/watch?v=82SXeAmZwk8)
- 버튼 이미지 변경
- 테이블 뷰에 랩 추가
- 숫자가 바뀔때마다 텍스트가 덜덜 떨리는 것들을 여러가지 테스트 해보며 고정시킴
- `테이블 뷰를 터치할때마다 timer가 정지되는 것 같이 보이는 문제 해결해야함! (다음과제)`
	+ RunLoop.current.add(timer, forMode: .commonModes):: 스크롤링을 해도 타이머가 계속 돌아가보이도록 함. runTimer 함수에 추가해줬음 [링크 참고](https://forums.developer.apple.com/thread/69387)

<img src="/img/dona_timer.png" width="300">

```swift
import UIKit

class ViewController: UIViewController, UITableViewDataSource {
    @IBOutlet weak var countLabel: UILabel!
    @IBOutlet weak var timeTable: UITableView!
    
    @IBOutlet weak var startStopButton: UIButton!
    @IBOutlet weak var lapResetButton: UIButton!
    
    var timer = Timer()
    
    var laps:[String] = []
    
    var minutes = 0
    var seconds = 0
    var fractions = 0
    
    var isTimerRunning = true
    var lapTapped = false

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // label 초기값 지정
        countLabel.text = "00:00.00"
        
    }
    
    // Timer 초기화
    func runTimer() {

        timer = Timer.scheduledTimer(timeInterval: 0.01, target: self, selector: #selector(ViewController.updateTimer), userInfo: nil, repeats: true)

	RunLoop.current.add(timer, forMode: .commonModes)

    }
    
    @objc func updateTimer() {
        fractions += 1
        
        if fractions == 100 {
            seconds += 1
            fractions = 0
        }
        
        if seconds == 60 {
            minutes += 1
            seconds = 0
        }
        
        let fractionsString = fractions > 9 ? "\(fractions)" : "0\(fractions)"
        let secondsString = seconds > 9 ? "\(seconds)" : "0\(seconds)"
        let minutesString = minutes > 9 ? "\(minutes)" : "0\(minutes)"
        
        countLabel.text = "\(minutesString):\(secondsString).\(fractionsString)"
        
    }
    
    @IBAction func startStop(_ sender: UIButton) {
        if isTimerRunning == true{
            runTimer()
            isTimerRunning = false
            
            // image change
            startStopButton.setImage(UIImage(named: "stop.png"), for: .normal)
            
            lapResetButton.setImage(UIImage(named: "lap"), for: .normal)
            
            lapTapped = true
            
        }else{
            timer.invalidate()
            isTimerRunning = true
            
            // image change
            startStopButton.setImage(UIImage(named: "start.png"), for: .normal)
            
            lapResetButton.setImage(UIImage(named: "reset.png"), for: .normal)
            
            lapTapped = false
        }
    }
    
    @IBAction func lapReset(_ sender: UIButton) {
        if lapTapped == true {
            
            laps.insert(countLabel.text!, at: 0) // 0번 인덱스에 텍스트 지정
            timeTable.reloadData()
            
        }else{
            
//            lapTapped = false // false 지정해 주는 방법도 있음
            
            lapResetButton.setImage(UIImage(named: "lap.png"), for: .normal)
            
            timer.invalidate()
            laps.removeAll()
            timeTable.reloadData()
            
            fractions = 0
            seconds = 0
            minutes = 0
            
            countLabel.text = "00:00.00"
            
        }
        
    }
    
    //table
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int{
        return laps.count
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
        let cell = UITableViewCell(style: .value1, reuseIdentifier: "Cell")
        
        cell.backgroundColor = self.view.backgroundColor
        
        cell.textLabel?.text = "Lap \(laps.count - indexPath.row)" // 전체 array 수 - 인덱스 row
        
        cell.detailTextLabel?.text = laps[indexPath.row]
        
        return cell
    }


}
```
