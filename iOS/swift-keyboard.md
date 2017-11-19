# Keyboard
```swift
override func viewDidLoad() {
    super.viewDidLoad()
   
    let tap: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(ViewController.dismissKeyboard))
    
    view.addGestureRecognizer(tap)
}

... (생략)

// dismissKeyboard 메서드
@objc func dismissKeyboard() {
    self.view.endEditing(true)
}

```
- UITapGestureRecognizer:: 사용자의 터치 감지
- dismissKeyboard 메서드 호출
- self.view.endEditing(true) 실행하여 키보드를 없애준다.
