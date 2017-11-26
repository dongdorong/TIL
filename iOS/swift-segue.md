# Segue (세그웨이)
- 한 화면에 다른 화면으로 넘어가는 기능

<img src="/img/segue.png" width="500">

### prepare methods
#### Fisrt ViewController
```swift
@IBOutlet weak var nameLable: UITextField!

override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if segue.identifier == "goToSecondViewController" { // segue가 goToSecondViewController가 맞다면
        let secondViewController = segue.destination as! SecondViewController
        secondViewController.name = nameLable.text! // secondViewController화면의 name변수를 nameLable.text!로 지정
    }
}
```

#### SecondViewController
- button으로 Action Segue:: 버튼으로 다음 화면 show 지정
- iOS에서 segue하기 전에 자동으로 불러와 주는 메서드
- parameter로 받는 segue가 실제로 적용된 것을 사용
```swift
// name을 받을 변수 지정
var name = ""
@IBOutlet weak var secondNameLable: UILabel!

override func viewDidLoad() {
    super.viewDidLoad()
    
    secondNameLable.text = name
}
```

### 어떤 값을 확인 한 후 segue하는 방법 (performSegue)
- 맞는 값을 확인한 후 segue하려 할 때 사용
- segue하기 전에 실행시킬 코드가 있을 때 사용

#### FirstViewController
- viewController로 Manual Segue:: 다음 화면 Show 
- performSegue::  넘어가고 싶은 segue의 identifier를 적용 -> 조건이 맞으면 해당 화면으로 이동
```swift
// UITextField
@IBOutlet weak var pwLabel: UITextField!
    
@IBAction func checkAndGo(_ sender: UIButton) {
    if pwLabel.text! == "test"{
        performSegue(withIdentifier: "goToSecond", sender: nil)
    }else{
        print("error your password!")
    }
}
```
