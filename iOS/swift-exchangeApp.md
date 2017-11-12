# 환전 어플 (Exchange App)
- 간단한 환전 어플을 만든다.
- 한국, 미국, 일본, 중국, 영국, 캐나다 환전 어플

<img src="/img/Exchange.png" width="300">

### 화면과 코드 연결 1
```swift
// 사용자가 입력하는 form
@IBOutlet weak var convertFrom: UITextField!

// 환전한 값
@IBOutlet weak var convertTo: UILabel!

// 끝 환전 세그먼트
@IBOutlet weak var chosenCurrency: UISegmentedControl!

// 시작 환전 세그먼트
@IBOutlet weak var chosenCurrentCurrency: UISegmentedControl!

// 시작 환전 TextField 옆 국가 표시
@IBOutlet weak var currencyFromLabel: UILabel!

// 끝 환전 TextField 국가 표시
@IBOutlet weak var currencyToLabel: UILabel!

// 시작 환전 국가 이미지 
@IBOutlet weak var convertFromFlag: UIImageView!

// 끝 환전 국가 이미지
@IBOutlet weak var convertToFlag: UIImageView!
```

### 환율 사전
```swift
let currencyRate = [
    "KRW" : 1,     // 한국 1원 = 1원
    "USD": 1163,   // 미국 1달러 = 1163원
    "JPY": 11,     // 일본 1엔 = 11원
    "RMB": 170,    // 중국 1위안 = 170원
    "GBP": 1463,   // 영국 1파운드 = 1463원
    "CAD": 863     // 캐나다 1달러 = 863원
]
```

### 국기 이미지 사전
```swift
// 국기 이미지 사전
let flagNames = [
    "KRW" : "south-korea.png",
    "USD": "united-states.png",
    "JPY": "japan.png",
    "RMB": "china.png",
    "GBP": "united-kingdom.png",
    "CAD": "canada.png"
]
```

### 화폐 선택
```swift
// 시작 화폐
@IBAction func currencyFromSelected(_ sender: UISegmentedControl) {
    currencyFromLabel.text = sender.titleForSegment(at: sender.selectedSegmentIndex)
        
    // 0으로 초기화
    convertTo.text = "0"
    convertFrom.text = "0"
        
    let filePath = flagNames[currencyFromLabel.text!]!
    convertFromFlag.image = UIImage(named: filePath)
}

// 끝 화폐
@IBAction func curreuncyToSelected(_ sender: UISegmentedControl) {
    currencyToLabel.text = sender.titleForSegment(at: sender.selectedSegmentIndex)
        
    // 0으로 초기화
    convertFrom.text = "0"
    convetTo.text = "0"
        
    let filePath = flagNames[currencyToLabel.text!]!
    convertToFlag.image = UIImage(named: filePath)
}
    
```
- Segmented Control을 선택할 때마다 convertFrom / convertTo text를 "0"으로 초기화
- currencyFromLabel / currencyToLabel의 값을 현재 내가 누른 Segmented값으로 변경하기 위해 **sender.titleForSegment(at: sender.selectedSegmentIndex)**를 사용한다.

#### sender.titleForSegment(at: sender.selectedSegmentIndex)
- 현재 내가 선택한 타이틀의 인덱스 값을 받아온다.

### 환전
```swift
// 환전
@IBAction func convertCurrency(_ sender: UIButton) {
    if let x = Int(convertFrom.text!){
        let currencyFrom = chosenCurrentCurrency.titleForSegment(at: chosenCurrentCurrency.selectedSegmentIndex)! // optinal
        let currencyTo = chosenCurrency.titleForSegment(at: chosenCurrency.selectedSegmentIndex)! // optinal
        convetTo.text = String(x * currencyRate[currencyFrom]! / currencyRate[currencyTo]!)
    }else{
        print("number only")
    }
}
```
- Label의 property [text]는 nil값을 갖고 있으므로 옵셔널타입이다.
- 옵셔널을 안전하게 풀어주는 방법을 사용해야한다.
- Segmented Control도 사전에 값이 없을 경우 nil의 값이 넘어 올 수 있으므로 **!** 옵셔널을 풀어줘서 사용한다.
- **currencyRate[currencyFrom]!** / **currencyRate[currencyTo]!** 은 사전에서 사용했기에 옵셔널을 풀어줘야 한다. 

### 환율 공식
```swift
환전할 값 * 해당 나라 환율 / 변경할 나라 환율
```