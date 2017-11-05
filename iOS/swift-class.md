# Class - 은행계좌

### Property (인스턴스 변수)

```swift
class Account {
    var owner: String   //문자열 property
    var balance: Double //소수형 property
}
```
### Mothods
- Class에서 사용하는 함수

#### 생성자 (init)
```swift
class Account {
    var owner: String  
    var balance: Double
    
    //생성자
    init(owner: String, balance: Double {
        self.owner = owner
        self.balance = balance
    }
    
}
```

#### 인스턴스 생성
```swift
var newAcoount = Account(owner: "dona", balance: 100000)
```

#### 인스턴스 값 받아오기
```swift
newAccount.owner    //dona
newAccount.balance  //100000
```

#### 일반 Mothods
```swift
func deposit(amount: Double) {
    self.balace += amount
}

func printAccount() -> String {
    return "\(owner)의 계좌 잔금은 \(balance) 입니다."
}
```

#### 추가
- property를 처음 정의할때 값을 지정 가능
- property 이름과 파라미터 이름이 겹치지 않으면 self 사용하지 않아도 된다.