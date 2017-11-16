# UserDefaults
- iOS의 영구 저장 클래스
- 간단한 저장을 해야 할때 (복잡한 저장은 coredata)

```swift
@IBOutlet weak var newName: UITextField!
@IBOutlet weak var savedName: UILabel!

@IBAction func saveAction(_ sender: UIButton) {
    savedName.text = newName.text!
    
    // UserDefaults에 영구 저장
    UserDefaults.standard.set(savedName.text!, forKey: "name")
}

override func viewDidLoad() {
    super.viewDidLoad()
    
    // UserDefaults에서 값 받아오기
    let stringObject = UserDefaults.standard.object(forKey: "name")
    
    // 존재하면 savedName.text에 지정
    // 존재하지 않으면 "None"을 표시
    if let name = stringObject as? String {
        savedName.text = name
    } else {
        savedName.text = "None"
}
}
```
- viewDidLoad:: 화면을 띄우기 전에 자동으로 불러와 지는 곳 (변수 초기값, 다른 초기값들을 설정)
- stringObject:: 어떤 타입이든 보관 가능한 Any? 옵셔널 타입 (옵셔널인 이유는:: 저장된 값이 없으면 nil이 리턴)
- stringObject은 옵셔널을 안전하게 풀어줘야한다.
- as?:: 형변환
