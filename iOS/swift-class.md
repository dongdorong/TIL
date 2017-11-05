# Class - 은행계좌

### Property (인스턴스 변수)
- swift에서 porperty를 인스턴스 변수라 한다.

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
    init(owner: String, balance: Double) {
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

### 클래스 상속
- class (name): (상속 클래스)
- 새로 추가 해야하는 property가 있다면 init 사용. 겹치는 생성자는 super를 사용한다.
- 똑같은 property를 갖고 있다면 init 사용하지 않아도 된다.
- 상속 클래스의 메소드를 덮어 쓰려면 override.

```swift
class CheckingAccount: Account{
    //한도추가
    var limit: Double
    
    //생성자 초기화
    init(owner: String, balance: Double, limit: Double) {
        self.limit = limit
        super.init(owner: String, balance: Double)
    }
    
    func withdraw(amount: Double) -> Double {
        if amount > self.limit {
            print("한도초과")
            return self.balance
        }
        
        if self.balance - amount >= 0 {
            self.balance -= amount
        }else{
            print("잔액부족")
        }
        
        return self.balance
    }
}

```

```swift
class SavingsAccount: Account {
    let interestRate = 3.0
    
    func yieldBalance() {
        self.balance += self.balnce * interestRate / 100
    }
    
    //상속 클래스를 덮어 쓰기
    override func printAccount() -> String {
        return "\(owner)의 적금 계좌 입니다. 계좌 잔금은 \(balance) 입니다."
    }
    
}
```

### 다형성 (Polymorphism)
- 다양한 형태의 인스턴스가 같은 인터페이스를 제공하는 개념
- 상속 받은 클래스의 인스턴스로 사용 가능
- 배열에 추가 가능

```swift
var accounts: [Account] =[]

//인스턴스 생성
var newAccount  = Account(ownerName: "Dona", balance: 1000000)
var newCheckingAccount = CheckingAccount(ownerName: "Hana", balance: 5000, limit: 1000)
var newSavingsAccount = SavingsAccount(ownerName: "Poo", balance: 2500)

//배열에 추가
accounts.append(newAccount)
accounts.append(myCheckingAccount)
accounts.append(mySavingsAccount)

for account in accounts {
    account.deposit(amount: 10000)
    print("account.printAccount()") 
}
```

